---
title: The first time I upgraded a PostgresSQL database
author: Karl Yngve Lervåg
date: 2025-08-18T01:00:00.000Z
---
Upgrading a live PostgreSQL database in a Kubernetes environment can be a daunting task.
Earlier this year, I faced this exact challenge: migrating an important PostgreSQL 15 database to version 16.
As this was the first time I tackled an upgrade of this nature, I realized I would gain more from the experience by documenting it.
Additionally, I hope to share insights that might help others navigate similar upgrades.

Fortunately, I collaborated with a knowledgeable colleague who was already well-versed in both PostgreSQL database maintenance and Kubernetes.
His support was invaluable—thanks [Robin](https://kaveland.no/)!

Now, the plan was simple:

1. Take a dump of the old database from the Kubernetes pod.
2. Perform the upgrade locally.
3. Upload the upgraded files to the Kubernetes pod.
4. Update our deployment scripts to run with the upgraded database files.

## Preliminary Work

To do this job, I needed both PostgreSQL 15, PostgreSQL 16, and [`kubectl`](https://kubernetes.io/docs/reference/kubectl/) installed on my system.
Thus, having a tool like [`mise`](https://mise.jdx.dev/) really comes in handy, as it makes it very easy to retrieve tools on demand[^1].
For instance, to do some very quick initial testing, I did:

```sh
mise shell postgres@15.5
initdb -D tmp
pg_ctl start -D tmp
pg_ctl stop -D tmp
```

## First try

Spoiler alert: I needed two tries.

### Dump of the old database

I first tried performing a [`pg_basebackup`](https://www.postgresql.org/docs/current/app-pgbasebackup.html) against the old PostgreSQL server via port forwarding:

```sh
# Port forward between local 5432 and remote
kubectl port-forward service/my-db 5432

# Test that it works
psql --host localhost --port 5432 --user postgres postgres

# Take backup
pg_basebackup --host localhost --port 5432 --user postgres -D dump -Fp -Xs -P --checkpoint fast
```

As the docs explain, `pg_basebackup` is used to take a base backup of a running PostgreSQL database cluster.
The backup is taken without affecting other clients of the database.
It really feels like the natural tool here.

However, the process failed with a "connection reset"-type error.
It must be some issue with networking or buffering – it's a bit unclear.
I don't remember the specifics of this failure, since I figured I could just copy the files manually.
So, I next tried to copy the old database files with `kubectl`:

```sh
kubectl cp my-service-db/my-db-pg15-0:/var/lib/postgresql/data/pgdata -c postgres db-old
```

I tested that I could start a server with the dumped files locally:

```sh
mise shell postgres@15.5
pg_ctl start -D db-old # failed first time!

rm db-old/postmaster.pid
pg_ctl start -D db-old # now it worked

pg_ctl stop -D db-old
```

It worked, except that I found I had to remove the `postmaster.pid` file before running `pg_ctl start`.
This was necessary because I had copied the files from a running database server in which the `postmaster.pid` is present, and PostgreSQL won't start if it is present.

### Upgrading the database locally

To upgrade the database locally, I used [`pg_upgrade`](https://www.postgresql.org/docs/current/pgupgrade.html), like this:

```sh
# Create a new DB that we will upgrade into
mise use postgres@16.6
initdb -D db-new -U postgres

# Upgrade
pg_upgrade -d db-old -D db-new \
  -b /home/lervag/.local/share/mise/installs/postgres/15.5/bin \
  -B /home/lervag/.local/share/mise/installs/postgres/16.6/bin
```

Then I copied the upgrade files back onto the Kubernetes pod:

```sh
kubectl cp db-new my-service-db/my-db-pg15-0:/var/lib/postgresql/data/pgdata-new -c postgres
```

### Upgrading the deployment scripts

Once the new files were in place, I went in and updated our `deployment.yml`.
I upgraded to 16.6 and instructed it to start from `pgdata-new`.

This seemingly worked fine.
However, I observed two things that were not working as expected:

1. When I look in the logs, I saw messages like

   ```
   WARNING:  database "postgres" has no actual collation version, but
             a version was recorded
   ```

   This happened because I performed the upgrade on a computer with `glibc`.
   The PostgreSQL server runs in Kubernetes on an Alpine Linux pod with `musl libc`.

2. When I tried to test by deploying to an environment in our project, I encountered a connection error.
   This turned out to be due to settings in both `pg_hba.conf` and `postgresql.conf` in the old version that were not carried over during the upgrade.
   The problem was resolved by fixing the configuration.

Still, it was not satisfying to have the warnings in our logs.
And we decided to try again with a clean slate.

## Second attempt

### Now with pg_dump

The next idea was based on [`pg_dump`](https://www.postgresql.org/docs/current/app-pgdump.html), which is another utility for backing up a PostgreSQL database.
According to the docs, it makes consistent backups even if the database is being used concurrently and it does not block other users.
The main difference is that `pg_dump` only dumps a single database and it does not include global objects such as roles.
Further, `pg_dump` extracts the database into an SQL script file, possible compressed and archived, while `pg_basebackup` creates a physical byte-for-byte backup of the entire database cluster's files.

So, we created a new plan:

1. Use `pg_dump` to obtain a logical backup.

   ```sh
   kubectl port-forward service/my-db 5432
   pg_dump -C -U postgres -h localhost -p 5432 -Zgzip:9 --if-exists -c mydbname > db15.sql.gz
   ```

2. Create a new fresh `StatefulSet` with PostgreSQL 16.
3. Add missing users with correct passwords, as well as grants:

   ```sql
   CREATE USER user1 WITH PASSWORD 'user1';
   CREATE USER user2 WITH PASSWORD 'user2';

   GRANT CONNECT ON mydbname TO user1;
   GRANT CONNECT ON mydbname TO user2;
   ```

   This is necessary, because as mentioned above, `pg_dump` does not include global objects such as roles.

4. Restore from the dump.

   ```sh
   kubectl port-forward service/my-db 5432
   mise shell postgres@16.6
   zcat db15.sql.gz | psql -U postgres -h localhost -p 5432 postgres -v ON_ERROR_STOP=1
   ```

I followed the plan, except I used `kubectl cp` to copy the dump into Kubernetes and then ran the `psql` command locally in the shell there.
It worked fine!

## Conclusions

All in all, I found this work to be interesting and I learned a lot from doing it.
The main things I want to remember:

- I should not create the new, updated database on a _different_ system from where it will be running.
  Understanding the environment, such as library differences between systems, is crucial!
- `pg_dump` proved more reliable in our scenario compared to `pg_basebackup`, especially when dealing with different system architectures.
- Working alongside a knowledgeable colleague accelerated the learning process and helped navigate unexpected challenges.

Overall, these insights not only facilitated a successful upgrade but also equipped me with strategies for future infrastructure challenges.

As a final remark, I should mention I just noticed [docker-pgautoupgrade](https://github.com/pgautoupgrade/docker-pgautoupgrade).
It looks awesome, and I'll definitely look into it the next time I need to do a PostgreSQL upgrade!

[^1]: See also [The tools that I love: mise-en-place](/posts/mise) for more reasons why I like `mise`!


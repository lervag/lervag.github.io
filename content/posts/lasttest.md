---
title: 'Load testing of "Søkerportalen"'
author: Karl Yngve Lervåg
date: 2025-02-20
_build:
  list: 'never'
---

![Samordna opptak](https://www.samordnaopptak.no/vrtx/dist/resources/images/logo.svg#floatright)

The last couple of years I've been working in a team [at Sikt](https://sikt.no/en/home) on the admission services for Norwegian universities and colleges[^1] [^2].
As part of this work, we've been performing load tests of [the applicant portal](https://sok.samordnaopptak.no/).
The first times my team and I did this, we followed a well-defined manual procedure that were executed two times each year.
We figured it would be relatively easy to automate this and have the load tests run daily in our pipelines.

So, last year, [I gave a talk at JavaZone 2024 in Oslo](https://2024.javazone.no/program/16de8fc2-4e54-4432-977a-e4400de271b1) where I presented our journey from doing manual load tests twice a year to having them automated.
I highlighted the main obstacles and talked about our experiences.
The talk was recorded and is freely available on the JavaZone websites.
However, the talk is in Norwegian, and I figured it would be nice to share the highlights in this short blog post.

## The journey

As mentioned, we started out performing a manual load-test process.
This was a process that had usually been performed twice a year up until and including 2023.
Essentially, our load testing procedure looked like this:

* Our load tests are written in Scala with [Gatling](https://gatling.io).
* We booked time with our operations staff and ensured that our test environment was ready and available at a given date a week or two in advance.
* We ensured that the tests ran as expected locally.
  Since it might be half a year since the last time anyone had run the tests, it was to be expected that we needed to fix a few things.
* During and after the load testing we wrote a report of our findings.

One of the main findings in my teams first report from June 2023 was that it should be relatively straightforward to automating the entire thing.
This was particularly true due to a parallel effort in which we had gotten much more control of our own pipelines and run time environments.
In this parallel effort we had also established an observability platform with open telemetry and Grafana.

So, to make true on my promise of keeping it short: We automated the thing.

* We used a custom Kubernetes environment on which we ran our load tests.
* The load tests are now run by scheduled pipelines once every day.
* The test results are available in Grafana dashboards.
  The main data source is from Prometheus, which gives us metrics on the response times for our APIs.

## Experiences

The main question after doing something like this: "Was it worth the time?"

![Is it worth the time](https://imgs.xkcd.com/comics/is_it_worth_the_time_2x.png)

Well, I think it definitely was!
In our case, a lot of relevant work was already being done in parallel.
Thus the costs were kept small.
Our main savings is that we no longer need to spend up to a week twice a year for doing this work.
But there are also other important benefits:

* Since we have daily results, it becomes much easier to react and fix problems that occur during the regular development cycle.
* We get a much stronger certainty on our load test results when we measure the aggregated results over a period of time.

Now, before I stop, I want to make an honourable mention of [Robin Kåveland](https://kaveland.no/) who was our expert.

* I would not advice doing work like this without having someone on team that has expertise in doing platform work, e.g. writing pipelines and Kubernetes configurations.
  In our case, 

## Addendum

[^1]: https://sikt.no/tiltak/fremtidens-opptak
[^2]: https://www.samordnaopptak.no/info/english/

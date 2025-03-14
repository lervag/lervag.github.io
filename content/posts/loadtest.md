---
title: Load testing of "Søkerportalen"
author: Karl Yngve Lervåg
date: 2025-02-21T00:00:00.000Z
---

{{< figure
  src="https://www.samordnaopptak.no/vrtx/dist/resources/images/logo.svg"
  alt="Samordna opptak"
  link="https://www.samordnaopptak.no/"
  class="so-logo"
>}}

The last couple of years I've been working in a team [at Sikt](https://sikt.no/en/home) on the admission services for Norwegian universities and colleges[^1] [^2].
As part of this work, we've been performing load tests of [the applicant portal](https://sok.samordnaopptak.no/).
The first times my team and I did this, we followed a well-defined manual procedure that were executed two times each year.
We figured it would be relatively easy to automate this and have the load tests run daily in our pipelines.

So, last year, [I gave a talk at JavaZone 2024 in Oslo](https://2024.javazone.no/program/16de8fc2-4e54-4432-977a-e4400de271b1) where I presented our journey from doing manual load tests twice a year to having them automated.
I highlighted the main obstacles and talked about our experiences.
The talk was recorded and is freely available on the JavaZone websites.
I was very happy to see so many people attending the talk, and [Kode24 even wrote a short story about it afterwards](https://www.kode24.no/artikkel/samordna-opptak-automatiserte-lasttesting-veldig-deilig/81904967) — thanks!
However, the talk is in Norwegian, and I figured it would be nice to share the highlights in this short blog post.

> ### 💡 Load test
>
> Load testing is the process of putting demand on a structure or system and measuring its response.[^3]

## The journey

The journey started when we first performed the mentioned manual load-test process.
This was a process that had usually been performed twice a year up until and including 2023.
We had a test suite written with [Gatling](https://gatling.io) in Scala that simulated load on our system.
Essentially, our load testing procedure looked like this:

- We booked time with our operations staff and ensured that our test environment was ready and available at a given date a week or two in advance.
- We ensured that the tests ran as expected locally.
  Since it might be 4-8 months since the last time anyone had run the tests, it was to be expected that we needed to fix a few things.
- During and after the load testing we wrote a report of our findings.
- The main data sources were the reports generated by Gatling and some observability metrics provided by our operations staff.

{{< figure
  src="/static/lasttest-gatling-report.png"
  alt="Gatling report"
  caption="An example of a Gatling report."
>}}

One of the main findings in my teams first report from June 2023 was that it should be relatively straightforward to automating the entire thing.
This was particularly true due to a parallel effort in which we had gotten much more control of our own pipelines and runtime environments.
In this parallel effort we had also established an observability platform with OpenTelemetry and Grafana.
This allowed us to measure the load on our environment by considering metrics such as response times for the APIs.

So, to make true on my promise of keeping it short: We automated the thing.

- We created a pipeline to run the load tests on a schedule - every day.
- The pipeline creates an ephemeral Kubernetes environment in which we ran our load tests.
- The test results are then readily available in our Grafana dashboards.
  The main data source is from Prometheus, which gives us metrics on the response times for our APIs.

{{< figure
  src="/static/grafana-aggregated.png"
  alt="Grafana"
  caption="Grafana plot of response times"
>}}

## Experiences

The main question after doing something like this:

{{< figure
  src="https://imgs.xkcd.com/comics/is_it_worth_the_time_2x.png"
  alt="Is it worth the time"
  caption="Is it worth the time (XKCD 1205)"
>}}

Well, I think it definitely was!
In our case, a lot of relevant work was already being done in parallel.
Thus the costs were kept relatively small.
And the savings are real: I estimate that the manual load tests required a week or perhaps even two each year.
But there are also other important benefits:

- We now have daily results of the load tests.
- Thus it becomes much easier to react and fix problems that occur during the regular development cycle.
- And having both the daily results, the Grafana dashboards, and the Gatling reports, we now have more and better tools for debugging in case we do observe regressions.

Finally, a couple of closing remarks:

- Working with Kubernetes environments and gitlab pipelines can be a hazzle.
  It was very useful to setup a local Kubernetes environment with [minikube](https://minikube.sigs.k8s.io/docs/) - this provided a much faster feedback loop.
- Doing this work would have been hard without having access to relevant competence.
  I would like to make an honourable mention of [Robin Kåveland](https://kaveland.no/), who made this possible!
  I've learned a lot from working with you; thanks!

[^1]: <https://sikt.no/tiltak/fremtidens-opptak>

[^2]: <https://www.samordnaopptak.no/info/english/>

[^3]: <https://en.wikipedia.org/wiki/Load_testing>


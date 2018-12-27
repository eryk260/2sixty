# ADR 0005: Application and Infrastructure metrics

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

There is a need to collect infrastructure and custom (app) metrics. Former for investigating issues (bottlenecks and areas to improve), latter for the purpose of scaling.

Viable solutions are to use a managed solution (stackdriver) or custom solution (prometheus + grafana)

## Decision

We will be using GCP Stackdriver since it gives us the features we need right now and requires no setup (GCP managed solution).

To give ourselves a way out from Stackdriver, we are going to use the prometheus_client for our applications to expose the metrics, instead of sending metrics directly to Stackdriver. To scrape the exposed metrics and send them to stackdriver, we are going to use https://github.com/GoogleCloudPlatform/k8s-stackdriver/tree/master/prometheus-to-sd.

## Consequences

If at a later date we ran into a major limitation of Stackdriver, we will be able to simply replace the prometheus-to-sd pod with a prometheus installation.

There is a higher direct cost to using stackdriver, but we don't need to manage the prometheus installation, don't need to worry about backups or scaling to cope with increased usage, etc...

Later on, if the collecting and storing capabilities of Stackdriver is suitable, but its graphing capability is not, we can introduce Grafana to offer more customisable dashboards to visualise the metrics from Stackdriver, but this will impose additional costs.

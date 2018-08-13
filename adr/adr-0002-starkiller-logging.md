<!-- File format adr/adr-0000-project-keyword.md -->

# ADR 0002: Picking a logging technology for Starkiller.

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Starkiller needs a centralised logging system so that we can reliable trace requests and events running through our
distributed system. There are two leading options; the (incumbent EFK stack)[https://essencedigital.atlassian.net/wiki/spaces/IO/pages/558170113/EFK]
and Stackdriver. Both have their advantages and disadvantages which we will try to list here (thanks to Csaba for this)

### EFK

#### Pro

- Essence already has this stack running
- Elastic search query language more widely known
- Kibana can visualise information from logs and supports dashboards

#### Con

- Requires VPC peering to access central fluentd/elasticsearch endpoints (until xpack is configured?!?)
- Managed by us

### Stackdriver

#### Pro

- Is widely available in Google Cloud and it's available for Google Kubernetes Engine as well. Requires no maintenance overhead from devops/sysops engineers and easier to (setup)[https://cloud.google.com/kubernetes-engine/docs/how-to/logging]
- Probably higher availability
- Probably cheaper
- With advanced filtering, you can observe cross-project logs
- Besides sending pod/container logs, kubernetes also sends:
  - Kubelet and container run-time logs
  - Logs for system components, such as VM startup scripts (Don't know if we would be able to send these to our EFK)
- (Logs-based metrics)[https://cloud.google.com/logging/docs/logs-based-metrics/]
  - Counter metrics
  - Distribution metrics
- Stackdriver used (log-based metrics)[https://cloud.google.com/logging/docs/logs-based-metrics/charts-and-alerts] to visualise logs
- Cloud functions logs are pushed to Stackdriver as well

#### Con

- Learning a different query language
- Logs are stored up to 30 days, if we require them for longer we need to export to either
  - Cloud storage
  - Cloud pub/sub
  - BigQuery
  - Custom destination

### Notes

- Both solutions support structured logging, we need to make sure all services are logging in the correct format. Fluentd has a library for logging in Python based services for example.
- Both solutions are near real time

## Decision

Given the above, Starkiller will use Stackdriver for its logging.

## Consequences

- Increases the observability of logging in Starkiller
- We can trace requests across all parts of our infrastructure, including Cloud functions

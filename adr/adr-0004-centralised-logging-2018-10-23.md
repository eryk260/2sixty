# ADR 0004: Application and Infrastructure logging

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

There is a need to log changes, issues, errors and potentials problems. This allows us to identify potential infrastructure issues and retrospectively investigate them.

Because of horizontal scaling, merging all the logs coming from the same kind of source requires the use of centralised log aggregations.

## Decision

We will be using GCP Stackdriver since it gives us the features we need right now and requires no setup (GCP managed solution).

## Consequences

As all other cloud resources, if used "abusevly" (meaning, we log everything, even things which are not relevant, or we forget to turn off debug logging for production environment) it can be significantly more expensive!

So, it requires planning of the logging to only log, which is absolutely necessary!

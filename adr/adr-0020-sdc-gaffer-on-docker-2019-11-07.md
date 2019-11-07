
# ADR 0020: Dockerise SDC forecasting (Gaffer) service

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

All of SDC's services are deployed and run on GCP App Engine Standard environment. As detailed in Google [App Engine feature](https://cloud.google.com/appengine/docs/the-appengine-environments) documentation, the standard environment has a limited request timeout of 60 seconds. This has until now been somewhat enough. With the planned increase in platform integration (Google Adwords, Facebook, Instagram etc) this limited timeout is expected to be a bottleneck for generating forecasts. This is especially the case when we have no control as to how long these 3rd party integration platforms take to run scenarios for forecasting.

App Engine was intended to be a cheap an effective way to get off the ground quickly, It is now a good time to start deploying our forecasting service (Gaffer) onto a more robust infrastructure without the limitations of App Engine.

## Decision

We will be dockerising the Gaffer service to be able to deploy on a separate SDC K8s cluster. This then allows us to scale services as and when we need to handle load and are not restricted by the App Engine limitation. 

## Consequences

- Increased cost (compared to App Engine)
- Increased maintenance and overhead (compared to App Engine)

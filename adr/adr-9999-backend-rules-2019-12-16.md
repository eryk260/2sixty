
# ADR XXXX: Rules for backend development

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

We have a growing number of backend developers and due to a lack of strong oversight strange decissions get made.

This is a list of rules that we demand dev teams stick to unless they speak to an architect and get approval for deviating.

It is not hard to think of scenarios where some of these rules do not apply, however when that happens it should trigger a discussion including architecure. 


## Decision

### Use most maintainable compute, (app engine > cloud run > GKE)
We want to avoid deployment headaches + devops work.

### Use HTTP
It is the most maintainable protocol

### Use most recent Python/ Java 11
Of course

### Don't user multithreading or multiprocessing 
We are not saying don't use a multithreaded framework, just don't use these yourself.

### Don't use async
Prove that you need it and that you can predict the memory and performance characteristics if you want to use it.

### If using a container, use the approved 260 Python container
If it exists...

### If building a Python HTTP server, use Flask + flask-rest-api + Marshmallow
Generate openapi.json from code

### Use the 3rd party integration pattern.
3rd party data is mirrored into a ‘local’ DB.
Works if 3rd party is down
Avoids throttling

### Datalab is a 3rd party service, as is Olive, VIE, ODP and any other data source outside of PaaS control.

### Environment is consistent state (persistence/config) , not GKE namespace GCP project.
‘Demo’ may be running on different projects, GKE clusters, Appengine instances etc.

### We can hit a version of a service at <version>.<service>.<environment>.api.2sixty.io
E.g. v1-2-3.facebook.demo.api.2sixty.io
Used for testing a specific version
Default version is latest 
E.g. facebook.demo.api.2sixty.io
Partial version names are supported
 V1-2.facebook.demo.api.2sixty.io
Can pin against particular version and get bug fixes

### Code dependencies are kept up to date with cicd
E.g. requirements are un pinned and checked for changes daily, rebuild is triggered on change
We do not pin library or language versions without specific reasons and even then, only for the shortest possible time.language

### We know the exact versions of all code dependencies that are in any version

### Openapi spec is built per version in cicd

### Use Cloudsql Postgres as default persistence

### Don’t use celery/RabbitMQ.Use Cloud PubSub or an equivalent (e.g. AWS SQS). 

### DB schemas + changes need to be approved by architecture. Schema definition + changes are written in SQL
### Relational databases must be designed to a minimum of 3NF. Denormalisation for performance reasons is a last resort solution, not a first response.
### Use HTTP 422 for data validation failures.
### Use X-Clacks-Overhead header
### Services all respond with their version in a header
### Use static analysis tools to verify that code us up to spec (sonarqube.2sixty.io, Python Black, pylint, flake8)
### No slugs
### No int ids? UUID4 for all ids everywhere, native column types in DB (not at text field!)

### Always UTC, no non timezone datetimes (will be UTC), ISO 8601

### Data is Json
### HTTP responses look like {“data”:<stuff>}


## Consequences

More discussions with architecture, less strange choices, increasingly consistent codebase. No loss of flexibility.

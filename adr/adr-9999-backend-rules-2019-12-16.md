
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

# Use most maintainable compute, (app engine > cloud run > GKE)
We want to avoid deployment headaches + devops work.


Use most maintainable protocols (HTTP is best)
Use most recent Python/ Java 11
Avoid multithreading or multiprocessing 
Avoid async
If using a container, use the approved 260 Python container
There is a process in place to keep this up to date 
If building HTTP server, use Flask + flask-rest-api + Marshmallow
Generate openapi.json from code
Use the 3rd party integration pattern.
3rd party data is mirrored into a ‘local’ DB.
Works if 3rd party is down
Avoids throttling
Olive is a 3rd party service
Environment is consistent state (persistence/config) , not GKE namespace GCP project.
‘Demo’ may be running on different projects, GKE clusters, Appengine instances etc.
We can hit a version of a service at <version>.<service>.<environment>.api.2sixty.io
E.g. v1-2-3.facebook.demo.api.2sixty.io
Used for testing a specific version
Default version is latest 
E.g. facebook.demo.api.2sixty.io
Partial version names are supported
 V1-2.facebook.demo.api.2sixty.io
Can pin against particular version and get bug fixes
Code dependencies are kept up to date with cicd
E.g. requirements are un pinned and checked for changes daily, rebuild is triggered on change
We know the exact versions of all code dependencies that are in any version


Not discussed

Datalab is a 3rd party service, as is Olive, VIE, ODP and any other data source outside of PaaS control.
Openapi spec is built per version in cicd
Use Cloudsql Postgres as default persistence
Don’t use celery/RabbitMQ.Use Cloud PubSub or an equivalent (e.g. AWS SQS). 
We do not pin library or language versions without specific reasons and even then, only for the shortest possible time.language
DB schemas + changes need to be approved by architecture. Schema definition + changes are written in SQL
Relational databases must be designed to a minimum of 3NF. Denormalisation for performance reasons is a last resort solution, not a first response.
Use HTTP 422 for data validation failures.
Use X-Clacks-Overhead header
Services all respond with their version in a header
All these rules apply unless stated otherwise by an architect (or another architect), if they dont the scope of the exception should be as limited as possible.
Python Black, pylint, flake8
Adhere as much as possible to QA standards when writing your code and use static analysis tools to verify that such standards are met (sonarqube.2sixty.io)


No slugs
No int ids? UUID4 for all ids everywhere, native uuid col in postgres. 


This applies to everyone
Always UTC no non timestamp datetimes, iso timestamps
Always in Json
{“data”:<stuff>} for every response






## Consequences

The consequences of the decision.

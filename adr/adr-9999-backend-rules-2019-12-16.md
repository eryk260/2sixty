
# ADR XXXX: Rules for backend development

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

We have a growing number of backend developers and due to a lack of strong oversight strange decissions get made. Sometimes for good reason, sometimes just temporarily, sometimes there is a better option.

This is a list of rules that we demand dev teams stick to unless they speak to an architect and get approval for deviating. 

This is a not a list of things you will never do, it is a list of scenarios where a discussion needs to happen.


## Decision

#### Use most maintainable compute, (app engine > cloud run > GKE)
We want to avoid deployment headaches + devops work. 

#### Use HTTP
It is the most maintainable protocol

#### Use most recent Python / Java 11
Of course

#### Don't user multithreading or multiprocessing 
We are not saying don't use a multithreaded framework, just don't use these yourself. In python at a minimum you need to be certain that our code and all libraries used are thread safe. (You can't test for this, concurrency problems appear occasionaly, and inconsistently.)

#### Don't use async
Prove that you need it and that you can predict the memory and performance characteristics if you want to use it.

#### If building a Python HTTP server, use Flask + flask-rest-api + Marshmallow
Generate openapi.json from code

#### Use the 3rd party integration pattern.
One service is responsible for mirroring 3rd party data is into a ‘local’ DB. Another service is responsible for serving requests that query the local DB. 
This works if 3rd party is down.
This gives us the best chance of avoiding throttling or rate limiting.

#### Datalab is a 3rd party service, as is Olive, ODP and any other data source outside of the platform.
Products and services built on the platform must keep working performantly if Datalab, Olive, ODP are down for maintenance or running slow.

#### Environment is consistent state (persistence/config), not GKE namespace GCP project.
‘Demo’ may be running on different projects, GKE clusters, Appengine instances etc. 

#### We can hit a version of a service at <version>.<service>.<environment>.api.2sixty.io
E.g. v1-2-3.facebook.demo.api.2sixty.io
Used for testing a specific version
Default version is latest 
E.g. facebook.demo.api.2sixty.io
Vanity version names are supported and can be moved to point at different versions
 edge.facebook.demo.api.2sixty.io
 

#### Code dependencies are kept up to date with cicd
E.g. requirements are un pinned and checked for changes daily, rebuild is triggered on change
We do not pin library or language versions without specific reasons and even then, only for the shortest possible time.

#### We know the exact versions of all code dependencies that are in any version
If version 1.2.3 of service_a has a bug, I need to know the exact version of every dependency that is running. If I deploy version 1.2.3 I must always get the same versions of dependencies. This can be done by pinning requirements during the build.

#### Openapi spec is built per version in cicd
Every version needs a spec, the spec needs to be guaranteed to be accurate. If using Flask + flask-rest-api + Marshmallow the spec can be built from the code. 

#### Use Cloudsql Postgres as default persistence
Postgres will be fine for nearly everything.

#### Don’t use celery/RabbitMQ. Use Cloud PubSub or an equivalent (e.g. AWS SQS).
Once less thing to go wrong.


#### DB schemas + changes need to be approved by architecture. Schema definition + changes are written in SQL
SQL is the correct language for describing databased and migrations. 


#### Relational databases must be designed to a minimum of 3NF. Denormalisation for performance reasons is a last resort solution, not a first response.
Basic hygiene 

#### Use HTTP 422 for data validation failures.
This will be handy for people trying to use your API.

#### Use X-Clacks-Overhead header
see https://xclacksoverhead.org/home/about

#### Services all respond with their version in a header
So someone can senf you the complete request and you can replicate it exactly, even if there has been a release since.

#### Use static analysis tools to verify that code us up to spec (sonarqube.2sixty.io, Python Black, pylint, flake8)
Just Yes. Exceptions are expected but also require justification.

#### No slugs
Leak information and assume it will always be unique.

#### No int ids. UUID4 for all ids everywhere, native column types in DB (not at text field!)
Int ids leak information. Also, this allows clients to choose the id for something they create, meaning they don't have to do a GET to get the id. Very handy when creating nested resources.

#### Always UTC, no non timezone datetimes (will be UTC), ISO 8601
Of course

#### Data over HTTP is Json

#### HTTP responses look like {“data”:<stuff>}



## Consequences

More discussions with architecture, less strange choices, increasingly consistent codebase. No loss of flexibility.

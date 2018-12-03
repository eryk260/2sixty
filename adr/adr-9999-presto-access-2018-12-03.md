<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0000: Direct Presto Access - Analytics Team

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Currently, Datalake's data is only accessible through Datalab (Looker). Analytics is requesting
programmatic access to the data so they can use it to build their products. Looker API does not
cover their needs as there isn't an available endpoint that runs queries through the SQL runner
and gives back the data.

Looker GCS scheduled exports have been activated as well but this adds an extra step and they do not feel
comfortable with it. Reasons given are that each time they would need like to request new/different data,
a new export would need to be scheduled. This left us with only one option which is to grant them a direct
Presto access.

Also, note that:
- We have currently 6 Presto clusters, all of them are hammering the same Dataproc Master Node.
- We have just a single instance of Hive Metastore (metadata) that lives inside the Master node,
  therefore is a single point of failure.
- There is no auth system (LDAP, Kerberos...) to connect to Presto.

Concerns are both: security and performance. There is a plan in process to mitigate all three
bullet points explained, but this is the situation for the time being. Best 'temporary' solution
found:

- Presto cluster read-only mode.
- New GCP project where just 2 Analytics team members have access to.
- Exposure of one of the cluster coordinators to this new project.

## Decision

Decision to make is simple. Do we give them access by the 'temporary' solution?
-

## Consequences

There will not be consequences of the decision. Has been already decided that for business reasons,
we should accept a minimum level of risk, however is highly important we share our thoughts here.

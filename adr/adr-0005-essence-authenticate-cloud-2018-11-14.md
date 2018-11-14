<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0000: Architecture decision record template

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Multiple requests across the business aligning which suggest that we need to move with migrating essence-authenticate (Keycloak) Staging and Development (Named UAT) environments to the cloud.
We will change architecture decisions from VM's to Kubernetes. 
This will also mean migrating Production environment to Kubernetes. There should be no downtime in this migration.

## Decision

Essence-Authenticate's (Keycloak) Staging and Development environments currently lives on at the Data Centre and infrastructure is maintained by IT

We will be doing the following:

- SQL Backend moved to CloudSql MYSQL 5.7 with failover node assigned.
- Quadrant Network assigned to the project (Shared VPC)
- Staging & Dev Environments moved to Kubernetes (Preemptible single node; node pool for Dev, Static single node; node pools for Staging)
- [Production](https://console.cloud.google.com/home/dashboard?organizationId=520597094442&project=essence-authenticate) migrated to Kubernetes from ComputeEngine instance groups (2 node; node pools non-preemptible in 2 regions for failover)
- KMS to hold secrets for DB and access
- Deployment CICD done through Gitlab CI file
- Repo to hold source code https://github.com/essence-tech/essence-keycloak and mirrored through Gitlab for CI Capabilities
- Ingress resource & controller used for directing Traffic instead of nginx-endpoints project.


Migrating Dev and Staging to the cloud will mean the deprication of:

authenticate-staging.essenceglobal.com
Host: ess-lon-keycloak-001s
Deployment: Puppet

authenticate-uat.essenceglobal.com (Used for Dev)
Host: ess-lon-keycloak-002s
Deployment: Puppet


## Consequences

Should not:

- Be any Downtime
- impact work
- impact other environments & services (As keycloak redirect is done through URL)

DO's

- Infrastructure as Code
- Testing with different realms for current services & products before roll out
- DB Backed up regularly
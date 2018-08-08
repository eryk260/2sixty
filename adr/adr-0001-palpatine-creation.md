# ADR 0001: Palpatine project creation.

## Status

**proposed**
~~accepted~~
~~rejected~~
~~deprecated~~
~~superseded~~

## Context

Multiple requests across the business aligning which suggest that we need to move with migrating Intern and its supporting
services to the cloud.

- APAC needs Intern externalised so that they can use it.
- Olive needs Intern to provide manual uploads.
- Starkiller will be using the same services as the one's Intern does.
- Deathstar is looking to merge with Starkiller.
- Deathstar already uses services that Intern uses.

This suggests that now is the right time to start the service mesh that provides API services to projects in the 2sixty
ecosystem. The mesh can then be extended to allow access to a cloud deployed Intern, Starkiller and Deathstar.

## Decision

Move the following services from the DC to a service mesh powered by Kubernetes and Istio

- [Intern](https://github.com/essence-tech/unpaid-intern)
- [Intern dash](https://github.com/essence-tech/unpaid-intern-dash)
- [Adwords](https://github.com/essence-tech/adwords)
- [Apple SEM](https://github.com/essence-tech/apple-search-ads)
- [DBM](https://github.com/essence-tech/dbm)
- [DS3](https://github.com/essence-tech/ds3)
- [Facebook](https://github.com/essence-tech/facebook)
- [LinkedIn](https://github.com/essence-tech/linkedin-data)
- [Snapchat](https://github.com/essence-tech/snapchat)
- [The Trade Desk](https://github.com/essence-tech/thetradedesk)
- [Twitter](https://github.com/essence-tech/twitter-data)

## Consequences

- Intern externalised for APAC and others
- Single place for development of API services across 2sixty, e.g. Intern, Deathstar and Starkiller can share the same AdWords API service
- Move out the DC onto cloud
- Might need to do the same for the [auth service](https://github.com/essence-tech/essence-auth)

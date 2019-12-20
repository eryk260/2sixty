# ADR 0000: Approach of migration Olive3 to python3

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context
In its current state Olive is using outdated libraries, outdated python version, is not ready for migration and this brings the following risks:
1. Security risk related to using unsupported libraries
2. Maintenance/release delay risk related to using unsupported libraries
3. Adoption of new libraries risk, more likely new python libs are not going to support python2

We've outlined 3 potential scenarions of migration to python3. As a prerequisite for this migration there are few steps that need to be done in advance:
1. Migrate codebase to be py3 ready (run python-modernize on existing codebase)
2. Set up py3 linters so new code changes are compatible with python2 and python3
3. Set up and use black as default code formatter for our codebase

As the next steps we need to choose one of the following scenarios:
![approach](./py3-approach-table.png)

First approach is more suitable when we have feature freeze and teams working on upgrade collectively.

Second approach is less risky and can go side by side with feature development. Also work on each step can be paralelized between multiple people.

Third approach is less risky and includes gradual releases and integration of py2 developmen branch into py3 branch. It's more suitable for longer term development and with less resources.

## Decision
We need to upgrade Olive to python 3. This will allows us to mitigate the mentioned risks and keep our product running and stay alive.

The approach that we choose is going to depend on business requirements. From our side we'd recommend approach 2 in case we can allocate more resources (4-5 BE engs) or approach 3 in case of less resources (1-2 BE engs)

## Consequences
We need to find resources for this migration.
We need to migrate codebase of big legacy product with as low impact to production as possible.

Cost of not doing:
* in case of unexpected security or downtime issue cost of 1 day of Olive downtime if very high (more than $XXX USD).

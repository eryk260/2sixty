<!-- File format adr/adr-9999-project-keyword-2019-04-04.md -->

# ADR 9999: Architecture decision record template

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Scalable Looker Security Model.

'Future Datalab' will require a lightweight (administrative) water-tight (data security) framework to govern Content, Data and Feature Access within a multi-instance, multi-tenant BI platform deployment.
It must be administrable via our Enterprise Identity and Access Management platform.
It must be have frictionless scalability.
Content Access control such that users can explore, find, view, organise, share, schedule and download appropriate Datalab BI assets.
https://docs.looker.com/admin-options/tutorials/permissions
'Appropriate' is defined as data that : 
    i) belongs to a users organisational space
    ii) the data belongs to a different organisation space but the user has reason and permission to access
    iii) the user has the appropriate permission level to access.

Data access control such that users 
https://docs.looker.com/admin-options/tutorials/permissions
Data Access, which controls which data a user is allowed to view. Data access is primarily managed via Model Sets, which make up one half of a Looker role. These roles are then applied to users and groups. Data access can be further restricted within a model using access filters to limit which rows of data they can see, as though there was an automatic filter on their queries. You can also restrict access to specific Explores, joins, views, or fields using access grants.

Feature Access, which controls the types of actions a user is allowed to do in Looker, including viewing data and saved content, changing the LookML models, administrating Looker and so forth.
https://docs.looker.com/admin-options/tutorials/permissions
Feature access is managed by Permission Sets, which make up the other half of a Looker role. Some of these permissions apply for the entire Looker instance, such as being able to see all schedules for sending data. Most of the permissions are applied to specific model sets, such as being able to see user-defined dashboards based on those models.
The data access, feature access, and content access for users and groups combine to specify what they can do and see in Looker.
sss
## Decision

The decision that we are making.

## Consequences

The consequences of the decision.

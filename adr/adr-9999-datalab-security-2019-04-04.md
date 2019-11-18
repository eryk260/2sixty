<!-- File format adr/adr-9999-project-keyword-2019-04-04.md -->

# ADR 9999: Architecture decision record template

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Scalable Looker Security Model.

ODP 'Future Datalab' will require a lightweight (administrative) water-tight (data security) framework to govern Content, Data and Feature Access.
ODP will be a multi-instance, multi-tenant BI SaaS platform. 
It must be administrable via our Enterprise Identity and Access Management platform.
It must be scalable.

Employ content access control such that users can:

Explore,
Find,
View,
Organise,
Share,
Schedule and 
Download appropriate Datalab BI assets. 
https://docs.looker.com/admin-options/tutorials/permissions

'Appropriate' is defined as data that : i) belongs to a users organisational space ii) the data belongs to a different organisation space but the user has reason and permission to access iii) the user has the appropriate permission level to access.

Data access control:
https://docs.looker.com/admin-options/tutorials/permissions

Data Access, which controls the data a user is allowed to view. Data access is primarily managed via Model Sets, which make up one half of a Looker role.
These roles can be applied to users and/or groups.
Data access can be further restricted within a model using access filters to limit which rows of data they can see, as though there was an automatic filter on their queries.
You can also restrict access to specific Explores, joins, views, or fields using access grants.

Feature Access, which controls the types of actions a user is allowed to do in Looker, including viewing data and saved content, changing the LookML models, administrating Looker and so forth. https://docs.looker.com/admin-options/tutorials/permissions Feature access is managed by Permission Sets, which make up the other half of a Looker role. Some of these permissions apply for the entire Looker instance, such as being able to see all schedules for sending data. Most of the permissions are applied to specific model sets, such as being able to see user-defined dashboards based on those models. The data access, feature access, and content access for users and groups combine to specify what they can do and see in Looker.

## Decision

Two types of groups are required:

1. Organisation Groups
Sets the organisation name
Associates all users from their organisation together
Keeps those users separate from other organisations (Cross-company data breach contamination)
Surfaces the ability to grant view access to Looker's "Shared Space" - a pre-requisite to setting up Organisational spaces
Defines global scope of model access since the Organisation group will be assigned an "Access" role. An access role is defined via a model set that comprises those models specific to that organisation. A Model will likely reflect a Domain "object".

2. Organisational Space Groups
Defines the role a user will have in their organisations space. A Space Group can either be "Space Editors" or "Space Views". Members of these groups can be controlled via the Space Admin Configuration section in Looker.
Note: A current (5th Sep, 2019 and double checked with Rich Kerrigan) constraint - Users can decide to share content with other users who share the same group. Consequently, a Space group must be defined for each and every organisation. This has been verified directly with Looker support.
User Type Roles.
Defines all roles available to Looker users E.G. Super Admin, Admin, Power User, User or Developer
Each Role is defined by a permission set that defines, in effect, Looker feature access for that role.

When a new user is on-boarded in Looker one should follow the following process:
Step 1: Assign the user to their respective Organisation Group.
Step 2: Assign the appropriate User Type Role to the user.
Step 3: Assign the user to their respective Organisation Space Group

Model Sets must be employed to control access to an Organisations domain logic which is modelled in Looker via Looker 'models'
Permission Sets contain policies that, when enabled, define the Looker features available to a User or Group of Users.
Both Model and Permission Sets are assigned to "Roles"
Roles can be assigned to Groups. Groups can be assigned to Roles. Roles can be assigned to individual Users.

Concerns:
Two concerns have been raised with Looker:

1. Beta Features e.g. Custom Fields require Groups the enable user access. Due to Looker's current behaviour of re: User content sharing for users in the same group - This has been identified as a high security gap. Looker have responded to say that the use of Beta Groups will be deprecated.
2. The manage_models permission is limited in scope and consequently provides Looker Instance-wide access to Users who have this permission. They can view other Looker Projects (Organisations) via the develop menu. Further information is available via the Looker issue sheet.

TODO:
What we will do?
How will we configure an IAM platform?
What SAML attributes will we translate?
What practice will we then follow for adding users; i.e. users get added to both client groups and some other group that defines their role type?

## Consequences

tbc

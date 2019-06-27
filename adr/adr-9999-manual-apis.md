# ADR-9999 Managing GCP Console Resources

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

There are resources in GCP for which no programmable API exists, so there is no
way to automate provision or management of these resources at least for the
time being.

Examples of these include:

- API & Services credentials
- Oauth2 client IDs and secrets

The creation of credentials is important as we make use of this feature in our
services, and also there are security implications around the access to this.

In order to administer credential resources it is necessary to have
'roles/editor' level permissions on the project. This is too broad a permission
to give out to all and sundry.

## Decision

The decision is to create a small Gsuite group, members of which will be
responsible for administration of this and any other identified resources.
The proposed members should include:

- 2 architects
- SRE team members

The group will be given the 'roles/editor' permission in a product's YAML
definition, either at product folder level or project level as appropriate.

These members will be able to service requests to add the necessary resources
manually. We should also consider keeping a 'live log' of the updates for
auditing purposes - this could be a GSuite sheet shared amongst the group
members with read-only allowed to certain outside users.

## Consequences

This will allow the administration of the resources while limiting as far as
possible the broad permissions required. By keeping the permissions of the
group in the product YAML files we also have auditable history of the group's
permissions against the projects, though of course not of the resources that
the group members have created or changed in GCP.

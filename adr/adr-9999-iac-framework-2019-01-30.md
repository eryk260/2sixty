<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: IaC Framework

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR is fundamental to enabling another ADR (SREs and DevOps); as a
pre-requisite to that ADR the workloads of DevOps engineers needs to be more
automated and this ADR is meant to provide that.

We are trying to bring all aspects of resource provisioning into a standard
Infrastructure-As-Code model. Without an IaC model:

- system state is either unknown or has to be manually logged (which is error
  prone even if done correctly).

- there is no way to *confidentally* recover a broken system, or recreate one
  from scratch

- updates can be ad-hoc and done without recording

- lacking full audit history

We already have some IaC solutions within the organisation, and there exists a
possibilty to use third-party solutions, however:

- each solution is non-standard, requiring each engineer to have particular
  knowledge

- it would be too much to ask users to learn how to use each solution, therefore
  DevOps engineers would have to take on all configuration work manually.

- 3rd party solutions would involve a certain amount of lock-in

## Decision

With IaC definitions held in version control we will be able to provide tooling
that works with the definitions to provide a self-service model to end users
who ask for resources. That tooling will also be part of the work covered under
this ADR.

We have decided to implement an IaC framework to help us provide a standardised
approach to implement IaC definitions and work flows. The framework will
provide software, services and documentation to meet the needs of DevOps and
end users of DevOps services. 

```
                +----------------------+-------------------------+
                |                      |                         |
                |                      |                         |
                |                      |                         |
 IaC Framework  |     ChatOps Bots     |        IaC Services     |
   Services     |                      |                         |
                |                      |                         |
                |                      |                         |
                |                      |                         |
+-------------------------------+------+--------+----------------+
                |               |               |                |
 Google Source  |               |               |                |
 Repositories   |    GitHub     |    Projects   |     ....       |
                |               |               |                |
                |               |               |                |
                +---------------+---------------+----------------+
```

- all IaC config will be stored in Google Source Repositories. Each
  supported feature will have its own repo.

- each repo contains a Python API module within itself in a standard location;
  the IaC Framework services manipulate the repo config only via this API. The
  API would also prove useful should we wish to implement other utilities e.g.
  a command line interface.

- a repo's API includes the ability to build and deploy itself into its
  target environments.

- each repo is configured to publish to one or more PubSub topics whenever it
  is updated. IaC Framework services are subscribers and can respond to these
  updates.

- the repos are manipulated by chat bots and some service modules. The chat
  bots provide a a user interface and a form of external history that
  complements the git log; e.g. the chat conversation between engineers might
  provide additional context to a configuration change.

- the service modules handle some messaging from pubsub and orchestrate the
  building and deployment of tagged versions, and report updates on builds to
  the bots.

- the chat bots use the repo APIs to update configuration in response to
  engineer requests e.g. ```github add user newguy to Jedi``` would update the
  GitHub IaC configuration appropriately.

- initial access to these chatbots will be limited to DevOps. In future we
  could allow a self-service model e.g. in GitHub, team leaders could have
  permission to add/remove members to their own teams via the chat bot.

In the short term we will focus on implementing two services for DevOps
engineers:

- GitHub IaC - this already exists in a MVP, but engineers work
  directly with the YAML file in the source repo.

- Projects - providing a UI (for the user) and back-end work flow to request,
  approve and provision projects in GCP in the new sixty-hyperspace model.

## Consequences

With an IaC framework in place:

- version controlled configuration

- auditable history of changes

- high confidence in understanding the current state of our systems

- roll-back facility e.g. undo changes and re-deploy

- repeatable e.g. ability to deploy exact replicas into test environments

- by using a chatops approach within a standardised framework we enable
  engineers to work more effectively by not having to worry about the data
  formats they are actually manipulating. 

- enable an ability to open up self-service facilities to end users which
  reduces the burden on SRE (or DevOps) resources.

- enforce a certain level of discipline into IaC; e.g. having to provide a
  programmable API makes engineers consider how usable their solution will be.
  
- as a result of more automation there is a greater need for DevOps engineers
  who are more experienced with development (esp in Python).

- no lock-in, and the API approach means other interfaces (e.g. CLI) could be 
  switched in and used instead or as well.


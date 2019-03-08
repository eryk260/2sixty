<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0010: DevOps and SRE - Operating model

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The current DevOps model Essence/2Sixty is using is unsustainable. The numbers of the developers and projects are going to increase in the future. The company won't be able to keep up with the demand to assign DevOps engineers to each development team.

## Decision

The following operating model will be implemented:

Form a new team called Site Reliabilty Engineers (SREs). Their responsibility will be to create and maintain the tools, which enable the developers to interact with GCP without the need of a dedicated and/or embedded DevOps engineer.

**Responsibilities of SRE team:**

- Managing the Infrastructure-as-Code framework, which allows the developers to create the necessary GCP resources, without the need to raise a service ticket and waiting for "someone" to fulfil the request.
- Managing the company's chosen CI/CD solution , raising support tickets with the provider (if CI/CD solution is managed), creating examples and filler documentation (for easier understanding) for developers, which help them get on speed quicker.
- Managing the Centralised Log Aggregation solution. Providing examples and documentation, how the developers will be able to ship, watch and archive their logs.
- Managing the Centralised Monitoring solution. Providing examples and documentation, how the developers will be able to create custom metrics (service level indicators), alerts (notifications of service level objective violation) and business contracts (service level agreements) for their applications.
- Create example application(s), CI/CD pipeline definition(s) and documentation how to use effectively the shared compute resources (mainly the GKE clusters) and the chosen CI/CD solution. This needs to include example kubernetes resources using a recommended templating language (for example helm) to show the developers, how they can deploy their applications's stateless part to the new GKE clusters.
- Create and maintain monitoring dashboards for the shared compute resources (mainly for GKE clusters) including cost, resource and status.
- Develop any new tool or process in the future, which prevents the SRE team becoming the bottleneck or failing to scale with a sudden increase of development projects/teams.

The last point is crucial, because it keeps the SRE team size under control.

**Responsibilities of Managed Service Provider (MSP) and/or Cloud Service Provider (CSP)**
- describe here
- describe here
- describe here

**Responsibilities of Developers:**

- Developing and maintaining the application's code base.
- Creating (and also removing) the GCP resources the developed application requires using the provided IaC tool.
- Using the provided examples, creating and maintaining the CI/CD pipeline (including test, build, deployment phases).
- Defining the application's service level indicators and service level objectives (monitoring and alerting).
- Complying with the Centralised Log Aggregation solution's requirements.

**NOTE**<br>
The above mentioned responsibilities can change if the product is not an in-house developed application, but a purchased one for example: Looks or Talend

**IMPORTANT**<br>
If the SRE team doesn't provide examples and documentation for a specific task, for example: bucket creation or how to support websocket connections within an ingress resource, then it will be the SREs responsibility to do the work or offer a solution!

## Consequences

- The development teams won't have embedded DevOps engineer(s), but they will need to step up and do some of the tasks a DevOps engineer would do (basically use the tools the SRE team develops and documents).
- If a development team cannot or doesn't want to do the DevOps tasks (because of the sheer amount of work or some other unforseen reasons), they will need to hire an embedded DevOps engineer (preferably a developer who wants to do some devops work as well) using the project's budget.

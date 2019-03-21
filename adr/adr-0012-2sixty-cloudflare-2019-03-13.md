<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0012: 2Sixty owned CloudFlare account

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Until recently 2Sixty has been regarded as a division within Essence.  As a result, the company CloudFlare account (which manages the DNS, WAF, DDOS protection, etc) is managed by Essence's IT department.  This prevents 2Sixty's SRE team from dealing directly with 2Sixty's domains.  It requires the SRE team to raise JIRA tickets or - in emergencies - skip the queue and talk directly with the IT department.

Because the SRE team doesn't have its own CloudFlare account, sometimes it's difficult for the SRE team to request exactly what is needed from IT, how to configure it and how to manage it.  This increases the likelihood of emergencies or, at least, sub-optimal configurations.

Consider a current example:  the SRE team is automating DNS zone modifications.  These modifications require CloudFlare's `Intelligent Failover and Load Balancing` as well as CloudFlare's API.  To effectively complete this work, the SRE team requires admin level API access, which as far as the team knows, cannot be scoped to a single domain (2sixty.io).  Additionally, were either the SRE team to make a configuration error, the error could affect not just 2Sixty, but Essence as well.

## Decision

2Sixty SRE shall register a new CloudFlare account and cooperate with Essence's IT department to organise the migration of the `2sixty.io` domain from Essence CloudFlare into newly owned and managed (and paid) 2Sixty CloudFlare.

The SRE team will provide admin access to Essence's Senior Director of Global Information Security and 2Sixty's security engineers.

## Consequences

__Pros__:

- 2Sixty SRE team will be able to manage 2Sixty's domains without Essence IT acting as an intermediary.
- In case of a mistake from the 2Sixty SRE, Essence CloudFlare settings would be be unaffected.

__Cons__:

- Essence's Senior Director of Global Information Security will loose the "Single-pane-of-Glass UI" on CloudFlare.
- Cost of CloudFlare account.
- Periodic account maintenance.

Cost implications:

- base cost is $20 per domain (initially we'll only have 2sixty.io)
- load balancing from
  - $5 for 2 origins - 1 website
  - $120 for 20 origins, faster health checks from 4 regions instead 1

If we need more than 10 websites or we have more than 2 origins per website, we'll need to contact sales!

Csaba Kolar, as the current SRE lead, weighs the pros over the cons.

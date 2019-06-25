<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: Shared Playground Projects

## Status

- [ ] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

We are currently on a journey - transitioning from a past of relative free-for-all GCP resource creation without operational management or structured financial control to a future of ~100% coverage of IAC-managed resources.

The implications of this future (which are already felt now for new projects managed by IAC) is that any sort of infrastructure change will entail an edit to 1 or more YAML files, a merge request raise, review and approval, and a deploy. With diligent error-free YAML edits, the feedback loop could be just a few minutes but this is still too long for more investigative style tasks such as:
- Understanding what permissions a new service requires.
- Thinking through the flow of a new POC in terms of resource, identity and permission requirements.

The feedback of a few minutes quickly becomes burdensome when encountering oversights or ommissions incrementally. In the slightly annoying [words of Raymond Hettinger](https://www.reddit.com/r/Python/comments/84ocgt/5_best_speeches_of_mr_raymond_hettinger/dvs30ji/), "there must be a better way ... there is!"

## Decision

We will create a single GCP playground project, *sixty-playground* where approved developers (developers who agree to usage conditions - more on this shortly) can create resources freely, outside of the IAC framework *for the purpose of investigation, not application testing*. This gives developers a place where they can play with new resource types / learn the parameters that an IAC module would require to administer this resource type and importantly, a very fast feedback loop.

This project would be wiped out on a daily basis @ 4AM UTC (time is sensitive to developer locations; 3AM/4AM London time, 5AM/6AM Lviv Time, 11PM/Midnight New York Time) so all resources / identities / permissions would be deleted in order to:
- Keep costs under control
- Discourage developers from developing dependencies in this environment / doing testing that they should be doing in their own IAC-managed test environment.

An alternative is one of these per project under the architect's responsibility but this is harder to maintain and more error-prone and so risk is increased.

## Consequences

We allow our GCP estate to be operationally managed and cost controlled everywhere except for 1 project which allows developers to gain fast feedback cycle on resource requirements, whilst adopting specific controls for this project exception to minimise risk of uncapped expenditure.

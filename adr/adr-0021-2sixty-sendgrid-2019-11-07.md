
# ADR 0021: Sendgrid as the default email sending platform for 2sixty products

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Numerous 2sixty products have a need to send emails. The solutions to this are limited and including setting us a custom mailserver, using an existing internal mail server or paying for a 3rd party service. The tech team working on each product has been left to decide what is the best approach. On some instances like Olive, the number of email sends has exceeded the quota (on the internal mail server). 

There is therefore a need for an alternative that does not impede on security and reliability. 

## Decision

We will be using a paid for Sendgrid service. This not only allows for the high send quotas olive and other products have, it is also a reliable, stable, and secure sending platform. Sendgrid also has a robust documentation for both account management and developer integration.

The Sendgrid account will be a 2sixty wide account to service all of 2sixty's tech products that need to send emails. Each product will have a separate API sending key and will be authorised to send from the following domains:

- @essenceglobal.com
- @2sixty.io

There will be minimal developer setup (if at all): [Python example](https://sendgrid.com/docs/for-developers/sending-email/v3-python-code-example/)

Additionally we will add the 30 day activity plan to the account to allow us to audit and filter email sends by API key. This will allow us to better account for quota usage for financial purposes.

## Consequences

- We will need to pay for the service
- All products share the same send quotas - although this shouldn't matter as the quota will just continue to increase as and when required
- Existing products that use a different email sending mechanism would need to be refactored if they wish to use Sendgrid



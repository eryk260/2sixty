# ADR 0003: Configuration and Secrets

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

We need to store sensitive (credentials, passwords, tokens, etc...) and non-sensitive informations (settings, variables, etc...) for application configurations.

## Decision

- We will store non-sensitive information/application configurations in the applications' git repositories.
- We will use k8s configmaps to store those configurations for the applications which need it. Either using environmental variables or configuration files.
- We will store sensitive informations in Vault.
- We will use CI/CD to reach out to Vault, grab the necessary information and save them in k8s as k8s secrets.
- Alternatively, if we implemented a GCP wide common network plane, we can use Vault directly from k8s clusters.

KMS was discarded, because it requires constant maintenance of keys. For Vault, we require an initial setup, but the day-to-day maintenance of Vault is simpler. Creating, modifying secrets in it is easier. Plus it offers features, which KMS is lacking of.

## Consequences

We will need to maintain a secure Vault platform, make sure, that it’s backed up periodically and it is high available.
For reaching from a k8s cluster Vault directly, we'll need to implement a GCP wide common network plane, so k8s clusters can reach securely and internally our Vault installation, without the need for us to expose Vault over the internet.

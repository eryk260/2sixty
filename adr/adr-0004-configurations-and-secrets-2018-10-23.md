# ADR 0004: Configuration and Secrets

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

We need to store sensitive (credentials, passwords, tokens, etc...) and non-sensitive informations (settings, variables, etc...) for application configurations.

## Decision

Because Vault and KMS can coexist together perfectly, the decision is to use both of them. The developer team can decide whether they use Vault or KMS.

- We will store non-sensitive information/application configurations in the applications' git repositories.
- We will use k8s configmaps to store those configurations for the applications which need it. Either using environmental variables or configuration files.
- KMS: We will store secrets encrypted by Google KMS in the applications' git repositories or GCS buckets.
- Vault: We will store secrets in Vault.
- KMS: Secrets at deployment) CI/CD will decrypt the encrypted secrets stored in the repositories and create kubernetes secrets, which then can be mounted in the pods as... files and/or env variables.
- KMS: Secrets at runtime) Package the secret (or store it in a GCS bucket) with your app and at runtime, you use KMS API to decrypt it.
- Vault: Secrets at deployment) CI/CD will grab the secrets from Vault and deploy with the application.
- Vault: Secrets at runtime) The Service Account which will run the application pods will have access to Vault to grab the necessary secrets at runtime.

The main reason not to exclude Vault was this:

```"The Google Cloud Vault secrets engine dynamically generates Google Cloud service account keys and OAuth tokens based on IAM policies. This enables users to gain access to Google Cloud resources without needing to create or manage a dedicated service account."```

From: https://www.vaultproject.io/docs/secrets/gcp/index.html


## Consequences

- We will need to create automation for managing the KMS keys (creation, giving access to them, etc...).
- We will need to deploy Vault in a HA fashion and make it available across our internal GCP network.

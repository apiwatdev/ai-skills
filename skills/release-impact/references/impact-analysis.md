# Application, configuration and infrastructure impact analysis

## Inputs and approved evidence

Use **only** the inspected repository's code, build and deployment files, repository documentation, and information **explicitly confirmed by the developer**. External repositories and live infrastructure are **outside v1.0 scope** unless their details are provided as user-confirmed evidence; never imply they were inspected.

For every material finding record the changed path(s), nature of the change, observed evidence and possible consequence. Do not infer final actions from file extensions alone.

## Application / component mapping

1. Group changed paths by project, package, component or deployable artifact using real project/solution/workspace structure and manifests.
2. Inspect build definitions, Dockerfiles, deployment workflow files, package references and deployment documentation if available.
3. Identify directly changed components, plus dependencies that may need coordinated deployment; **do not infer automatic rebuild/deploy requirements** without evidence of the packaging process.
4. Track deleted/renamed/relocated files and compatibility changes (API contracts, DTOs, route changes, authentication flows, shared libraries, scheduled jobs, external interfaces).
5. If only a shared library changed, determine which deployables include it, based on actual dependency declarations. Otherwise mark the possible downstream components `Review Required` or `Unknown`.
6. If no authoritative deployment mapping is available, list candidate artifacts and ask which output/server/service is deployed; never invent ZIP names, target folders or IIS site names.

## Configuration impact

- Compare key **names**, structure and semantics across versions (e.g. `appsettings`, environment configuration, YAML, Helm, Terraform, pipeline variables) without displaying sensitive values.
- Distinguish added/removed/renamed keys from default-value changes and environment-specific overrides.
- Classify configuration changes as `Review Required` until their target-environment availability and intended values are confirmed.
- Do not claim that sample configs match production or that a secret exists in a vault.
- Never reveal a password, token, connection string with credentials, private key, or real environment secret. State "value must be supplied/confirmed securely" where needed.

## Infrastructure impact

Inspect explicit evidence for:
- Hosting model / service declarations / worker scheduling
- Ports and network connectivity requirements
- File system and identity permissions
- Certificates, key stores, service accounts and secret-management integration
- Runtime/framework upgrades, package/OS prerequisites
- Storage, queues, object storage, SFTP/SMB, third-party service contracts
- Deployment pipeline, release artifacts, warmup/health-check integration

A source-level reference to an external service does **not** prove that infrastructure must change. Mark uncertainty and required confirmation.

## Deployment action decision ledger

| Field | Expectation |
| --- | --- |
| ID | Stable action identifier (for example APP-01, CFG-01, INFRA-01) |
| Impact | What changed in code/docs |
| Action | Specific deployment/review item, or "confirm needed" |
| Classification | Required / Review Required / Not Applicable / Unknown |
| Evidence | Relevant changed path, manifest, docs section, commit or explicit developer confirmation |
| Environment dependency | What deployed baseline or configuration must be confirmed |
| Risk / validation | What can break and how the team can verify it (grounded only) |

If the environment baseline or artifact mapping is unknown, **do not** label Deploy API/Worker/Web as `Required` merely because source files changed.

## Change Summary

Summarize observed functional changes, bug fixes and technical changes with evidence. Do not confuse commit messages or comments with proof of runtime behavior. Separate planned/tested behavior from what the code demonstrably does.

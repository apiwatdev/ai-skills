# Deployment sequence, environment readiness, PVT and rollback

## Baseline gate

Ask for **target environment** and **currently deployed version/commit** (or trustworthy artifact identity). Without them:
- You can provide a Git-only change report and a **provisional** checklist with `Review Required` / `Unknown` items.
- **Do not** promote candidate actions to `Required` solely based on the FROM→TO diff.
- State "Target environment or actual deployment baseline unverified; this is not an execution-ready deployment plan."

If the confirmed deployed baseline **differs from FROM**, report the mismatch and ask whether to rerun comparison from the actual baseline or to treat this as hypothetical. The Git FROM→TO diff alone is **not** a reliable inventory of pending changes for that environment.

## Action classification

- `Required` — confirmed pending action for this exact environment and baseline, supported by code/docs and execution/prerequisite evidence.
- `Review Required` — concrete possible action with missing execution history, sequencing detail, environment configuration or owner decision.
- `Not Applicable` — evidence positively rules out the action for this release and environment. Absence of a matching file is not sufficient for an external dependency.
- `Unknown` — insufficient evidence to evaluate whether the change has any deployment impact.

For actions with conditional execution, prefer an explicit review/decision item before an executable step. Do not call unverified steps mandatory.

## Candidate checklist phases

These are **organizational headings**, not approval to perform generic actions:

1. **Pre-deployment** — confirm deployment baseline, environment, artifacts, prerequisites and necessary backups or snapshots (if evidenced).
2. **Migration and data** — dependency-reviewed database steps, if any; otherwise record `Review Required` or `Unknown`.
3. **Application and configuration** — relevant artifact packaging/deploy targets, configuration changes and restart/warmup steps when established.
4. **PVT** — check actual project-defined health checks, affected API flows, workers, integrations, database data or job behavior.
5. **Rollback readiness** — prior artifact availability, rollback procedure and data/migration recovery feasibility when known.

An ordered list is valid only when dependencies/order are supported. If sequence is uncertain, **do not fabricate an exact execution order**; state that the order requires confirmation and identify the gap. Service restarts, downtime and backup steps should appear only when supported by a real plan or clearly marked as requiring review.

## PVT design

- Identify affected capabilities from inspected code and existing tests/deployment runbooks.
- Suggest the smallest test set that actually covers the changed contract and key integrations.
- Use exact endpoint names/paths, scripts, test commands, environment values **only if present in evidence**; otherwise ask the developer to supply them.
- Every verification item must state what success would mean **without claiming the test was run**.
- Do not conflate source-level unit tests with a passed deployment smoke test.
- Mark checks as **planned**, **pending confirmation**, or **performed (user-confirmed)** if and only if the developer provided that information.

## Rollback review

- Capture reversible vs one-way database changes; some upgrades are not safely reversible.
- Check whether previous artifacts/config snapshots and documented restore procedures exist.
- Identify whether service/data compatibility across versions is supported.
- If no verified rollback procedure exists, say "Rollback procedure unverified — requires explicit owner/DBA review" and **do not invent commands**.
- Flag blockers to production rollout rather than issuing confident go/no-go approval with missing critical information.

## Scope control

Only analyze the current repository. Where deployments depend on other repositories, vendors, databases or infrastructure you cannot inspect, document those dependencies as external validation items. Do not claim they are unchanged or configured.

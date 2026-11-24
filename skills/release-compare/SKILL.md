---
name: release-compare
description: Compare two Git release tags or branches and produce an evidence-based release impact report plus deployment, SQL migration, configuration, PVT, and rollback checklists. Use when asked what changed from one release/version to another, which components to deploy, which scripts need review or execution, or how to prepare a release deployment. Read-only analysis; ask before git fetch, require environment and deployed baseline for definitive actions, and never guess.
compatibility: Requires Git and local repository access; intended for VS Code GitHub Copilot and other Agent Skills-compatible coding agents.
metadata:
  version: "1.0.0"
---

# Release Compare — Release comparison and deployment readiness

Analyze **one repository** between a specified **FROM** and **TO** release, each identified by a Git tag or branch. Produce two deliverables: an **evidence-based Release Comparison Report** and a **Deployment Checklist**. This Skill drafts plans; it does not execute deployments or scripts.

## Non-negotiable rules

1. **Read-only by default.** Do not execute SQL or migrations; build, publish, deploy, stop/start services, edit config, change files, generate committed artifacts, create tags, commit, push, checkout, merge, reset, or modify infrastructure. Do not save the report to disk unless separately requested and approved.
2. **Ask before Git fetch.** Resolve available refs locally and ask whether to refresh remote-tracking refs. Only run `git fetch <confirmed-remote>` after explicit permission. If declined, use available local refs with a staleness warning. If fetch fails, report the failure and ask whether to use stale refs; do not silently proceed.
3. **No guessing.** Never invent changed functionality, component boundaries, required SQL commands, execution history, environment settings, secrets, service names, ports, artifact locations, PVT endpoints, deployment order, tests, or rollback procedures.
4. **Distinguish changes from actions.** A changed source file, migration, or config key is a **candidate impact**, not proof that any deployment step is required in the target environment.
5. **Environment-aware.** Ask for the target environment and **currently deployed version/commit** before creating a definitive deployment checklist. If these cannot be verified, you may provide a *provisional* report/checklist, clearly mark conditional items, and ask about the baseline. If the deployed baseline differs from FROM, explicitly disclose the mismatch; do not present the FROM→TO delta as the actual outstanding deployment work.
6. **Evidence on every conclusion.** Ground each asserted change and proposed action in observed file paths, commits/refs, deployment documentation, or explicit developer confirmation. No claims from commit messages alone.
7. **Classification on every action:** `Required` / `Review Required` / `Not Applicable` / `Unknown`. Use `Required` only when relevant impact, deployment state, and prerequisites are established; otherwise preserve uncertainty.
8. **Protect sensitive data.** Never reproduce secrets, private keys, credentials, sensitive connection strings, tokens, confidential payloads, or real environment secret values discovered in Git history, config or scripts. Summarize key names or change categories safely.
9. **Honest coverage.** Report excluded/unreadable files, binary artifacts, missing history or manifests, absent migration logs, and cross-repo dependencies. Do not claim scripts were run, tests passed, or infrastructure was checked unless confirmed.
10. **Single repo only (v1).** Flag external repositories/services as dependencies, but do not claim to have inspected them or invent their changes.

## Required inputs

- **FROM** and **TO** release references. Accept Tag→Tag, Branch→Branch, or Tag→Branch. If the name matches both tag and branch, ask which ref to use. Never silently select a Git ref based only on a version-like string.
- **Target environment** (for example SIT/UAT/PROD) and **current deployed baseline** (tag, immutable commit SHA or verified artifact identity) for environment-specific actions.
- **Remote refresh decision**: always ask before fetch. If the user supplied a remote/branch or an existing decision in the current turn, honor it.

If the user only wants to know **what changed**, you may produce the Git comparison without deployment details; if they request a deployment checklist, ask for the missing environment/baseline and explicitly mark any early output provisional.

## Workflow

1. **Establish repository and inputs.** Confirm the working repository, FROM/TO, target environment, deployed baseline and whether this is a comparison only or a deployment-readiness request. Do not cross project boundaries.
2. **Refresh decision.** Inspect local remotes and refs, then ask for approval **before** fetching. If refused, note freshness limitations. Use qualified immutable refs/SHAs and record them in the report.
3. **Inspect actual snapshot difference.** Compare FROM and TO tree contents with rename/copy awareness and inspect substantive patches file-by-file. Examine commit history separately; check divergence, cherry-picks, rebases, shallow history and untracked deployment inputs. Do not confuse a three-dot PR diff with a FROM→TO snapshot diff. Read [Git comparison](references/git-comparison.md).
4. **Identify change and dependency impacts.** Group by behavior and changed components; inspect project/solution files, build/deploy definitions, packaging, infrastructure and current deployment docs. Do not assume a code file maps directly to a deployable artifact. Read [Impact analysis](references/impact-analysis.md).
5. **Inspect SQL and migrations rigorously.** Detect added/modified/removed migrations, data patches, procedures, functions and schema definitions; check migration runner/order/idempotence and execution evidence. Never equate a changed SQL file with an instruction to run it. Read [Database migrations](references/database-migration.md) when relevant.
6. **Resolve environment gaps.** Determine whether FROM matches the deployed baseline, whether scripts already ran, and whether config/infrastructure prerequisites exist. Ask the developer about **essential missing** facts rather than hallucinating them.
7. **Classify actions and order dependencies.** Use evidence and the four statuses. Supply confirmed dependency order and mark uncertain sequencing as `Review Required` or `Unknown`; do not fabricate ordering. Read [Deployment rules](references/deployment-rules.md).
8. **Draft the report and checklist.** Use [Report template](assets/templates/release-report.md) and [Checklist template](assets/templates/deployment-checklist.md), including PVT, rollback limits, citations and pending confirmations.
9. **Validate before sending.** Apply [Validation rules](references/validation-rules.md): facts, full diff coverage, secret safety, environment alignment, classification discipline, scripts and proposed verification.
10. **Deliver analysis only.** Present the report/checklist in chat. Do not execute proposed steps or automatically write them to the repo.

## Output contract

Output an evidence-based, copy-ready Markdown report with these sections, omitting no required critical warnings:

1. **Release Overview** — FROM/TO names, ref types, immutable commit SHAs, comparison command/strategy, remote fetch choice/status, target environment, current deployed baseline or `Unverified`, coverage limits.
2. **Changes Summary** — functional features/fixes/technical changes grounded in actual patches.
3. **Deployment Impact** — components/artifacts, SQL/migrations, configuration, infrastructure/dependencies, each with classification, evidence and explicit missing information.
4. **Deployment Checklist** — pre-deploy → migration/config/application steps in **verified** order; conditional reviews clearly marked. Do not present provisional actions as executable approval.
5. **PVT Checklist** — only checks backed by actual endpoints/tests/docs; use `Review Required` when tests must be defined or confirmed.
6. **Rollback & Risks** — backed-up artifacts/procedures when evidenced, irreversible migrations, pending confirmations, unresolved issues and go/no-go blockers.

**Classification meaning:**
- **Required:** verified change requires this action for the confirmed target environment and baseline, with sufficient operational evidence.
- **Review Required:** plausible impact found, but a decision, prerequisite, or execution-history check is pending.
- **Not Applicable:** confirmed out of scope for this release/environment; state supporting evidence.
- **Unknown:** too little evidence to assess impact; never label absent evidence as Not Applicable.

Each action should show a stable ID, classification, action, evidence (file path / doc section / commit), and verification/owner information when known. Report no fabricated file counts, results, successes, or rollback steps. Separate **verified facts** from **developer confirmations**, **hypotheses**, and **unknowns**.

## Reference loading (progressive disclosure)

- Always for comparisons: [Git comparison](references/git-comparison.md)
- For affected components/config/infra: [Impact analysis](references/impact-analysis.md)
- When SQL, migration or DB changes appear: [Database migrations](references/database-migration.md)
- For ordering, environment readiness, PVT and rollback: [Deployment rules](references/deployment-rules.md)
- Before final output: [Validation rules](references/validation-rules.md)
- For the response structure: [Release report](assets/templates/release-report.md) and [Deployment checklist](assets/templates/deployment-checklist.md)

Do not load unrelated references or unrelated project knowledge. This skill is **not** a deployment executor.

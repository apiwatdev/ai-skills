# Release Impact Report — Markdown template

> Agent instructions: Replace bracketed prompts with **observed** facts only. Never publish a bracketed prompt as a fact. State "Unverified" for unresolved required input. If the target environment or deployed baseline is unknown, label every deployment-action section **PROVISIONAL — NOT EXECUTION-READY**. Do not invent file counts, script states, passed tests, or references. Remove this instruction note when drafting the report.

# Release Impact Report: [FROM] → [TO]

## 1. Release Overview

| Field | Verified value |
| --- | --- |
| Repository | [Repository name] |
| FROM reference | [Qualified ref and Tag/Branch type] |
| FROM SHA | [Full commit SHA] |
| TO reference | [Qualified ref and Tag/Branch type] |
| TO SHA | [Full commit SHA] |
| Comparison | [Snapshot diff strategy and refs] |
| Git refresh | [Approved and fetched / declined and local refs / failed; include data-freshness warning] |
| Target environment | [Confirmed environment or Unverified] |
| Currently deployed baseline | [Confirmed tag/SHA/artifact or Unverified] |
| Baseline alignment | [Matches FROM / Mismatch / Unverified] |
| Inspection coverage | [Actual changed files/commits inspected and exclusions] |
| Readiness | [Evidence-based status or PROVISIONAL — NOT EXECUTION-READY] |

## 2. Changes Summary

Describe only behavior observed in the inspected net snapshot diff. Group as needed:

### Features
- [Grounded change + file evidence, or state no features were identified in the inspected scope.]

### Bug Fixes
- [Grounded change + evidence.]

### Technical Changes
- [Grounded change + evidence.]

## 3. Deployment Impact

| ID | Area | Impact and proposed action | Classification | Evidence | Prerequisite / pending confirmation |
| --- | --- | --- | --- | --- | --- |
| [APP-01] | Application | [Observed impact and target-specific action] | [Required / Review Required / Not Applicable / Unknown] | [File path / doc / developer confirmation] | [Real conditions] |
| [DB-01] | Database | [Exact migration/script candidate or uncertainty] | [Status] | [Evidence] | [Execution history / dependency check] |
| [CFG-01] | Configuration | [New/changed key name; never reveal secret value] | [Status] | [Evidence] | [Environment value confirmation] |
| [INFRA-01] | Infrastructure | [Actual change or uncertainty] | [Status] | [Evidence] | [Dependency/owner confirmation] |

Remove example rows that have no actual grounded finding. If an area has no identified changes, distinguish "none observed in inspected files" from a confirmed `Not Applicable` decision.

## 4. Deployment Checklist

Use the [deployment checklist template](deployment-checklist.md). Carry over the **same classifications, conditions and evidence**, and order only proven dependencies. All unchecked items are planned actions, not evidence of execution.

## 5. PVT Checklist

| ID | Proposed verification | Classification | Expected result / coverage | Evidence | Actual result |
| --- | --- | --- | --- | --- | --- |
| [PVT-01] | [Real existing endpoint/test or test design requiring review] | [Status] | [What would establish success] | [Repo path/docs] | Not executed / user-confirmed result |

Never invent a health endpoint, test command or passing result.

## 6. Rollback & Risks

### Rollback readiness
- [Verified procedure or "Rollback procedure unverified; owner/DBA review required".]
- [Database/data recovery limitations only when evidenced.]

### Risks and blockers
- [Potential breaking compatibility, irreversible changes, missing artifact, external dependencies, baseline mismatch.]

### Questions requiring developer confirmation
- [Only essential missing information; ask directly.]

### Evidence and limitations
- [Reference paths, commit IDs and list of any uninspected changes.]

> Final reviewer warning: analysis only, not a deployment authorization. No scripts or deployments were executed by this Skill.

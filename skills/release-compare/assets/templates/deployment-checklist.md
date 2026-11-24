# Deployment Checklist — Markdown template

> Agent instructions: This is a **planned checklist**, not executed steps. Every row must have a real evidence source and one of the four action classifications. Do not turn Review Required or Unknown into an executable command. Omit nonexistent steps instead of inventing procedures. When baseline is unverified/mismatched, title it "PROVISIONAL — NOT EXECUTION-READY". Checklist order is illustrative until dependencies are proven.

# Deployment Checklist: [FROM] → [TO] — [Environment]

**Readiness:** [Verified for planning / PROVISIONAL — NOT EXECUTION-READY]  
**Current deployed baseline:** [Confirmed tag/SHA/artifact or Unverified]  
**Scope:** One inspected repository; external dependencies require independent confirmation

## Pre-deployment

- [ ] **PRE-01** · **[Classification]** Confirm environment, deployed baseline, expected target artifact(s). Evidence: [source]. Condition: [if any].
- [ ] **PRE-02** · **[Classification]** Confirm specific backup/prerequisite requirements documented for impacted changes. Evidence: [source].

## Database / Migration

- [ ] **DB-01** · **[Classification]** [Specific migration or "Review execution history for ..."]. Evidence: [script/migration path; tracking record if established]. Prerequisite: [confirmed order or Pending review].

## Application and Configuration

- [ ] **APP-01** · **[Classification]** [Named application artifact and target only when verified]. Evidence: [build/deployment manifest, developer confirmation].
- [ ] **CFG-01** · **[Classification]** [Confirm availability of specific new/renamed config key; never print its sensitive value]. Evidence: [path]. Prerequisite: [environment confirmation].
- [ ] **INFRA-01** · **[Classification]** [Explicit infra dependency to verify; omit if no actual evidence]. Evidence: [path or developer statement].

## Post-deployment Verification (PVT)

- [ ] **PVT-01** · **[Classification]** [Actual affected endpoint/test/check]. Expected success: [confirmed observable behavior]. Evidence: [test/runbook/code path]. Actual result: **Not executed** unless developer confirmed otherwise.

## Rollback Readiness

- [ ] **RB-01** · **[Classification]** [Verified prior-artifact/config/data recovery procedure, or request owner confirmation]. Evidence: [runbook/file]. Irreversible/unknown aspects: [if any].

## Pending Confirmations / Blockers

- [ ] **Q-01** · [Question needed to determine whether a candidate action must occur]
- [ ] **Q-02** · [Question needed to confirm migration history/dependency/rollback]

## Classification legend

| Classification | Meaning |
| --- | --- |
| Required | Evidence verifies this action is pending and necessary for the confirmed environment and baseline. |
| Review Required | A concrete candidate needs confirmation before being scheduled/executed. |
| Not Applicable | Positive evidence confirms out of scope for this environment/release. |
| Unknown | Evidence is insufficient to assess applicability. |

Delete placeholder items with no factual basis. Do not execute any command, script, deployment or rollback step through this reporting Skill.

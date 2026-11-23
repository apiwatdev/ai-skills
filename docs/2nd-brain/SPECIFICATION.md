# 2nd-brain — Final Specification & Implementation Plan

**Version:** Design v1.0  
**Status:** Requirements confirmed; implementation not started  
**Scope:** Project-based Markdown knowledge management  
**Skill distribution:** Central `ai-skills` repository  
**Compatible agents (target):** Codex, Claude Code, Cursor  
**Date:** 2026-11-23

> This document specifies the future `2nd-brain` Agent Skill. It is **not** a `SKILL.md` implementation. Installation and per-project knowledge-root configuration are deferred.

## 1. Purpose

Create one reusable Agent Skill named **`2nd-brain`** to help AI coding agents and developers find, read, capture, update, organize, and review project-specific knowledge written in Markdown. Knowledge must be understandable to developers, easy for AI to retrieve selectively, and trustworthy. The Skill must **never guess missing facts** or alter Knowledge without approved scope.

The Skill is centrally maintained; knowledge itself belongs to each project. Manual and automatic invocation are both intended.

## 2. Confirmed Requirements (1–22)

| # | Requirement | Approved decision |
| --- | --- | --- |
| 1 | Knowledge capture | Proactively **suggest** capturing useful knowledge; obtain approval before writing. |
| 2 | Knowledge update | When documentation differs from code, report the discrepancy and propose an update. |
| 3 | Markdown detail | Adaptive to knowledge type and complexity. |
| 4 | Classification | Hybrid: use default categories; propose new categories only when necessary. |
| 5 | Reading strategy | Read only knowledge relevant to the current task. |
| 6 | Markdown structure | Standard templates for document types. |
| 7 | Lifecycle | `draft`, `verified`, `outdated`, `deprecated`. |
| 8 | Verification | Only a developer can confirm `verified`. |
| 9 | Duplicate handling | Suggest **merge / update / create** and let the developer choose. |
| 10 | Metadata schema | Balanced YAML frontmatter. |
| 11 | Relationships | Maintain related links and index automatically *within the approved write scope*. |
| 12 | Validation | Strict; block affected writes on critical failures. |
| 13 | Capture criteria | Adaptive: favor durable, reusable project knowledge over transient notes. |
| 14 | Approval scope | Show proposed content and the **complete list of affected files** for approval. |
| 15 | Evidence tracking | Minimal: source references are optional, but evidence must never be invented. |
| 16 | Index strategy | Begin with one `INDEX.md`; propose category indexes as knowledge grows. |
| 17 | Missing information | Ask the developer for essential missing information before creating/updating knowledge. |
| 18 | No guessing | Strict: do not invent facts, endpoints, root causes, settings, test outcomes, or decisions. |
| 19 | Unverified reading | Cautious: read `draft`/`outdated` with explicit uncertainty and verification. |
| 20 | Category and document type | Six default categories; category and type are separate concepts. |
| 21 | Missing fields | Ask about essential facts; omit missing optional fields and optional sections. |
| 22 | Write consistency | On partial failure, **stop, report actual state, and propose a repair plan**. |

## 3. Core Operations

| Operation | Responsibility |
| --- | --- |
| `find` | Discover documents relevant to a task or question. |
| `read` | Retrieve selected knowledge and assess status and relevance. |
| `capture` | Propose creation of a new knowledge document. |
| `update` | Propose corrections or improvements to an existing document. |
| `organize` | Maintain categories, indexes, and related links. |
| `review` | Inspect for duplicates, stale/conflicting information, broken links, and metadata defects. |

These describe Skill behavior and **do not** require a separate CLI.

## 4. Proposed Skill Package (not created yet)

```text
2nd-brain/
├── SKILL.md
├── references/
│   ├── classification.md
│   ├── reading-rules.md
│   ├── writing-rules.md
│   ├── lifecycle.md
│   ├── metadata-schema.md
│   └── validation-rules.md
└── assets/
    └── templates/
        ├── architecture.md
        ├── decision.md
        ├── development.md
        ├── operation.md
        ├── troubleshooting.md
        └── reference.md
```

The future `SKILL.md` should cover activation, intent detection, routing, approval, safety, and the No Guessing Policy. Supporting rules/templates should be read only for operations that need them (progressive disclosure).

## 5. Classification and Templates

| Default category | Example document types |
| --- | --- |
| `architecture` | `system-overview`, `component`, `data-flow` |
| `decisions` | `adr`, `technical-decision` |
| `development` | `coding-standard`, `implementation-guide` |
| `operations` | `runbook`, `deployment`, `configuration` |
| `troubleshooting` | `incident`, `known-issue`, `resolution` |
| `references` | `api-reference`, `external-reference` |

`category` describes where knowledge is filed; `type` describes the document form. Propose a new category with justification and include it in the approval plan; do not create categories silently.

### Balanced YAML Frontmatter

**Required:** `title`, `type`, `status`, `summary`, `tags`, `created`, `updated`.

**Optional:** `related`, `verified_at`, `superseded_by`.

By default, the directory is the category, so a redundant `category` field is not necessary. Use real creation/update dates rather than invented historical dates.

Example *template only* (not data about a real project):

```markdown
---
title: Authentication Flow
type: system-overview
status: draft
summary: Authentication process and token validation flow
tags: [authentication, jwt]
created: 2026-11-23
updated: 2026-11-23
related: []
---

# Authentication Flow

## Overview
<!-- Document only known facts. -->

## Components
<!-- Include known components; ask for essential missing details. -->

## Process
<!-- Explain verified steps without guessing. -->
```

## 6. Knowledge Lifecycle

| Status | Meaning | Reading rule |
| --- | --- | --- |
| `draft` | Not yet developer-verified | May read cautiously; explain uncertainty. |
| `verified` | Explicitly confirmed by developer | Use subject to currency and relevance checks. |
| `outdated` | Potentially inconsistent with current implementation | Warn and verify against current code; propose correction. |
| `deprecated` | No longer current | Historical use only; point to replacement when known. |

**Proposed transitions:**
- `draft → verified`: explicit developer confirmation required.
- `verified → outdated`: propose change after discovering discrepancy; obtain approval.
- `outdated → verified`: fix and obtain new developer confirmation.
- `verified/outdated → deprecated`: requires developer approval.

No silent status transitions. `verified` is not a guarantee of permanent accuracy.

## 7. Knowledge Reading Workflow

1. Determine whether project knowledge is relevant to the current task; do not read it by default for unrelated tasks.
2. Locate the project's knowledge root. If the project or path is ambiguous, **ask rather than guessing**.
3. Read `INDEX.md`, using title, summary, tags, and category to select candidate documents.
4. Read only relevant Markdown and follow genuinely useful related-document links.
5. Check lifecycle status, staleness, and current code when that affects correctness.
6. Answer from grounded information; explicitly distinguish verified facts, hypotheses, unknowns, and conflicts.
7. Do not use `deprecated` content as current guidance or present `draft`/`outdated` as confirmed facts.

## 8. Knowledge Writing Workflow

1. Identify a knowledge candidate worth preserving for future work; propose saving it.
2. Search existing knowledge for overlap, duplicates, conflicts, or an existing canonical document.
3. **Ask for essential missing information**; never manufacture answers.
4. Choose category, document type, template, and adaptive level of detail.
5. Validate accuracy, metadata, template structure, links, duplication, consistency, and secrets.
6. Present the **complete proposed content and every file path to be added/updated/moved**, including affected `INDEX.md` and related links.
7. Obtain explicit developer approval for the particular content and full change list **before writing**.
8. Apply only approved changes; automatically keep indexes/relationships consistent *within that authorization*.
9. Validate each written file and report actual changes and any limits.
10. On a partial failure, **stop**, identify successful and failed writes, and present a repair plan; do not silently attempt unapproved repair changes.

No automatic Git commit or push. Never store passwords, credentials, private keys, access tokens, or other secret values.

## 9. Index and Knowledge Relationships

- Start with one root `INDEX.md` for navigation and summaries.
- When the collection grows, **propose** category indexes; do not restructure without approval.
- Maintain one canonical document per topic where possible; prefer links over duplicated explanations.
- Use portable relative paths for links inside the knowledge base.
- When approved changes move, rename, or deprecate a document, update impacted index entries and inbound/outbound links within scope.
- Do not create unrelated or speculative connections.

## 10. Strict Validation and No Guessing

| Check | Mandatory rule |
| --- | --- |
| Metadata | Valid YAML, complete required fields, allowed statuses, consistent dates. |
| Structure | Template-conformant sections; omit optional empty sections rather than invent filler. |
| Accuracy | Ground facts in source code, confirmed developer statements, or identifiable evidence. |
| Consistency | Report code/docs conflicts; never silently choose a side. |
| Duplicate prevention | Present merge/update/create options before changing a canonical topic. |
| Link integrity | Check relative references and all affected index links. |
| Security | Do not write secrets or expose confidential payloads. |
| Approval | Obtain approval for complete content and all affected paths. |
| Completeness | Ask only for essential gaps; never guess. |
| Post-write | Verify all intended changes; partial success is not full success. |

**Strict** means block the affected writes on critical failures, missing approval, or unresolved essential facts. Unrelated noncritical issues can be reported as warnings without modifying them.

## 11. Example Change Proposal

```text
Proposed Knowledge Changes

CREATE  docs/knowledge/troubleshooting/api-timeout.md
UPDATE  docs/knowledge/INDEX.md
UPDATE  docs/knowledge/architecture/api-flow.md

Validation: Passed for this proposed change set
Approval: Required before any write
```

Paths here are examples, **not hardcoded project paths**. The developer must be shown each proposed file's content.

## 12. Implementation Plan

### Phase 1 — Core Skill
Create `SKILL.md`: intent detection, manual/automatic invocation, workflow routing, explicit approval gate, No Guessing Policy, and scope boundaries.

### Phase 2 — Knowledge Reading
Write `reading-rules.md` and `classification.md`: project discovery, index routing, relevant-only retrieval, and lifecycle-aware reading.

### Phase 3 — Knowledge Writing
Write `writing-rules.md`, `metadata-schema.md`, and six templates; implement duplicate-handling choices, essential clarification, full-change approval, and index updates.

### Phase 4 — Validation and Lifecycle
Write `validation-rules.md` and `lifecycle.md`: integrity validation, human verification, status transitions, and partial-failure behavior.

### Phase 5 — Acceptance Testing
Exercise all six operations against a sample project, including unverified docs, duplicates, conflicting code, missing facts, secrets, and simulated write failures.

## 13. Acceptance Criteria

- Finds and reads only relevant documents from the correct project.
- Discloses uncertainty when using `draft` or `outdated` knowledge.
- Never invents facts, inferred root causes, missing endpoints, or evidence.
- Asks for essential missing details and omits optional unknowns.
- Suggests merge/update/create when knowledge overlaps.
- Requires explicit developer confirmation for `verified`.
- Shows all proposed content and paths, and obtains approval before changes.
- Prevents secret leakage.
- Maintains index and related-document links within authorized scope.
- Stops on partial write failure and gives an accurate repair plan.

## 14. Deferred Design Decisions

- How to install and version central Skills for Codex, Claude Code, and Cursor.
- How to configure a project Knowledge Root and discover multi-repository projects.
- The specific threshold for splitting `INDEX.md` into category indexes.

**Implementation status:** This is the approved design baseline for **`2nd-brain`**. No `SKILL.md` has been created.

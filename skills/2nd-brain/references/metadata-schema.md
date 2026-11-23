# Balanced YAML frontmatter and document shape

All managed knowledge Markdown files should have valid YAML frontmatter.

## Required frontmatter

| Field | Type | Rules |
|---|---|---|
| `title` | non-empty string | Human-readable title, aligned with `#` heading. |
| `type` | non-empty kebab-case string | Select type based on content and category. |
| `status` | enum | One of `draft`, `verified`, `outdated`, `deprecated`. |
| `summary` | short string | Specific enough for a reader/agent to choose the file. |
| `tags` | list of strings | Useful topics; empty list if no reliable tags. |
| `created` | YYYY-MM-DD | Real document creation date. Preserve when updating. |
| `updated` | YYYY-MM-DD | Real document content-update date. |

## Optional frontmatter

| Field | Type | Rules |
|---|---|---|
| `related` | list of Markdown paths | Paths relative to the owning Markdown file; link only real files. |
| `verified_at` | YYYY-MM-DD | Developer-verified timestamp only if established. |
| `superseded_by` | Markdown path | For deprecated knowledge when replacement exists. |

The category is represented by the containing directory, not redundantly required in metadata. Avoid introducing new frontmatter fields unless the project conventions demand them and the change plan includes them.

Example *shape* (replace with established facts and actual dates before writing):

```yaml
---
title: "Example API Investigation"
type: "incident"
status: draft
summary: "Investigation notes for an API issue"
tags: [api, investigation]
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
related: []
---
```

The example title/date values are placeholders, **not** acceptable committed knowledge. Remove all template instructions and placeholders before saving.

## Document body

- One H1 title; concise Overview/Summary near the top.
- Use the matching template in `assets/templates/`, retaining only substantiated, useful sections.
- Short, explicit sentences and headings; use lists, tables and fenced code only where they improve clarity.
- For investigations, separate observed symptoms/evidence from hypotheses and unconfirmed root causes.
- For decisions, distinguish decision/status from considered alternatives and rationale.
- Relative links should resolve from the document directory and remain portable.
- Do not assume a source file or citation exists just because a metadata path is plausible.

# Classification and canonical knowledge

Use these default categories for project knowledge. A category is a storage location; a document `type` describes its shape.

| Category | Typical types | Examples |
|---|---|---|
| `architecture` | `system-overview`, `component`, `data-flow` | Boundaries, responsibilities, architecture and diagrams |
| `decisions` | `adr`, `technical-decision` | Approved trade-offs and rationale |
| `development` | `coding-standard`, `implementation-guide` | Patterns, conventions, build instructions |
| `operations` | `runbook`, `deployment`, `configuration` | Deployment procedures, operational practices |
| `troubleshooting` | `incident`, `known-issue`, `resolution` | Observations, investigations, verified fixes |
| `references` | `api-reference`, `external-reference` | API behavior, specifications, reference material |

## Classification procedure

1. Identify the *single dominant purpose* of the document, not just words in its title.
2. Search `INDEX.md` and relevant documents before choosing a path.
3. Prefer the closest existing category and canonical document. Do not duplicate a concept solely because it is used by multiple components.
4. If categories cannot express the concept accurately, propose a new category with a reason and affected index changes. Creating it requires approval.
5. Use descriptive, stable, lowercase-kebab-case filenames. Avoid ambiguous `notes.md` and `misc.md` when possible.
6. If classification is ambiguous and changes navigation or ownership, ask the developer before proposing a write.

## Duplicate/collision handling

- Exact same topic with improvements: propose **update**.
- Overlapping documents that should be consolidated: propose **merge**, preserving unique facts and historical decision rationale.
- Distinct purpose or historical record: propose **create**, then link to the canonical document.
- Present these choices to the developer. Never delete or silently overwrite knowledge to resolve duplicates.
- Keep one current canonical document per topic where practical; references may link to it.

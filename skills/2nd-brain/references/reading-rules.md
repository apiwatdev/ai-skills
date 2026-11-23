# Relevant and cautious reading

Use this procedure for `find`, `read`, and knowledge-assisted development tasks.

1. Confirm the task needs project knowledge. Do not activate a broad knowledge scan for an unrelated request.
2. Determine project/workspace scope from explicit instructions and existing project conventions. Look for a known knowledge root and its `INDEX.md`. Do **not** assume a hardcoded path or mix content from other projects. Ask if ambiguous.
3. Read the root `INDEX.md` first when present; use document title, summary, tags and category to shortlist files. If there is no index, use a limited, targeted filesystem search; disclose that navigation may be incomplete.
4. Read only shortlisted documents, beginning with the most relevant. Follow `related` links only if they materially help the task.
5. Respect the `status` in YAML. `verified` is developer-confirmed but can become stale. `draft` and `outdated` may be read cautiously with an explicit warning, not as reliable current truth. `deprecated` is historical only.
6. When the answer depends on current behavior, inspect related code/config and compare with documentation. Report concrete conflicts, ideally using paths or excerpts, and propose an update rather than editing automatically.
7. Distinguish observed facts, developer-confirmed statements, hypotheses, and unknowns. Never claim a missing document says something, invent an API endpoint, or infer a root cause from symptoms alone.
8. In the answer, cite source file paths/sections when available; note coverage limitations when relevant.
9. Do not change any knowledge file while performing a read-only task.

## Priority on conflicting information

- Do not declare code or documentation automatically authoritative in every circumstance: runtime behavior, branch/environment, and intended design may differ.
- Describe the conflict, concrete evidence, and what requires developer confirmation.
- Do not silently promote `draft` content to `verified` or change lifecycle status as a side effect of reading.

## Scalability

- Do not read every `.md` to establish context.
- When `INDEX.md` grows unwieldy, *propose* category-level indexes. Do not restructure without full change-plan approval.

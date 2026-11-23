# Strict pre-write and post-write validation

## Block the affected write if any critical condition holds

- Developer has not approved *the full proposed content* and *every affected file path*.
- Project root or destination is ambiguous.
- Required facts are missing, contradictory or guessed; ask the developer before writing those assertions.
- Content would contain credentials, tokens, private keys, passwords, sensitive personal information, or unapproved confidential payloads.
- Invalid/missing YAML frontmatter or status, invalid required metadata, or material template inconsistencies.
- Output contains unresolved required placeholders.
- Duplicate/merge conflict unresolved with no developer selection.
- Status would be promoted to `verified` without explicit developer confirmation.
- File writes, renames or deletions fall outside the approved paths.

## Further checks

- Check Markdown heading hierarchy and required type-specific sections without forcing optional empty content.
- Compare claims with cited code/docs when available. Evidence references are optional, **accuracy is not**.
- Verify every new/changed relative link resolves to a real document in the intended target tree.
- Detect broken links caused by moves/renames and ensure indexes reflect new paths.
- Search for redundant canonical documents or overlapping content; propose merge/update/create rather than deciding silently.
- Validate that linked `deprecated` documents are not presented as current instructions.
- If unrelated pre-existing errors are found, warn separately; do not modify unrelated files.

## Post-write

- Re-open each changed file and verify successful write and content fidelity.
- Validate all affected `INDEX.md` and related links again.
- If a write fails: **stop**, report written/unwritten paths and resulting inconsistencies, propose a repair plan, and wait for further authorization for unapproved repairs.
- Never claim all files succeeded from a partial tool response.
- Do not infer test pass status from the absence of an error; state what was actually checked and what was not.

## Approval checklist (show to developer)

```text
Change intent:
Project / Knowledge root:
Proposed files (including indexes/related links):
Proposed content/diffs:
Knowledge lifecycle changes:
Validation results / unresolved questions:
Approval: required before any write
```

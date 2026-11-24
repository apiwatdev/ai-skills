# Release-impact validation and quality gates

Run this review **before presenting** the final Release Impact Report and Deployment Checklist.

## Git accuracy

- Confirm both input refs exist and their exact **type** (tag/local branch/remote tracking branch).
- If a ref is ambiguous across tag and branch namespaces, **ask**; do not choose silently.
- Ask for permission before `git fetch`. Record approved/declined/failed status and staleness warning if applicable.
- Record resolved immutable FROM and TO commit SHAs.
- Primary comparison is `git diff FROM TO`; review net file changes and relevant content, not commit messages alone.
- Report incomplete inspection (binary paths, LFS pointers, missing history, shallow repo, submodules, uninspected files).
- Disclose the difference between net snapshot diff and the commit graph, especially for diverged branches and cherry-picked releases.

## Environment and deployment readiness

- Ask for environment and currently deployed baseline. If missing or mismatched, **block definitive action claims**.
- Verify component/artifact mapping using real manifests or developer confirmation.
- Do not infer Required from changed files, migration paths or CI configuration alone.
- Check config key changes without exposing secret values.
- Flag cross-repo dependencies as out of inspection scope.

## SQL and safety

- Enumerate changed migration candidates accurately; verify run history and required order when relevant.
- Never claim a script was run, not run, safe to rerun, or required without evidence.
- For a modified previously applied migration, flag explicit review.
- Do not generate or execute SQL, deployment commands, or rollback scripts as automatic side effects.
- Do not print secrets, private keys, tokens, credentials or sensitive connection strings in report, log excerpts or examples.

## Action classifications

For each action:
- Is there a clear action and relevant evidence?
- Is the target environment/baseline confirmed?
- Does the classification match the **defined four-state policy**?
- If `Required`, what proves it is pending and necessary for this environment?
- If `Not Applicable`, what positively rules it out?
- If `Review Required` or `Unknown`, is the exact missing information stated?
- If dependencies/order are uncertain, is the step marked as such rather than assigned an invented order?

## Verification and rollback

- PVT tests correspond to actual changed behavior and evidenced endpoints/checks; none is marked passed unless confirmed.
- Rollback requirements are supported by actual procedures and artifact/backup evidence. Unknown migration reversibility is flagged.
- Unknowns, risks and go/no-go blockers appear prominently in the report.
- No automatic Git writes except an explicitly approved fetch; no side-effecting operations.

## Output sanity

- Include Release Overview, Changes Summary, Deployment Impact, Deployment Checklist, PVT Checklist, Rollback & Risks.
- Include FROM/TO types+SHAs, environment/baseline status, comparison/fetch details and limitations.
- Cite relevant paths/commits/documentation for important claims and action rows.
- List critical missing facts as questions for developer confirmation.
- If unable to verify a definitive checklist, **label the whole checklist provisional**; do not hide uncertainty in a footnote.
- Only claim coverage/tests or no changes when actually substantiated.

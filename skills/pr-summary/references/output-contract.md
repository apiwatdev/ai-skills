# Output contract

Use professional English for all content that is intended to be pasted into GitHub or GitLab. A compact analysis section can use the conversation language when appropriate, but the PR/MR title and description remain English.

## Mandatory order

### Git Analysis / Warnings (not for copying)

- Source: `<source-ref>` (`<short-sha>`; local HEAD / remote ref as chosen by user)
- Target: `<remote-target-ref>` (`<short-sha>`)
- Fetch: `Succeeded` / `Failed — user approved stale refs`
- Comparison: `<actual command or comparison strategy>` — `<brief rationale>`
- Scope: `<verified commits/files>`; `uncommitted content excluded`
- Unpushed commits: `<count and user choice, or no remote source to verify>`
- Warnings: `<only verified or explicitly qualified concerns>`

Do not show fake counts or "No warnings" when analysis is incomplete. If a list of warnings is long, prioritize risks to interpretation and put detail outside the PR/MR copy block.

### PR / MR Title

`<type>(optional-scope): <short action-oriented subject>`

For `release/1.2.0`, default to `chore(release): prepare v1.2.0` (provided `1.2.0` is indeed the release version). For `release/foo`, do not invent a version; ask or use a justified human-readable label.

### PR / MR Description

```markdown
## Summary
<One to three concise sentences describing the observable change.>

## Changes
- <Functional change with supporting technical detail when useful.>
- <Another meaningful change.>

## Testing
- <User-confirmed testing type>: <Passed / Failed / Pending / Not performed, with truthful scope>
```

For multi-feature or release work, group under relevant `### Feature` subheadings **inside** `## Changes`. Adapt the number of bullets to actual complexity. Include user-confirmed ticket IDs in relevant change bullets if requested; avoid guessing ticket descriptions or inventing a standalone default section. If a release contains fixes and features, maintain a short overall summary and separate change groups.

## Illustrative sample (fabricated example, not repository evidence)

**Title**: `chore(release): prepare v1.2.0`

**Description**:

```markdown
## Summary
Release v1.2.0 adds transaction inquiry capabilities and refines authentication behavior.

## Changes
### Transaction Inquiry
- Added an endpoint for retrieving transaction details.
- Added request parameter validation.

### Authentication
- Updated JWT expiration validation and invalid-token handling.

### Bug Fixes
- Fixed transaction status mapping for rejected inquiries.

## Testing
- Functional testing: Passed (user-confirmed)
- Regression testing: Not performed (user-confirmed)
```

This example assumes the code diff supports each listed change and the user explicitly confirmed both testing statuses. Without these prerequisites, do not copy the example claims into a real MR.

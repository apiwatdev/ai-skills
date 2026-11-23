# pr-summary v1.0.0

A portable, instruction-only **Agent Skill** for generating copy-ready **GitHub Pull Request** and **GitLab Merge Request** titles/descriptions from committed Git branch changes. It does **not** create PRs/MRs or modify source code.

## What it does

- Detects target branches using `feature/*` / `bugfix/*` -> `develop` and `release/*` / `hotfix/*` -> `main` (overridable).
- Automatically fetches remote refs; on failure, asks whether it may use stale local refs.
- Asks what to do with local commits that have not been pushed.
- Analyzes committed changes in full using adaptive merge-base and snapshot comparisons.
- Asks about any unverified tickets and **always asks for testing results**.
- Writes a Conventional Commit-style title and concise `Summary / Changes / Testing` Markdown.
- Keeps Git analysis and warnings separate from the copy-ready description.

## Requirements

A local Git repository, Git CLI, and a coding agent that supports Agent Skills. Network access and Git credentials are needed for automatic `git fetch`. Running the skill is subject to the agent's normal tool permissions.

## Installation

Install the **entire** `pr-summary/` directory, including `references/`, in your agent's skill search path. For repository-local installations:

| Agent | Folder path |
| --- | --- |
| VS Code / GitHub Copilot | `<repo>/.github/skills/pr-summary/` |
| Codex | `<repo>/.agents/skills/pr-summary/` |
| Claude Code | `<repo>/.claude/skills/pr-summary/` |
| Cursor | `<repo>/.cursor/skills/pr-summary/` |

The same central folder may also be included using an agent's custom skill-location setting when supported. Keep one canonical source rather than maintaining divergent copies.

## Invoke

```text
Use pr-summary for the current branch. Compare it with the default target, ask me about unpushed commits and testing, then draft a PR/MR title and Markdown description. Do not create the PR/MR.
```

For a target override:

```text
Use pr-summary. Compare my current branch with origin/release/1.2.0 instead of the default target. Only include committed changes.
```

## Project files

```text
pr-summary/
├── SKILL.md
├── README.md
└── references/
    ├── git-analysis.md
    └── output-contract.md
```

## Behavior and limitations

- `git fetch` is the only permitted Git metadata mutation; no `pull`, `push` or source editing.
- No GitHub/GitLab API token is needed for summary generation alone.
- The agent asks before finalizing when it needs testing confirmation, ticket validation, local/remote source selection, or permission to continue after fetch failure.
- Shallow/incomplete histories, binaries, semantic cherry-picks, and divergent branches may require warnings.
- Sensitive values from diffs must never be reproduced in the generated summary.

## Authoring references

- [Agent Skills specification](https://agentskills.io/specification)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Cursor skills](https://cursor.com/docs/skills)

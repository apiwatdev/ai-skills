# AI Skills

Central repository for reusable AI Coding Agent Skills and their design documentation.

## Available Skills

| Skill | Purpose | Status | Entry point |
| --- | --- | --- | --- |
| `2nd-brain` | Project-based Markdown knowledge management | Implemented; live project testing pending | [SKILL.md](skills/2nd-brain/SKILL.md) |
| `pr-summary` | Draft GitHub PR / GitLab MR descriptions from committed branch differences | Implemented; live project testing pending | [SKILL.md](skills/pr-summary/SKILL.md) |
| `release-compare` | Compare releases (tags or branches); analyze deployment, SQL, config, infrastructure, PVT and rollback impacts | Implemented; live project testing pending | [SKILL.md](skills/release-compare/SKILL.md) |

## Principles

- Skills are reusable across projects. A central Skill installation should **not** be confused with project-specific source code or knowledge.
- Use progressive disclosure: load the appropriate `SKILL.md` and only necessary references/templates.
- **No guessing:** Separate confirmed facts from potential impacts and unknowns.
- No deployment, SQL execution, config mutation, Git branch operation or other destructive step just because a checklist contains it.
- `release-compare` **asks before `git fetch`**, asks for the target environment/deployed baseline before producing a definitive deployment checklist, and classifies each action as Required / Review Required / Not Applicable / Unknown.
- `2nd-brain` asks for explicit approval of all knowledge content and affected paths before writing.
- Do not reproduce secrets or private keys in reports or knowledge.

## Repository Structure

```text
ai-skills/
├── README.md
├── docs/
│   └── 2nd-brain/
│       └── SPECIFICATION.md
└── skills/
    ├── 2nd-brain/
    │   ├── SKILL.md
    │   ├── references/
    │   └── assets/templates/
    ├── pr-summary/
    │   ├── SKILL.md
    │   ├── README.md
    │   └── references/
    └── release-compare/
        ├── SKILL.md
        ├── references/
        │   ├── git-comparison.md
        │   ├── impact-analysis.md
        │   ├── database-migration.md
        │   ├── deployment-rules.md
        │   └── validation-rules.md
        └── assets/
            └── templates/
                ├── release-report.md
                └── deployment-checklist.md
```

## Use in VS Code + GitHub Copilot

Install or configure the **entire `skills/` directory** in your agent's Skill search locations; keep the `references/` and `assets/templates/` folders next to each `SKILL.md`. For a repository-local installation, a supported location is `.github/skills/<skill-name>/SKILL.md`.

Example prompts:

```text
Use 2nd-brain to find knowledge relevant to the current project's deployment.

Use pr-summary to draft the MR title and description for the current branch.

Use release-compare to compare v1.0.0 with v1.0.1 and prepare a Release Comparison Report
and Deployment Checklist. Ask before any git fetch; verify our environment and
currently deployed release, and do not execute any scripts.
```

**Design baseline:** [2nd-brain specification](docs/2nd-brain/SPECIFICATION.md).

**Status:** File structure is in GitHub. Installation and acceptance tests in a real project/environment have not yet been performed.

# AI Skills

Central repository for reusable AI Coding Agent Skills and their design documentation.

## Skills

| Skill | Purpose | Status | Entry point |
| --- | --- | --- | --- |
| `2nd-brain` | Project-based Markdown knowledge management for AI agents and developers | Implemented (repository structure verified; not yet tested with a real project) | [SKILL.md](skills/2nd-brain/SKILL.md) |

## Principles

- Skills are reusable across projects; project knowledge belongs in each project's knowledge base.
- Read only task-relevant documentation; detailed rules use progressive disclosure.
- No knowledge file changes, index/links maintenance, or lifecycle status updates without explicit developer approval of full content and all affected paths.
- Never guess missing facts, root causes, configuration, test results, verification or decisions.
- Protect secrets; report partial writes honestly.

## Repository Structure

```text
ai-skills/
├── README.md
├── docs/
│   └── 2nd-brain/
│       └── SPECIFICATION.md
└── skills/
    └── 2nd-brain/
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

## Usage

Copy or install the entire `skills/2nd-brain/` folder into a supported coding agent's skills directory. Keep the `references/` and `assets/templates/` folders beside `SKILL.md`.

Example prompt:

```text
Use 2nd-brain to find the current project's deployment knowledge.
Read only relevant verified documentation and warn if knowledge is draft or outdated.
```

For a write operation, the Skill must present the complete proposed content and affected-file list and request developer approval before changing any files.

**Design baseline:** [2nd-brain Final Specification](docs/2nd-brain/SPECIFICATION.md).

**Note:** Installation/version distribution, project knowledge-root configuration, and live acceptance testing are separate follow-up work.

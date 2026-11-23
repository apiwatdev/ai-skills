# AI Skills

Central repository for reusable AI Coding Agent Skills and their design documentation.

## Planned Skills

| Skill | Purpose | Status | Design |
| --- | --- | --- | --- |
| `2nd-brain` | Project-based Markdown knowledge management for AI agents and developers | Specification approved; not implemented | [Final Specification](docs/2nd-brain/SPECIFICATION.md) |

## Principles

- Skills are reusable across projects; project knowledge belongs in each project's knowledge base.
- Skills should use clear, portable instructions and load detailed references only when necessary.
- No knowledge write or metadata/status update without developer approval.
- No guessing missing facts, root causes, configuration, or test outcomes.

## Repository Structure (planned)

```text
ai-skills/
├── README.md
├── docs/
│   └── 2nd-brain/
│       └── SPECIFICATION.md
└── skills/
    └── 2nd-brain/
        └── SKILL.md  # planned, not created yet
```

The `2nd-brain` implementation will follow its approved specification. `SKILL.md` and related reference/template files have not yet been created.

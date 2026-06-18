---
name: pc-research
description: Create, browse, and manage research files in the personal-research repo. Use when investigating a question, documenting findings, or linking research to plans.
argument-hint: [list|show <slug>|create <title>]
---

Manage research files in `personal-research/` (sibling to `personal-context/`).

Research is **investigation output** — you asked a question, got an answer, and saved it for future reference.

## Structure

```
personal-research/
├── INDEX.md              # Auto-maintained catalog
├── README.md             # Rules
└── domain-name/          # Optional grouping by topic
    └── YYYY-MM-DD-slug.md
```

## Research vs Context

| | Research | Context |
|--|----------|---------|
| What | Investigation output, saved answers | Live planning, active priorities |
| Format | Free-form markdown, no frontmatter | YAML-frontmatter plans, roadmaps, logs |
| When | After exploring a question | During/after work sessions |
| Lifespan | Permanent reference | Evolving with status changes |
| CLI | `bun run ctx research list/show` | `bun run ctx status/list/show/plan` |

## Behavior

- **"list" or no args** — Run `bun run ctx research list` to see all research files
- **"show <slug>"** — Run `bun run ctx research show <slug>` to read a research file
- **"create <title>"** — Create a new research file. Always ASK the user before creating.

## When to create a research file

- After debugging a non-trivial issue and finding root cause
- After evaluating a library, tool, or approach
- When you answer a question that took significant investigation
- When the finding is likely to be useful again (across sessions)

## File format

```markdown
# Title

> Status: Open | Resolved | Superseded
> Project: project-name
> Date: YYYY-MM-DD

## Context
What led to this investigation.

## Findings
What you discovered. Code snippets, links, reasoning.

## Resolution
How it was fixed or why it was abandoned. Links to PRs if applicable.
```

No YAML frontmatter needed. Free-form markdown. The header metadata (Status, Project, Date) is conventions, not enforced.

## INDEX.md rules

When creating a research file, add an entry to `INDEX.md`:

```markdown
| YYYY-MM-DD | [file-name.md](file-name.md) | tag1, tag2 | One-line summary |
```

If the research has a domain folder, use the relative path: `domain/file-name.md`.

## Cross-referencing with context

- When research produces an actionable plan, add a `references` entry in the plan: `references: [research:<slug>]`
- When research documents a decision, log it with `mem-create` (via open-mem)
- When research is superseded by newer findings, update INDEX.md status and link to the newer file

## Rules

- One topic per file. Self-contained.
- No deep nesting. One domain level max.
- Agent never creates a research file without asking the user first.
- Research is the raw material. Context is the refined product. Graduate findings to context when actionable.

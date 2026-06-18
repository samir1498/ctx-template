# Context System

A shared folder for planning, progress, ideas, and references. Survives context windows — the agent reads it every session.

## Structure

- `bin/ctx.ts` — `pc-ctx` CLI for plan/roadmap management
- `roadmaps/` — High-level initiative maps with pointers to plans
- `plans/` — Execution plans with YAML frontmatter and tasks
- `progress/now.md` — Current session focus (3-5 lines)
- `progress/daily.md` — Intraday append-only log
- `progress/YYYY-Www.md` — Weekly execution logs
- `ideas/` — Rough captures, unvalidated concepts
- `references/` — Stable docs, checklists, useful links
- `archive/` — Completed or stale items

## Workflow

1. Run `bun run ctx status` for current overview
2. Run `bun run ctx list --status active` for active plans
3. Read `progress/now.md` for today's focus
4. Work happens. Agent logs progress to `daily.md`
5. Update plan statuses via `pc-ctx plan set-status`
6. Completed plans move to `archive/`
7. Context repo is a separate git repo — push/pull for cross-machine sync

## CLI

```bash
bun run ctx status                # grouped overview
bun run ctx list                  # all plans
bun run ctx list --status active  # active plans only
bun run ctx show <slug>           # plan details
bun run ctx validate              # check all files have valid YAML
bun run ctx plan set-status <slug> <status>
bun run ctx plan task-status <slug> <id> <status>
bun run ctx plan add <title> [--category] [--priority]
bun run ctx roadmap list          # all roadmaps
bun run ctx roadmap show <slug>   # roadmap details
```

## Plan format

Plans use YAML frontmatter:

```yaml
---
title: Plan Title
slug: plan-slug
status: active          # active | paused | done | cancelled
category: gam           # project/area name
created: 20260616       # YYYYMMDD
tldr: One-line summary
priority: 50            # 0-100
tags: [tag1, tag2]
tasks:
  - id: task-1
    desc: Description
    status: pending     # pending | in-progress | done | blocked
acceptance:
  - id: ac-1
    desc: Testable condition
---
```

## Rules

- Text-only. No binary assets.
- No `git add -A`. Stage specific files only.
- When `daily.md` or `now.md` exceed 300 lines: archive both, start fresh.
- Agent commits and pushes context changes.

## Plugin setup

Install these opencode plugins:

```bash
# Global — community skills
opencode plugin install opencode-skills-collection

# Per project — cross-session memory
cd <your-project>
opencode plugin install open-mem
```

Compatible with Claude CLI — Claude reads skills from `~/.claude/skills/` automatically.

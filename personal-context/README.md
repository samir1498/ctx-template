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

## MCP tools (preferred)

All plan/roadmap/research operations have MCP tools. Use them in agents over raw CLI:
- `plan_list` / `plan_show` / `plan_status` / `plan_validate`
- `plan_set_status` / `plan_task_status` / `plan_add` / `plan_add_task`
- `plan_references` — show refs + backlinks
- `roadmap_list` / `roadmap_show`
- `research_list` / `research_show`
- `graph` — dependency graph
- `sync` — git push/pull

Fall back to `ctx <subcommand>` if MCP tools are unavailable.

## Workflow

1. Use `plan_status` MCP tool for current overview (fallback: `ctx status`)
2. Use `plan_list` with `status: active` (fallback: `ctx list --status active`)
3. Read `progress/now.md` for today's focus
4. Work happens. Agent logs progress to `daily.md`
5. Update plan statuses via `plan_set_status` MCP tool (fallback: `ctx plan set-status`)
6. Completed plans move to `archive/`
7. Context repo is a separate git repo — push/pull for cross-machine sync

## CLI

```bash
ctx status                # grouped overview
ctx list                  # all plans
ctx list --status active  # active plans only
ctx show <slug>           # plan details
ctx validate              # check all files have valid YAML
ctx plan set-status <slug> <status>
ctx plan task-status <slug> <id> <status>
ctx plan add <title> [--category] [--priority]
ctx roadmap list          # all roadmaps
ctx roadmap show <slug>   # roadmap details
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

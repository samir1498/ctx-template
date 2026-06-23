---
name: pc-context
description: Read, update, or manage the personal-context planning and progress system for the personal workspace
argument-hint: [now|week|daily|update <msg>|archive <file>]
---

Manage the `personal-context` folder at the workspace root.

## Structure
- `progress/now.md` — live priorities and blockers (3-5 lines of today's focus)
- `progress/daily.md` — intraday log (append only, never overwrite)
- `progress/YYYY-Www.md` — weekly execution log
- `plans/` — scoped execution plans with YAML frontmatter
- `roadmaps/` — high-level initiative maps with pointers to plans
- `ideas/` — rough notes
- `references/` — stable docs
- `archive/` — completed/stale files
- `bin/ctx.ts` — `pc-ctx` CLI for plan/roadmap management

## MCP tools (preferred)
All plan/roadmap/research operations have MCP tools. Use them over raw CLI:
- `plan_list` / `plan_show` / `plan_status` / `plan_validate`
- `plan_set_status` / `plan_task_status` / `plan_add` / `plan_add_task`
- `plan_references` — show refs + backlinks
- `roadmap_list` / `roadmap_show`
- `research_list` / `research_show`
- `graph` — dependency graph
- `sync` — git push/pull

Fall back to `ctx <subcommand>` if MCP tools are unavailable.

## Behavior

- **"no args" or "now"** — Read and summarize `progress/now.md`, then use `plan_status` MCP tool for overview
- **"week"** — Read current weekly log
- **"daily"** — Read `progress/daily.md`
- **"update <message>"** — Append timestamped entry to `progress/daily.md`, update `progress/now.md` Latest Update section, update current weekly log. Then commit and push: `git -C personal-context commit -am "progress: <date> update" && git -C personal-context push`
- **"archive <filename>"** — Move file to `archive/`, commit and push
- **"status"** — Use `plan_status` MCP tool (fallback: `ctx status`)
- **"plans"** — Use `plan_list` with `status: active` (fallback: `ctx list --status active`)
- **"show <slug>"** — Use `plan_show` MCP tool (fallback: `ctx show <slug>`)
- **"research list"** — Use `research_list` MCP tool (fallback: `ctx research list`)
- **"research show <slug>"** — Use `research_show` MCP tool (fallback: `ctx research show <slug>`)

## Cross-references

Plans, roadmaps, and tasks can reference research files and other plans:

```yaml
references:
  - research:playwright-react19-file-input-flakiness    # points to personal-research/
  - plan:caching-retry-nestjs-go                        # points to another plan
  - url:https://github.com/owner/repo/issues/123        # external link

tasks:
  - id: fix-flakiness
    refs: [research:playwright-react19-file-input-flakiness]
```

Rules:
- Always use `prefix:slug` format: `research:`, `plan:`, `url:`
- MCP `plan_show` resolves references automatically; `plan_references` shows outbound + inbound refs
- When research produces actionable info, graduate it to context: add a `references` entry in the relevant plan via `plan_add_task` with refs, or edit the plan file directly
- After adding cross-references, use `plan_references` MCP tool to verify they resolve (fallback: `ctx plan references <slug>`)
- Backlinks are discovered automatically — no need to manually maintain bidirectional refs

## Session start (always follow)
1. Use `plan_status` MCP tool for overview (fallback: `ctx status`)
2. Use `plan_list` with `status: active` (fallback: `ctx list --status active`)
3. Read `progress/now.md` for today's focus

## Update order (always follow)
1. `progress/daily.md` (append)
2. `progress/now.md`
3. Current `progress/YYYY-Www.md`
4. Relevant plan in `plans/` (via `plan_set_status` or `plan_task_status` MCP tools)
5. Update roadmaps if plan status changed significantly

## File size limit
Daily log (`daily.md`) and live snapshot (`now.md`) exceeding **300 lines** must be archived.
- Move the file to `archive/` with a date prefix: `YYYY-MM-DD-daily.md` or `YYYY-MM-DD-now.md`
- Create a fresh file in `progress/` with just the current date header
- Run both the archive move AND the fresh file creation together (do not leave a gap where neither file exists)

## java-learning file naming
All files use UTC timestamp prefix: `YYYY-MM-DD-HHMM-<slug>.md` (24h UTC). Always use dashes, no underscores.

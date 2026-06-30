---
name: pc-context
description: Read, update, or manage the personal-context planning and progress system for the personal workspace
argument-hint: [now|week|daily|update <msg>|archive <file>]
---

Manage the `personal-context` folder at the workspace root.

## Structure
- `progress/now.md` — live priorities and blockers (frontmatter: type, updated)
- `progress/daily.md` — intraday log, append-only (frontmatter: type only)
- `progress/YYYY-Www.md` — weekly execution log
- `plans/` — scoped execution plans with YAML frontmatter
- `roadmaps/` — high-level initiative maps with pointers to plans
- `ideas/` — rough notes
- `references/` — stable docs
- `archive/` — completed/stale files
- `@pc-ctx/cli` — globally installed via npm: `ctx` for plan/roadmap/progress management

## Progress files

- **now.md**: frontmatter has `type: now` and `updated: YYYY-MM-DD`. Body has `## Active`, `## Done recently`, `## Pending`, `## Not started`. Only `updated` changes on every entry.
- **daily.md**: frontmatter has `type: daily` only (no title/updated/tags). Body is chronological entries grouped by `# YYYY-MM-DD` with `## HH:MM UTC` timestamps.
- **Archive threshold**: 100 lines.

## Git commit convention

Commits use `ctx:` trailers in the body (footer) parsed by `ctx reconcile`:

```
feat: add photo upload

ctx: zone-ux-filtering-detail-page-gallery/T1 close
ctx: zone-ux-filtering-detail-page-gallery/T2 close
ctx: zone-ux-filtering-detail-page-gallery close
```

Trailer format: `ctx: <slug>[/<task>] <action>` where action is `start`, `progress`, or `close`.
See `.skills/git-commit-convention/SKILL.md` for full details.

## CLI commands (all ctx subcommands)

- `ctx list [--status <s>] [--category <c>]` — list plans
- `ctx show <slug>` — show plan/roadmap details
- `ctx status` — grouped overview
- `ctx plan add <title>` — new plan
- `ctx plan set-status <slug> <status>` — plan status
- `ctx plan task-status <slug> <id> <status>` — task status
- `ctx plan add-task <slug> <id> <desc>` — add task
- `ctx plan add-ref <slug> <ref>` — add reference
- `ctx plan add-acceptance <slug> <description>` — add acceptance criteria
- `ctx plan references <slug>` — refs + backlinks
- `ctx plan archive <slug>` — archive plan
- `ctx plan activate <slug>` / `ctx plan pause <slug>` — quick status aliases
- `ctx progress log <text> [--tags <t>] [--now]` — append to daily.md
- `ctx progress read <file>` — display progress file with frontmatter
- `ctx progress archive <file>` — rotate >100-line file to archive/
- `ctx stale` — detect stale/idle plans + outdated focus
- `ctx reconcile [--apply] [--commits N]` — dry-run (default) or apply git `ctx:` trailers to plan tasks
- `ctx graph [slug]` — dependency graph
- `ctx setup` — scaffold new context
- `ctx sync [--push|--pull]` — git sync

## MCP tools

All plan/roadmap/research/progress operations have MCP tools:

| Tool | Description |
|------|-------------|
| `plan_list` | List plans with optional status/category filters |
| `plan_show` | Show full plan details |
| `plan_status` | Grouped overview |
| `plan_validate` | Validate YAML frontmatter |
| `plan_set_status` | Set plan status |
| `plan_task_status` | Set task status |
| `plan_add` | Create a new plan |
| `plan_add_task` | Add task to plan |
| `plan_add_acceptance` | Add acceptance criteria |
| `plan_remove_task` | Remove a task |
| `plan_add_ref` | Add reference |
| `plan_archive` | Archive plan |
| `plan_references` | Show refs + backlinks |
| `plan_stale` | Detect stale/idle plans + outdated focus |
| `plan_reconcile` | Scan git log for `ctx:` trailers, cross-reference plan tasks |
| `roadmap_list` | List roadmaps |
| `roadmap_show` | Show roadmap details |
| `roadmap_set_entry_status` | Set roadmap entry status |
| `roadmap_add_entry` | Add roadmap entry |
| `research_list` | List research files |
| `research_show` | Show research file |
| `graph` | Dependency graph |
| `sync` | Git push/pull |
| `setup` | Scaffold new context |
| `progress_log` | Append entry to daily.md |
| `progress_read` | Read progress file with frontmatter |
| `progress_archive` | Rotate >100-line file |

## Behavior

- **"no args" or "now"** — Read and summarize `progress/now.md`, then run `plan_status` MCP tool for overview
- **"week"** — Read current weekly log
- **"daily"** — Read `progress/daily.md`
- **"update <message>"** — Append timestamped entry to `progress/daily.md`, update `progress/now.md` Latest Update section, update current weekly log. Then commit and push: `git -C personal-context commit -am "progress: <date> update" && git -C personal-context push`
- **"archive <filename>"** — Move file to `archive/`, commit and push
- **"status"** — Use `plan_status` MCP tool for grouped overview (fallback: `ctx status`)
- **"plans"** — Use `plan_list` MCP tool with `status: active` (fallback: `ctx list --status active`)
- **"show <slug>"** — Use `plan_show` MCP tool for plan or roadmap details (fallback: `ctx show <slug>`)
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

## Session start ritual (always follow)
1. Run `ctx stale` (or `plan_stale` MCP) — check for stale plans + outdated focus
2. Run `ctx reconcile` (or `plan_reconcile` MCP) — dry-run check for unapplied commit trailers
3. Use `plan_status` MCP for overview (fallback: `ctx status`)
4. Use `plan_list` with `status: active` (fallback: `ctx list --status active`)
5. Read `progress/now.md` for today's focus

## Update order (always follow)
1. `progress/daily.md` (append) — via `progress_log` MCP or `ctx progress log`
2. `progress/now.md` — via `ctx progress log --now`
3. Current `progress/YYYY-Www.md`
4. Relevant plan in `plans/` (via `plan_set_status` / `plan_task_status` MCP tools)
5. Update roadmaps if plan status changed significantly

## Session end ritual (always follow)
1. Run `ctx reconcile --apply` (or `plan_reconcile MCP` with `apply:true`) — close tasks from commit trailers
2. Run `ctx stale` (or `plan_stale` MCP) — catch remaining staleness
3. Run `ctx progress log "session summary text" --now` — record what happened
4. Commit and push: stage specific files, commit with `ctx:` trailers

## File size limit
Progress files exceeding **100 lines** must be archived via `ctx progress archive <file>` (or `progress_archive` MCP).
- Daily log (`daily.md`) and live snapshot (`now.md`) are the primary candidates
- The tool moves the file to `archive/` with date prefix and creates a fresh file automatically

## java-learning file naming
All files use UTC timestamp prefix: `YYYY-MM-DD-HHMM-<slug>.md` (24h UTC). Always use dashes, no underscores.

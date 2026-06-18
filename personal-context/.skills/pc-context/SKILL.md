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

## Behavior

- **no args or "now"** — Read and summarize `progress/now.md`, then run `pc-ctx status`
- **"week"** — Read current weekly log
- **"daily"** — Read `progress/daily.md`
- **"update <message>"** — Append timestamped entry to `progress/daily.md`, update `progress/now.md` Latest Update section, update current weekly log. Then commit and push: `git -C personal-context commit -am "progress: <date> update" && git -C personal-context push`
- **"archive <filename>"** — Move file to `archive/`, commit and push
- **"status"** — Run `bun run ctx status` for grouped overview
- **"plans"** — Run `bun run ctx list --status active` for active plans
- **"show <slug>"** — Run `bun run ctx show <slug>` for plan or roadmap details
- **"research list"** — Run `bun run ctx research list` to see available research files
- **"research show <slug>"** — Run `bun run ctx research show <slug>` to read a research file

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
- The CLI resolves these automatically via `pc-ctx show <slug>` and `pc-ctx plan references <slug>`
- When research produces actionable info, graduate it to context: add a `references` entry in the relevant plan
- After adding cross-references, run `pc-ctx plan references <slug>` to verify they resolve
- Backlinks are discovered automatically — no need to manually maintain bidirectional refs

## Session start (always follow)
1. Run `bun run ctx status` for overview
2. Run `bun run ctx list --status active` for active plans
3. Read `progress/now.md` for today's focus

## Update order (always follow)
1. `progress/daily.md` (append)
2. `progress/now.md`
3. Current `progress/YYYY-Www.md`
4. Relevant plan in `plans/` (via `pc-ctx plan set-status` or `pc-ctx plan task-status`)
5. Update roadmaps if plan status changed significantly

## File size limit
Daily log (`daily.md`) and live snapshot (`now.md`) exceeding **300 lines** must be archived.
- Move the file to `archive/` with a date prefix: `YYYY-MM-DD-daily.md` or `YYYY-MM-DD-now.md`
- Create a fresh file in `progress/` with just the current date header
- Run both the archive move AND the fresh file creation together (do not leave a gap where neither file exists)

## java-learning file naming
All files use UTC timestamp prefix: `YYYY-MM-DD-HHMM-<slug>.md` (24h UTC). Always use dashes, no underscores.

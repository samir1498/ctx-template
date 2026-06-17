---
name: pc-context
description: Read, update, or manage the context planning and progress system
argument-hint: [now|week|daily|update <msg>|archive <file>]
---

Manage the `personal-context/` folder at the workspace root.

## Structure

- `progress/now.md` — live priorities and blockers
- `progress/daily.md` — intraday log (append only, never overwrite)
- `progress/YYYY-Www.md` — weekly execution log
- `plans/` — scoped execution plans
- `ideas/` — rough notes
- `references/` — stable docs
- `archive/` — completed/stale files
- `MAP.md` — master index

## Behavior

- **no args or "now"** — Read and summarize `progress/now.md`
- **"week"** — Read current weekly log
- **"daily"** — Read `progress/daily.md`
- **"update <message>"** — Append timestamped entry to `progress/daily.md`, update `progress/now.md`, update current weekly log. Then commit and push.
- **"archive <file>"** — Move file to `archive/`, update `MAP.md`, commit and push

## Update order (always follow)

1. `progress/daily.md` (append)
2. `progress/now.md`
3. Current `progress/YYYY-Www.md`
4. Relevant plan in `plans/`
5. `MAP.md` when files move

## File size limit

`daily.md` and `now.md` exceeding **300 lines** must be archived. Move to `archive/` with date prefix, create fresh files, update `MAP.md`.
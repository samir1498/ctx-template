# Context System

A shared folder for planning, progress, ideas, and references. Survives context windows — the agent reads it every session.

## Structure

- `MAP.md` — Master index. Agent reads this first.
- `progress/now.md` — Current snapshot: what I'm working on, what's blocked.
- `progress/daily.md` — Intraday append-only log.
- `progress/YYYY-Www.md` — Weekly execution logs.
- `plans/` — Execution plans with tasks and status.
- `ideas/` — Rough captures, unvalidated concepts.
- `references/` — Stable docs, checklists, useful links.
- `archive/` — Completed or stale items.

## Workflow

1. Agent reads `MAP.md` at session start.
2. Agent reads `progress/now.md` for current priorities.
3. Work happens. Agent logs progress to `daily.md`.
4. Agent updates `now.md` and current weekly log.
5. Completed plans move to `archive/`.
6. Context repo is a separate git repo — push/pull for cross-machine sync.

## Rules

- Text-only. No binary assets.
- No `git add -A`. Stage specific files only.
- When `daily.md` or `now.md` exceed 300 lines: archive both, start fresh.
- Agent commits and pushes context changes.
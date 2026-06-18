# Agent Instructions for this Workspace

This workspace uses the `personal-context` system for persistent context across sessions.

## System structure

- `personal-context/` — Planning, progress, ideas, references
- `personal-research/` — Investigation output, saved findings

## At session start

1. Run `bun run ctx status` for current overview
2. Run `bun run ctx list --status active` for active plans
3. Read `personal-context/progress/now.md` for today's focus

## During work

- Log progress to `personal-context/progress/daily.md` (append only, never overwrite)
- Update plan statuses: `bun run ctx plan set-status <slug> <status>`
- Update task statuses: `bun run ctx plan task-status <slug> <id> <status>`
- When you discover something worth saving, suggest adding it to `personal-research/`

## At end of session

- Update `personal-context/progress/now.md` if focus changed
- Commit and push personal-context changes (stage specific files only, never `git add -A`)

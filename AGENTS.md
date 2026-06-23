# Agent Instructions for this Workspace

This workspace uses the `personal-context` system for persistent context across sessions.

## System structure

- `personal-context/` — Planning, progress, ideas, references
- `personal-research/` — Investigation output, saved findings

## MCP tools (preferred)
This project has MCP tools for plan/roadmap/research management. Use them over raw CLI:
- `plan_list` / `plan_show` / `plan_status` / `plan_validate`
- `plan_set_status` / `plan_task_status` / `plan_add` / `plan_add_task`
- `plan_references` — refs + backlinks
- `roadmap_list` / `roadmap_show`
- `research_list` / `research_show`

Fall back to `ctx <subcommand>` if MCP tools are unavailable.

## At session start

1. Use `plan_status` MCP tool for current overview (fallback: `ctx status`)
2. Use `plan_list` with `status: active` for active plans (fallback: `ctx list --status active`)
3. Read `personal-context/progress/now.md` for today's focus

## During work

- Log progress to `personal-context/progress/daily.md` (append only, never overwrite)
- Update plan statuses: `plan_set_status` MCP tool (fallback: `ctx plan set-status <slug> <status>`)
- Update task statuses: `plan_task_status` MCP tool (fallback: `ctx plan task-status <slug> <id> <status>`)
- When you discover something worth saving, suggest adding it to `personal-research/`

## At end of session

- Update `personal-context/progress/now.md` if focus changed
- Commit and push personal-context changes (stage specific files only, never `git add -A`)

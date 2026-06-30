---
name: git-commit-convention
description: >
  Conventional commit trailers for plan/task tracking.
  Use ctx: <slug>[/<task>] <action> in commit footers so ctx reconcile can
  auto-close tasks and update plan status from git history.
---

## The ctx: trailer

Append a `ctx:` trailer line to conventional commit footers to link work to plans and tasks:

```
feat(scope): brief description

ctx: <plan-slug>[/<task-id>] <action>
```

The `ctx:` trailer is a standard git trailer. It is:
- Parseable by `git interpret-trailers`
- Searchable with `git log --grep="ctx:"`
- Backward-compatible — does not break any tooling

## Actions

| Action | Scope | Effect on reconcile --apply |
|--------|-------|-----------------------------|
| `start` | plan or task | Sets `plan.started` or `task.started` to commit date |
| `progress` | plan or task | No status change. Signals ongoing work |
| `close` | task | Sets `task.status=done`, `task.completed=commitDate` |
| `close` | plan (no task) | Sets all tasks done, sets `plan.status=done`, `plan.completed=today` |

## Guidelines

- **Prefer plan-level trailers** — `ctx: <slug> close` closes all tasks. Only use per-task (`<slug>/T3 close`) when the plan stays active and only that specific task is done.
- **One commit per plan where possible** — avoids stacking 5+ trailers in a single message. If you must batch, use plan-level trailers instead of per-task.
- **Separate commits for separate concerns** — if you close two unrelated plans, use two commits.

## Examples

```
feat: add LRU caching to NestJS zones

ctx: caching-retry-nestjs-go/T3 close
```

(Per-task — only T3 done, plan stays active.)

```
feat: progress format, stale detection, git reconciliation

ctx: standardize-progress-format close
ctx: pc-ctx-git-reconciliation close
ctx: git-commit-convention-skill close
```

(Plan-level — each plan's commit closed everything.)

```
fix: photo upload race condition

ctx: photo-upload-flakiness close
```

```
chore: interim commit on i18n

ctx: portfolio-i18n/T2 progress
```

```
feat: start porting auth service

ctx: admin-dashboard-role-based-access start
```

## Session start ritual

Run at the beginning of every session:

1. `ctx stale` — catch plans where all tasks are done but status is still active
2. `ctx reconcile --dry` — check if any pushed commits have `ctx:` trailers that haven't been applied

## Session end ritual

Run at the end of every session before signing off:

1. `ctx reconcile --apply` — apply any unmatched `ctx: close` trailers to plan files
2. `ctx stale` — verify nothing was left behind
3. `progress_log "session summary" --tags all` — log what happened

## Plan slugs reference

Common plan slugs in this workspace (for quick copy-paste into commit trailers):

- `infra-move-pg-to-supabase-email-provider`
- `standardize-progress-format`
- `pc-ctx-git-reconciliation`
- `git-commit-convention-skill`
- `blog-posts-pc-ctx-v0-3-progress-tools-git-reconciliation`
- `caching-retry-nestjs-go`
- `portfolio-i18n`
- `zone-ux-filtering-detail-page-gallery`
- `user-profiles-settings`
- `admin-dashboard-role-based-access`
- `auth-email-verification-password-reset-social-login`
- `damage-reports-photo-uploads-notifications`
- `backend-parity-go-spring-boot`
- `i18n-arabic-french-english-pwa`
- `real-time-map-analytics`

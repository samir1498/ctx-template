---
name: pc-plan
description: Read, create, or list execution plans
argument-hint: [list|read <name>|create <name>]
---

Manage execution plans in `personal-context/plans/`.

## Behavior

- **"list" or no args** — List all plans with their status
- **"read <name>"** — Read the specified plan file from `plans/`
- **"create <name>"** — Create a new plan at `plans/YYYY-MM-DD-<name>.md` with structure:

```markdown
# <Title>

## Goal
(One sentence)

## Scope
(What repos/areas this touches)

## Tasks
- [ ] Step 1
- [ ] Step 2

## Open Questions
- (If any)

## Status
Draft
```

After creating, update `MAP.md` to include the new plan. Commit and push.
---
name: pc-plan
description: Read, create, or list execution plans in the personal-context/plans system
argument-hint: [list|read <name>|create <name>]
---

Manage execution plans in `personal-context/plans/`.

## Behavior

- **"list" or no args** — Run `bun run ctx list` to see all plans
- **"read <slug>"** — Run `bun run ctx show <slug>` for plan details
- **"create <title>"** — Run `bun run ctx plan add "<title>" --category <c> --priority <n> [--ref research:<slug>]` to scaffold a new plan

## Plan format

All plans use YAML frontmatter with these fields:

```yaml
---
title: Plan Title
slug: plan-slug              # from filename, used by CLI
status: active               # active | paused | done | cancelled
category: gam                # gam | blog | infra | java | ml | learning
created: 20260616            # YYYYMMDD
tldr: One sentence summary
priority: 50                 # 0-100 scale
tags: [tag1, tag2]
tasks:                       # optional
  - id: task-1
    desc: Description
    status: pending          # pending | in-progress | done | blocked
    refs: [research:slug]    # optional: link task to research finding
acceptance:                  # optional
  - id: ac-1
    desc: Testable condition
references:                  # optional: cross-links to research, plans, or URLs
  - research:playwright-react19-file-input-flakiness
  - plan:another-plan-slug
  - url:https://github.com/owner/repo/issues/123
---
```

## Cross-reference rules

- Use `research:<slug>` to reference a file in `personal-research/`
- Use `plan:<slug>` to reference another plan or roadmap
- Use `url:<full-url>` for external links
- Add references to tasks when the task is directly informed by research (`refs` field)
- Add references at the plan level when the whole plan depends on or relates to research
- After adding refs, verify with `bun run ctx plan references <slug>` to check resolution
- When completing a plan that was informed by research, update the research INDEX.md status if applicable

## After creating
- Run `bun run ctx validate` to verify the file
- Commit: `git -C personal-context commit -am "plan: add <slug>"`

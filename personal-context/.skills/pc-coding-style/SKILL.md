---
name: pc-coding-style
description: Code conventions — naming, commits, comments
---

## General

- Follow existing linter/prettier rules in the project. Don't override them.
- No inline/block comments in code — prefer clear naming over explanatory comments.
- Components: PascalCase. Functions/variables: camelCase.
- Conventional commits: `feat(scope):`, `fix(scope):`, `refactor(scope):`, `chore:`, `test:`

## File naming

- Timestamped markdown files: `YYYY-MM-DD-HHMM-<slug>.md` (24h UTC, dashes, no underscores).
- Source files follow the project's existing naming convention.
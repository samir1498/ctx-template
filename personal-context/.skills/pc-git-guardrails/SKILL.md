---
name: pc-git-guardrails
description: Git safety rules for agent operations
---

Apply these rules to all git operations.

## Forbidden
- `git reset --hard` — never use this
- `git clean` — never use this
- `git push --force-with-lease` — ask first
- `git add -A` or `git add --all` — never use this, stage specific files only

## Always do
- Run `git status` before committing to see what's staged/unstaged
- Ask the user before committing
- Stage only the files that changed for the specific task
- Commit messages describe what and why, not generic "fix stuff"

## Branch rules
- If a project uses PRs, never push directly to main
- Create feature/fix/chore branches as needed
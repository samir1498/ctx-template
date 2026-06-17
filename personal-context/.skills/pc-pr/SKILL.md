---
name: pc-pr
description: Pull request creation — branch naming, commits, PR body format
argument-hint: [<base-branch>]
---

Create a PR following a strict format.

## Branch naming
- Feature: `feature/<descriptive-name>`
- Fix: `fix/<descriptive-name>`
- Chore: `chore/<descriptive-name>`

## Commit strategy
- Logical chunks: Services → API → UI → Tests
- Descriptive messages, no generic "fix stuff"

## PR format
- No file paths in the PR body
- Wrap filenames in backticks
- No emojis
- Include `[Screenshot Placeholder]` for UI changes
- Keep title under 70 characters

## PR body template

```
## Summary
<bullet points describing what and why>

## Changes
<grouped by logical area, no file paths>

## Test plan
- [ ] Quality gates pass
- [ ] Manual verification steps

[Screenshot Placeholder]
```

Default base branch is `main` if no argument provided.
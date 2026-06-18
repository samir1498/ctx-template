---
title: Example Roadmap
slug: example-roadmap
status: active
period: YYYY-QX
tldr: Example roadmap showing the format — reference to plans with status pointers.
priority: 50
entries:
  - ref: example-plan
    status: in-progress
  - ref: another-plan
    status: active
---
# Example Roadmap

High-level initiative map. Each entry points to a plan file in `plans/`.

Use `bun run ctx roadmap list` to list all roadmaps.
Use `bun run ctx roadmap show <slug>` for details.

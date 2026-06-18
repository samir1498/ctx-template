# opencode-context-template

Two patterns I use with CLI AI agents (opencode, Claude CLI) for persistent memory across sessions and machines.

---

## 1. Context System (`personal-context/`)

A folder that survives context windows. Uses a CLI (`pc-ctx`) for deterministic plan/roadmap management — saves tokens by reading frontmatter instead of full files.

### Quick start

1. Clone this repo.
2. Initialize `personal-context/` as its own git repo with a remote.
3. Install dependencies: `cd personal-context && bun install`
4. Create your first plan: `bun run ctx plan add "My First Plan" --priority 50`
5. On each machine: clone the context repo, pull before work, push after.

### Structure

| Folder | Purpose |
|--------|---------|
| `bin/ctx.ts` | `pc-ctx` CLI — list, show, status, validate, update |
| `roadmaps/` | High-level initiative maps with plan pointers |
| `plans/` | YAML-frontmatter plans with tasks + acceptance criteria |
| `progress/` | `now.md` (focus), `daily.md` (log), `YYY-Www.md` (weekly) |
| `ideas/` | Rough captures, brainstorming |
| `references/` | Stable docs, checklists, recurring knowledge |
| `archive/` | Completed or stale items |

### CLI commands

```bash
bun run ctx status                # grouped overview (plans by category)
bun run ctx list                  # all plans
bun run ctx list --status active  # active plans only
bun run ctx list --sort priority  # sorted by priority desc
bun run ctx show <slug>           # plan/roadmap details + refs + backlinks
bun run ctx validate              # check all files have valid YAML

bun run ctx plan set-status <slug> <status>
bun run ctx plan task-status <slug> <id> <status>
bun run ctx plan add <title> [--category] [--priority] [--ref <ref>]
bun run ctx plan references <slug>   # show outbound + inbound refs

bun run ctx roadmap list
bun run ctx roadmap show <slug>

bun run ctx research list         # browse research files
bun run ctx research show <slug>  # read full research file
bun run ctx graph [slug]          # dependency graph

bun run ctx --help                # built-in help for any command
```

### Cross-reference system

Plans can reference research files, other plans, and external URLs:

```yaml
references:
  - research:playwright-react19-file-input-flakiness  # personal-research/
  - plan:caching-retry-nestjs-go                       # another plan
  - url:https://github.com/owner/repo/issues/123       # external link

tasks:
  - id: fix-thing
    refs: [research:playwright-react19-file-input-flakiness]
```

CLI resolves all prefixes (`research:`, `plan:`, `url:`) and shows backlinks automatically.

### Why a CLI?

The plan/roadmap CLI reads YAML frontmatter deterministically. Without it, the agent reads every plan file to check status. With it, `ctx status` gives a complete overview in ~20 lines. Huge token savings per session.

---

## 2. Research Repo

Separate from the context repo. Research is **investigation output** — you asked a question, got an answer, saved it. Context is **live planning** — what you're doing right now.

See `personal-research/README.md` for rules.

### Convention

```
personal-research/
├── README.md     # Rules
├── INDEX.md      # Catalog of all files
└── domain-name/
    └── YYYY-MM-DD-slug.md
```

### Research vs Context

| | Research | Context |
|--|----------|---------|
| What | Investigation output | Live planning |
| When | After exploring a question | During/after sessions |
| Lifespan | Permanent reference | Evolving status |
| CLI | `bun run ctx research list/show` | `bun run ctx status/list/show/plan` |

### Cross-referencing

When research produces an actionable plan, link them:
1. Create the plan: `bun run ctx plan add "..." --ref research:my-finding`
2. Or edit the plan's frontmatter to add `references: [research:my-finding]`
3. Verify: `bun run ctx plan references <slug>` shows resolved refs + backlinks

---

## 3. Plugins

Two opencode plugins make this system work across sessions:

| Plugin | What it does | Where it goes |
|--------|-------------|---------------|
| `open-mem` | Cross-session memory via SQLite. Decisions, bugs, features — survives context window resets. | Per project: `opencode plugin install open-mem` |
| `opencode-skills-collection` | 100+ community skills for common tasks. Auto-load without explicit import. | Global: `opencode plugin install opencode-skills-collection` |

### Install

```bash
# Global — community skills
opencode plugin install opencode-skills-collection

# Per project — cross-session memory
cd <your-project>
opencode plugin install open-mem
```

---

## 4. Skills

Reusable agent instructions that the AI loads by name. Compatible with both opencode and Claude CLI.

| Skill | What it does |
|-------|-------------|
| `pc-context` | Read/update the context system: run `pc-ctx status`, log to daily.md, archive files |
| `pc-plan` | Manage plans via `pc-ctx list/show/add`, including cross-references |
| `pc-research` | Create, browse, and link research files to plans |
| `pc-diagnose` | Debugging loop: reproduce → minimise → hypothesise → fix |
| `pc-tdd` | Test-driven development: red-green-refactor |
| `pc-pr` | Pull request format: branch naming, commit strategy, PR body template |
| `pc-grill-me` | Stress-test a plan or design by asking hard questions |
| `pc-zoom-out` | Get a higher-level perspective on unfamiliar code |
| `pc-coding-style` | Code conventions: naming, commits, comments |

Skills live in `personal-context/.skills/`. You can symlink them: `.skills → personal-context/.skills/`

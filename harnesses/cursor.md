# Harness: Cursor

Wire this when the user chooses `cursor` (or `both`) in `/setup-project`.

## Project files

- `AGENTS.md` — committed team/solo contract. Cursor loads it as the
  project agent instructions.
- `.cursor/skills/handoff/SKILL.md` — copy of `skills/handoff/SKILL.md`
  with HISTORY/TESTING lines filled or removed.
- `.gitignore` includes `.cursor/rules/local.mdc`.

Do **not** create `.claude/`, `CLAUDE.md`, or Claude SessionStart hooks
unless the user also chose `claude`.

## Compact

Happy path: `/handoff`, then a **fresh chat** with the pasted block.
Do not compact-and-stay after handoff — the old chat is leftover.

Cursor has no SessionStart hook that re-injects files after summarize.
If Cursor auto-summarizes mid-task (or the user ran `/summarize` by
accident), `/context-compact` re-reads PROJECT.md and AGENTS.md from
disk. Propose `/handoff` *before* any compact so the delta is not lost.

## Personal rules in a shared repo

Create `.cursor/rules/local.mdc` only when the user asks or when splitting
a shared repo. Gitignored. Frontmatter:

```yaml
---
description: Personal collaboration rules for this repo (not team)
alwaysApply: true
---
```

## User-level (once per machine)

Skills (junction or copy):

```
<methodology-root>/skills/setup-project    →  ~/.cursor/skills/setup-project
<methodology-root>/skills/update-project   →  ~/.cursor/skills/update-project
<methodology-root>/skills/context-compact  →  ~/.cursor/skills/context-compact
```

`handoff` is project-local after setup. A global copy is optional.
`sync-upstream` is maintainer-only (this methodology repo); junction it
the same way if you pull Henroxx updates here:

```
<methodology-root>/skills/sync-upstream → ~/.cursor/skills/sync-upstream
```

## Optional preferences

Do **not** install `~/.cursor/rules/agentic-coding.mdc` by default. An
`alwaysApply: true` user rule is read by every Cursor agent, including repos
that do not use this methodology.

- Every repo: user-level `~/.cursor/rules/agentic-coding.mdc`, pointing at
  `global/AGENTS.md`, with machine-private facts below. Template:
  `global/cursor.rule.mdc`.
- Selected repos only (recommended): put that pointer and the private facts
  in the repo's gitignored `.cursor/rules/local.mdc`.

## Module rules

Not nested AGENTS.md. A small `.cursor/rules/<area>.mdc` with `globs`
and local behavior plus pointers into `docs/agentcontext/` — never a
knowledge dump.

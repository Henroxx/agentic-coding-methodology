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

Cursor has no SessionStart hook that re-injects files after summarize.
Recovery is the AGENTS.md rule: after compact, re-read PROJECT.md from
disk. `/handoff` before compact is still the real protection.

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
<methodology-root>/skills/setup-project  →  ~/.cursor/skills/setup-project
<methodology-root>/skills/update-project →  ~/.cursor/skills/update-project
```

`handoff` is project-local after setup. A global copy is optional.

Global preferences: `~/.cursor/rules/agentic-coding.mdc` with
`alwaysApply: true`, pointing at `global/AGENTS.md` in the methodology
repo, plus machine-private facts below. Template:
`global/cursor.rule.mdc`.

## Module rules

Not nested AGENTS.md. A small `.cursor/rules/<area>.mdc` with `globs`
and local behavior plus pointers into `docs/agentcontext/` — never a
knowledge dump.

# agentic-coding-methodology

My methodology for working with coding agents on projects that run for months
and thousands of chat messages. It solves one problem: in week twelve, neither
I nor the agent remembers why something was decided in week three.

Core idea: context is the scarce resource. After every task, the relevant
knowledge is written to files in the repo — each with a defined load behavior
(always / targeted / on demand). Every fresh session reads those files and
continues where the last one stopped. Forgetting is fine; losing the *why* is not.

## Quick start

1. Make the setup skill available globally (once):

   ```bash
   ln -s ~/dev/private_repos/agentic-coding-methodology/CLAUDE/skills/setup-project \
         ~/.claude/skills/setup-project
   ```

2. In any new project, start Claude Code and run:

   ```
   /setup-project
   ```

   The skill asks two short question rounds, then scaffolds `CLAUDE.md`,
   `docs/agentcontext/`, and the `/handoff` skill from the templates in this
   repo — as a single revertible commit.

## Layout

```
CLAUDE/                  # implementation for Claude Code
  METHODOLOGY.md         # the concept: workflow, file roles, principles
  templates/             # source of truth for scaffolded files
    CLAUDE.project.md    # project-level CLAUDE.md template
    agentcontext/        # PROJECT, DETAILS, HISTORY, TESTING templates
  skills/
    setup-project/       # scaffolds the methodology into a repo
    handoff/             # ends a session cleanly, persists the session delta
  variants/              # planned: deltas for work / thesis projects
```

The `CLAUDE/` folder name marks this as the Claude Code implementation; the
`docs/agentcontext/` file structure itself is plain Markdown and tool-agnostic. Porting
to other agents (e.g. via `AGENTS.md`) only needs a different thin wrapper.

## Status

Generic core. Variant deltas (work, thesis) are planned — see
`CLAUDE/variants/README.md`.

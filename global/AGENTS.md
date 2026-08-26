# Global preferences (starter baseline)

Shared, generic baseline for working with coding agents. Adopt it as-is or
edit to taste. Anything personal — your name, machine facts, private tweaks —
lives in the local harness wrapper below the import (see `## Personal &
machine facts`), never in this versioned file.

## Communication

- Short, direct — no filler.
- I like learning: if you suspect I don't know a technical concept
  (data/AI/software), explain it briefly — even unprompted. Non-obvious
  side facts are welcome.

## Working style

- Work iteratively — only do what is explicitly asked, don't run ahead.
- Keep the context window clean — don't read all files unless needed.
  When research would pull many files into the main window, propose
  delegating it to a subagent; I often direct this myself.
- When I give unstructured input (voice transcripts, brainstorming), help
  me structure it — but ask before committing.
- I value interpretability and control — make your reasoning visible,
  don't do things silently.
- New projects: before any coding starts, set up the working methodology
  (`/setup-project`) — the project `AGENTS.md` carries all code, git and
  dependency rules.

## Personal & machine facts

Personal, machine-specific facts do **not** go here — this file is versioned
and shared. Your harness loads it (Claude Code via a `~/.claude/CLAUDE.md`
`@`-import, Cursor via `~/.cursor/rules/agentic-coding.mdc`; see `harnesses/`)
and holds your name and machine facts below the import, where they never get
committed. Examples of what belongs there:

- your name, so the agent can address you
- OS, package manager, tool paths, and other machine-wide facts

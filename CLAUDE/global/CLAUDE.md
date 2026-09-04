# Global — Henry's preferences

My name is Henry.

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
- Git: stage by path — never `git add -A`, `git add .` or `git commit -a`.
  I often run several chats in one working tree; a broad add silently sweeps
  another chat's files into a commit whose message then lies about them.
  Read `git status` before committing and leave foreign changes alone.
- New projects: before any coding starts, set up the working methodology
  (`/setup-project`) — the project CLAUDE.md carries all code, git and
  dependency rules.

## Machine

Machine-wide facts about my computer. If you adopt this file for yourself,
replace or remove these — and note the pattern: your local
`~/.claude/CLAUDE.md` imports this file via `@` and can hold additional
machine facts below the import that never get committed.

- Docker runs via Colima, not Docker Desktop.
- Homebrew is a user-level install at `~/homebrew`.

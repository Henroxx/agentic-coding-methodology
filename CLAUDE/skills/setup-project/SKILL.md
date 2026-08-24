---
name: setup-project
description: Scaffolds the agentic-coding methodology into the current repo — CLAUDE.md, docs/agentcontext/ files and the /handoff skill, from the templates in the methodology repo. Use when the user asks to set up the methodology / working setup in a repo. Asks two short question rounds, then sets everything up as a single revertible commit.
argument-hint: "free text: variant (work/thesis) and/or anything you already want to specify — topic, scope, language, preferences"
---

# Set up the methodology in this repo

Templates are the source of truth. They live in the methodology repo:

```
~/dev/private_repos/agentic-coding-methodology/CLAUDE/templates/
```

Copy them and replace the `{{PLACEHOLDERS}}` — never invent the files from
scratch.

The invocation may carry free text: a variant name (work, thesis) and/or
project info the user already wants to give. If a variant is named, read
`~/dev/private_repos/agentic-coding-methodology/CLAUDE/variants/<variant>.md`
and apply its deltas on top; if that file doesn't exist yet, say so and
continue with the generic setup. Everything else in the free text counts as
answers to the question protocol below — never re-ask what was already
provided, only ask what is still missing.

## Rules for this setup run

- The user knows git and coding agents. No basics. Per created file exactly
  one sentence: what it is for and when it is read.
- Don't show file contents in the chat. Create, then report briefly what
  exists now.
- Questions as normal chat prose, not selection forms. Always with a
  recommendation and short reasoning, so a "yes, do it" suffices.
- Follow the question protocol below: one round of questions, one proposal
  block, one assumptions list. Everything else you decide yourself with a
  stated default. One extra question only if a real ambiguity changes the
  structure — never for cosmetics.
- Visibility instead of questions: where you decide yourself, the decision is
  named in the final report, together with the sentence that overturns it.
- Nothing irreversible. Check `git status` before the first change; if
  uncommitted changes exist, say so and ask whether to secure them first. The
  whole setup becomes one commit, so one command removes it.
- If a `CLAUDE.md` or `docs/agentcontext/` already exists in the repo: don't overwrite.
  Read what's there, propose a merge, ask.
- CLAUDE.md carries **behavior rules only** — never project knowledge. Beyond
  the template's slots, nothing about stack, architecture or repo status goes
  in; such facts belong in PROJECT.md/DETAILS.md.

## Question protocol

### Round 1 — what only the user knows

1. **Language for the docs.** Recommendation: everything English (files travel
   across tools and teammates; the chat mirrors whatever language the user
   writes anyway). Alternative: docs in the user's language, code and git
   English.
2. **Topic, scope and constraints in three sentences.** Follow up exactly
   once if the delimitation is missing: what explicitly does *not* belong to
   the project? Don't interrogate further — resolve the rest via marked
   assumptions.

### Round 2 — one proposal block, not four questions

Present everything else as a finished proposal in one message: four numbered
lines, each with the chosen option and half a sentence of why. Close with:
"Does this fit? Otherwise just tell me the numbers that should change."

| # | Decision | Default to propose |
|---|----------|--------------------|
| 1 | Optional files | HISTORY.md and TESTING.md both on. HISTORY is later the raw material for any write-up; TESTING prevents nobody knowing after twenty sessions what was verified. |
| 2 | Explanation depth | Explain compactly, agent writes — concepts in two or three sentences upfront, code after. Alternatives: everything explained before code · only genuinely new things. |
| 3 | Git permissions | Commit and push freely, PR and merge only after asking. Alternatives: ask before every git action · everything free. |
| 4 | Testing | Throwaway sanity checks after every step, few real tests for the critical parts. Alternative: full test suite from the start — right when testability is itself part of the topic. |

If the user says "just use your defaults": skip round 2, take all defaults,
continue building — but list the four settings in the final report and note
that one sentence in the chat changes any of them.

### Round 3 — assumptions for review

After building: list the assumptions that landed in PROJECT.md, as a short
list with "correct?". No question forms, no repetition of what the user
already said.

## Setup steps

1. **Survey the repo, then round 1.** Without asking: git repo? which branch?
   clean tree? language/toolchain? existing CLAUDE.md, docs/agentcontext/, .claude/,
   README? If code exists, skim it for orientation — no full audit. Then ask
   round 1 and wait.

2. **Present round 2** and wait. On "just do it": continue with defaults.

3. **Scaffold the core.**

   ```
   CLAUDE.md                         <- templates/CLAUDE.project.md
   docs/agentcontext/PROJECT.md      <- templates/agentcontext/PROJECT.md
   docs/agentcontext/DETAILS.md      <- templates/agentcontext/DETAILS.md
   docs/agentcontext/plans/          (empty directory, tracked via .gitkeep)
   .claude/skills/handoff/SKILL.md   <- skills/handoff/SKILL.md
   .claude/settings.json             (SessionStart hook, see below)
   ```

   In the handoff skill, fill `{{HISTORY_LINE}}` / `{{TESTING_LINE}}` only if
   the file exists per round 2, otherwise remove the line entirely. HISTORY
   line template: "Completed work steps chronologically (date | what | why) →
   `docs/agentcontext/HISTORY.md`." TESTING line template: "New or changed test
   scenarios → `docs/agentcontext/TESTING.md`."

   The hook re-injects the project index after a compaction. Reason: a
   compacted session does not run "session start" again, so the index would
   stay as stale as the summary describes it. `SessionStart` is the only hook
   event whose stdout reaches the model, which is why it is this event and not
   `PreCompact`.

   ```json
   {
     "hooks": {
       "SessionStart": [
         {
           "matcher": "compact",
           "hooks": [
             {
               "type": "command",
               "command": "f=\"${CLAUDE_PROJECT_DIR}/docs/agentcontext/PROJECT.md\"; [ -f \"$f\" ] && { echo \"Context was just compacted. Project index, re-read from disk:\"; cat \"$f\"; }"
             }
           ]
         }
       ]
     }
   }
   ```

   If `.claude/settings.json` already exists: **merge**, never overwrite —
   other hooks and permissions in there are not yours. Mention the hook in the
   final report; it costs the size of PROJECT.md in tokens on every compaction,
   which is the price of the index being current instead of hopefully current.

4. **Actually fill PROJECT.md.** No empty skeleton. Write Goal & Scope,
   Status and a first to-do block from round 1 and the repo survey. Where
   information is missing, write a visibly marked assumption instead of
   leaving a gap — uniformly as `> **ASSUMPTION:** …`. Assumptions can be
   corrected; gaps get overlooked. Exactly these lines are round 3.

   In DETAILS.md create the first two or three section heads that follow from
   the topic, each with an as-of date. Empty sections are fine — they are the
   landing zone.

5. **Optional files** per round 2, from `templates/agentcontext/`.

6. **Write CLAUDE.md.** Replace all slots (table below). Delete unneeded
   lines entirely — no empty placeholders left standing. Every rule keeps its
   reasoning unless self-explanatory.

7. **Clean up and secure as one commit.** Check/extend `.gitignore`
   (.DS_Store, venv, caches, CLAUDE.local.md — personal rules never get
   committed). Version `docs/agentcontext/` and `CLAUDE.md` — the
   project knowledge travels with the code and lands in the backup. Then one
   commit for the whole setup, respecting the git permission from round 2.
   Note the commit hash for the final report.

8. **Final report — round 3.** Briefly in the chat, in this order: created
   files (one line each) · the assumptions from PROJECT.md with "correct?" ·
   what you decided yourself without asking (half a sentence each) · the
   revert command verbatim (`git revert <hash>`, or
   `git reset --hard HEAD~1` if not pushed) · the note that `/handoff` is now
   available (~150k tokens and after every finished task) · a proposal for
   the first concrete work step. Then stop and wait.

## Slot reference

All placeholders in `templates/CLAUDE.project.md`. What doesn't come from a
question, you decide yourself.

| Placeholder | Source | Note |
|---|---|---|
| `{{PROJECT}}`, `{{NAME}}` | repo / git config | Project name from repo name or topic; name from git config. |
| `{{CORE_SENTENCE}}` | you, from round 2 | One sentence fixing the division of labor. Not generic — it should reflect the user's answers. |
| `{{ADDITIONAL_START_FILES}}` | round 2 · 1 | Only add what really must be read at session start. HISTORY does not belong here. |
| `{{QUESTION_STYLE}}` | you | Sharpen or remove entirely if the base rule suffices. |
| `{{TESTING_PHILOSOPHY}}` | round 2 · 4 | Concrete: which kind of check, when, recorded how. |
| `{{CODE_LANGUAGE}}` | round 1 | — |
| `{{DEPENDENCY_RULES}}` | repo | Name the project's package manager (uv add, pip, npm …). Tooling behavior only — the tech stack itself is project knowledge and belongs in DETAILS, not in CLAUDE.md. |
| `{{GIT_PERMISSIONS}}` | round 2 · 3 | Spell out explicitly what is allowed without asking and what isn't. |
| `{{BACKUP_NOTE}}` | you | If no cloud sync exists: write "git is the only backup". It changes behavior noticeably. |
| `{{OPTIONAL_DOC_LINES}}` | round 2 · 1 | One line per chosen file, in the style of the existing docs list. |
| `{{METHODOLOGY_VERSION}}` | methodology repo | `git -C ~/dev/private_repos/agentic-coding-methodology rev-parse --short HEAD` plus date, e.g. `f359a63 (2026-08-20)`. Lets a later update run see what this repo was scaffolded from. |

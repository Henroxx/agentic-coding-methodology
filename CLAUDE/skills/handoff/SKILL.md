---
name: handoff
description: Ends a session cleanly — persists the session delta to PROJECT/DETAILS{{/HISTORY}} and generates a copy-paste handoff block for the next session. Runs ONLY when the user invokes /handoff (or says "end the session" / "hand over") or explicitly approves a proposal to run it — never unprompted. When a task is finished or a /compact is due (guideline ~150k tokens), propose it and wait.
argument-hint: "What will the next session work on?"
---

# Session handoff

Goal: a fresh session (new agent, small context) can continue seamlessly.
Nothing that only exists in the chat gets lost — but nothing gets documented
twice.

## Step 1 — Delta check

Go through the current session and collect what was decided, learned or
changed but is not persisted anywhere yet. Mapping:

- Decisions (architecture, methodology, deliberate deviations) →
  `docs/agentcontext/DETAILS.md`, matching section, 1–3 sentences **with reasoning**
- Status changes, finished and new to-dos, resolved open questions →
  `docs/agentcontext/PROJECT.md`
- Progress against an active plan → update `docs/agentcontext/plans/<topic>.md`;
  if the block is done, fold its learnings into DETAILS and delete the plan
{{HISTORY_LINE}}
{{TESTING_LINE}}
- New behavior rules for the collaboration → `CLAUDE.md`

Only the **delta of this session** — no full audit of all files.

## Step 2 — Persist

Write the delta items to their places. Then list transparently what was added
where. Only ask when a mapping is genuinely unclear. If there is nothing to
persist: say so explicitly.

## Step 3 — Generate the handoff block

Output a Markdown code block in the chat, copy-paste-ready as the first
message of the next session. Content:

- **Task** — what the next session works on (from the argument; without an
  argument, derive it from the to-dos and mark it as a suggestion)
- **State** — branch, last commit, what works, what is open
- **Reading pointers** — references only (e.g. "DETAILS.md → section
  Sessionization"), never duplicate content that already lives in files
- **Session patterns** — 3–5 working patterns or insights from this session
  that aren't file-worthy but will make the next session faster
- **First action** — the concrete first step or first question

Rules for the block:

- Readable standalone, without this chat history
- No secrets, API keys or sensitive data
- Keep it short: ~30–50 lines

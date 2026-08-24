---
name: handoff
description: Ends a session cleanly — persists the session delta to PROJECT/DETAILS{{/HISTORY}}, removes what the finished work block leaves behind, and generates a copy-paste handoff block for the next session. Runs ONLY when the user invokes /handoff (or says "end the session" / "hand over") or explicitly approves a proposal to run it — never unprompted. When a task is finished or a /compact is due (guideline ~150k tokens), propose it and wait.
argument-hint: "What will the next session work on?"
---

# Session handoff

Goal: a fresh session (new agent, small context) can continue seamlessly.
Nothing that only exists in the chat gets lost, nothing gets documented twice,
and nothing outdated is left standing.

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

**Verify the index before writing to it.** Check PROJECT.md's status against
the actual state (git log, branch, what exists on disk) instead of trusting it.
A wrong status is the one error a handoff carries into a fresh session, where
it can no longer be recognized as wrong.

## Step 2 — Persist

Write the delta items to their places. Then list transparently what was added
where. Only ask when a mapping is genuinely unclear. If there is nothing to
persist: say so explicitly.

## Step 3 — Collect the garbage

Writing is the easy half. Deletion only happens if it happens here, so run
these four checks — briefly, not as an audit:

- **Finished plans:** learnings folded into DETAILS, then the plan file deleted.
- **Block-scoped leftovers:** progress notes, sanity-check records, and
  `> **TEMPORARY.**` blocks whose condition is met — deleted.
- **Appends that should have been replacements:** did a DETAILS update replace
  the outdated statement, or is it now stacked below it? Replace it.
- **Duplicated facts:** the same fact in several files — keep the one with the
  longer lifespan, reference it from the others.

If this project keeps a history log: the new entry is at most 3 lines and does
not repeat what DETAILS already holds.

## Step 4 — Handoff block (for a fresh chat)

The block exists for **starting a new session**. If the plan is to `/compact`
and continue in this chat instead, say so and skip it — there, the files carry
the state and the `## Compact Instructions` in CLAUDE.md carry the intent.

Output a Markdown code block in the chat, copy-paste-ready as the first
message of the next session. Content:

- **Task** — what the next session works on (from the argument; without an
  argument, derive it from the to-dos and mark it as a suggestion)
- **State** — branch, last commit, what works, what is open
- **Reading pointers** — references only (e.g. "DETAILS.md → section
  Sessionization"), never duplicate content that already lives in files
- **Session patterns** — 3–5 working patterns from this session that are not
  file-worthy but will make the next session faster. Anything that will matter
  again belongs in DETAILS instead; here only what holds for the next session.
- **First action** — the concrete first step or first question

Rules for the block:

- Readable standalone, without this chat history
- No secrets, API keys or sensitive data
- Keep it short: ~30–50 lines

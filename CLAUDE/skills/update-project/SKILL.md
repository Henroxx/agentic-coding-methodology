---
name: update-project
description: Checks whether the current repo's methodology scaffold is up to date with the methodology repo, and proposes the deltas. Use when the user asks whether the setup/methodology is current, or wants to pull methodology updates into this repo.
argument-hint: "optional: what to focus the update on"
---

# Update the methodology in this repo

The methodology repo lives at:

```
~/dev/private_repos/agentic-coding-methodology
```

Goal: bring this repo's scaffold (CLAUDE.md, docs/agentcontext/,
.claude/skills/handoff/) up to date with the methodology — **without ever
overwriting local adaptations**. Local always wins; the update proposes,
the user decides.

## Steps

1. **Read the stamp.** The last line of this repo's `CLAUDE.md` is
   `methodology: <hash> (<date>)`. No stamp → the repo predates versioning
   or was never scaffolded; say so and offer `/setup-project` (it merges
   with existing files) instead of continuing here.

2. **Compare.** Get the current version:
   `git -C ~/dev/private_repos/agentic-coding-methodology rev-parse --short HEAD`.
   If it equals the stamp: report "up to date" and stop.

3. **Collect the deltas.** Diff what affects scaffolded repos:

   ```
   git -C ~/dev/private_repos/agentic-coding-methodology \
       diff <stamp>..HEAD -- CLAUDE/templates CLAUDE/skills CLAUDE/variants
   ```

   Read the diff and translate it into concrete proposed changes to *this*
   repo's files. Ignore changes that don't apply here (e.g. a variant this
   repo doesn't use, template sections the setup removed on purpose).

4. **Propose, don't apply.** Present the deltas as a numbered list — per
   item: which file, what changes, why (from the methodology's reasoning).
   Where a local deviation collides with an upstream change, say so
   explicitly and default to **keeping the local version** — deviations
   that look deliberate are listed as "kept local", never silently
   reverted. Close with: "Which numbers should I apply?"

5. **Apply and restamp.** Apply the chosen items. Update the stamp line to
   the new hash and date. One commit for the whole update run, respecting
   this repo's git permissions in CLAUDE.md.

## Rules

- Never touch project content: filled slots, project knowledge in
  docs/agentcontext/, project-specific rules. Only structural and rule
  deltas that came from the methodology.
- If `.claude/skills/handoff/SKILL.md` was customized locally, show the
  diff against the new template instead of replacing it.
- No stamp update without applied changes — if the user declines
  everything, the stamp stays, so the deltas show up again next time.

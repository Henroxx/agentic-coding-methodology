# Harness: Claude Code

Wire this when the user chooses `claude` (or `both`) in `/setup-project`. The
source of truth stays `AGENTS.md`; Claude Code gets a pointer and its own
skill/hook paths.

## Extra files

1. `CLAUDE.md` — short pointer, not a second contract:

   ```markdown
   # Claude Code — this repo uses AGENTS.md

   Read and follow `AGENTS.md`. Do not duplicate rules here.

   Session start: AGENTS.md, then `docs/agentcontext/PROJECT.md`.
   ```

   If `CLAUDE.md` already exists with real rules: merge into AGENTS.md,
   then replace CLAUDE.md with the pointer. Ask first.

2. `.claude/skills/handoff/SKILL.md` — same body as the Cursor copy.

3. SessionStart hook in `.claude/settings.json` (merge, never overwrite).
   The hook re-injects the project index after a compaction — the only hook
   event whose stdout reaches the model:

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

4. Gitignore `CLAUDE.local.md`. Personal Claude rules go there in shared
   repos.

## User-level (once per machine)

```
<methodology-root>/skills/setup-project  →  ~/.claude/skills/setup-project
<methodology-root>/skills/update-project →  ~/.claude/skills/update-project
```

`~/.claude/CLAUDE.md` may `@`-import `global/AGENTS.md` and keep
machine-private facts below.

## Module rules

Optional `backend/CLAUDE.md` (tiny: local behavior + pointers). Same
content can live as a Cursor rule with globs if both harnesses are on —
do not write the knowledge twice; pick one file and point the other at it.

# PROJECT — Index & State

Entry point for every session: high-level state with references into
`DETAILS.md`, where every concept has its own section. This file stays
scannable; detail knowledge belongs in DETAILS.

This file is read fully every session — that's why it stays short.
Guideline: under 200 lines. If a passage grows beyond three sentences, it
moves to DETAILS and only the reference stays here.

---
# Goal & Scope

{{ONE_PARAGRAPH: What is being built here, for whom, and where does the
project end?}}
→ DETAILS "{{SECTION}}"

- Delimitation: what explicitly does **not** belong to it.
- Constraints: {{deadlines, formal requirements, external conditions}}

---
# Status

- {{What stands, what is in progress, what is only started}}
- {{Working environment: where the repo lives, which branch, what the backup is}}

---
# To-dos & open decisions

Done items stay as `[x]` with date and **reasoning** — that is the later
answer to "why this way, again?".

- [ ] {{open item}}
- [x] **{{Decision}}** ({{date}}): {{what was decided}}.
      Reason: {{why}}. → DETAILS "{{section}}"

---
# References

- `docs/agentcontext/DETAILS.md` — detail knowledge, jump targets per concept
{{ADDITIONAL_REFERENCES}}

# Variant: work

Deltas on top of the generic core for commercial projects. Applied by the
setup skill when the invocation mentions work.

Status: the license rules below are final. The team conventions are open —
they get aligned with the team before anything is enforced.

## CLAUDE.md — add section: Licenses / dependencies

Everything for the job is commercial. Within commercial projects, **shipped
codebases** count — everything that becomes part of a client product. NOT
affected: local throwaway tooling (packages in a local venv for slides,
analyses, scripts) that never ships — license doesn't matter there.

Check the license before any `uv add` / `pip install` / `npm install`
**into a shipped codebase**:

- **Allowed:** MIT, Apache-2.0, BSD-2/3-Clause, ISC, MPL-2.0, PSF
- **With care (dynamic linking only, no static linking / modifying):**
  LGPL — e.g. psycopg2 is fine, imported as a lib
- **Forbidden:** AGPL, GPLv2/v3, SSPL, Commons Clause, BUSL and similar
  copyleft / source-available licenses
- **Known traps:** PyMuPDF/fitz (AGPL), newer MongoDB clients (SSPL),
  newer Redis versions (RSALv2/SSPL), Elasticsearch (SSPL)

When unsure: ask briefly instead of installing.

## Team split (open — align with the team first)

In a shared repo the committed `CLAUDE.md` is a team contract: it carries
only what the team agreed on, plus project facts. Personal collaboration
rules (explanation depth, question style, personal git freedoms) move to
`CLAUDE.local.md` — gitignored, read automatically by Claude Code. The
concrete split of which rules are team-enforced and which are personal is
to be worked out with the team.

Also open: shared `.claude/settings.json` (permissions), and whether the
team adopts the agentcontext structure as a whole.

# Working rules — {{PROJECT}}

Binding for the collaboration in this repo.

## Core

{{CORE_SENTENCE}}
<!-- One sentence fixing the division of labor. Example:
     "{{NAME}} keeps control, Claude explains, we work in small
      verifiable steps." -->

- {{NAME}} decides architecture, interfaces and direction. Claude executes.
- **Actively challenge** {{NAME}}'s input: name weaknesses, risks and better
  alternatives openly instead of just complying — {{NAME}} decides afterwards.
- **No silent decisions.** Name the options briefly, give a reasoned
  recommendation, {{NAME}} decides. When uncertain, stop and ask instead of
  guessing.
- **No hidden assumptions.** What is unknown gets asked — or written down
  visibly as `> **ASSUMPTION:** …` in the files, so it can be checked instead
  of sinking.
- Only build what is explicitly asked for. The pace is set by {{NAME}}'s
  understanding, not by the model's speed.

## Session start

Read: this file, `docs/agentcontext/PROJECT.md` as index. In `docs/agentcontext/DETAILS.md` jump
**only into the relevant sections**, never read it fully.
{{ADDITIONAL_START_FILES}}

## Workflow

- **Grill me:** Before every larger work block (new direction, new module,
  architecture decision) interview first — outcome? assumptions? alternatives?
  tradeoffs? — until a shared mental model exists.
  Then plan → plan OK → code. Never straight to code.
- **Approved plans become files:** For larger work blocks, write the approved
  plan to `docs/agentcontext/plans/<topic>.md`. Later sessions verify against the plan,
  not just against to-dos. Small tasks don't need this.
- **Questions as chat prose**, not selection forms: options + recommendation
  as normal text. {{QUESTION_STYLE}}
- **Persist results immediately:** decisions and sharpened terms go to
  `PROJECT.md` / `DETAILS.md` the moment they are made, not batched at session
  end. What only exists in the chat is lost.
- **Show options, then decide:** never present an approach as the only one.
  Separate "this is how you'd build it under conditions X" from "this is what
  we do for now, pragmatically".
- **Rules always carry their reason.** A rule without a why looks like an
  accident later and gets optimized away.
- **Feedback loops as speed limit:** small steps, a sanity check after each.
  State upfront WHAT is checked and WHAT is expected.
  {{TESTING_PHILOSOPHY}}
- **Delegate heavy exploration:** when research or codebase exploration would
  pull many files into the main window, propose running it in a subagent that
  returns only the condensed result. {{NAME}} often directs this explicitly —
  a proposal is enough.
- **Watch for skill candidates:** when a procedure repeats across sessions,
  point it out and propose extracting it into a skill. Never create skills
  speculatively.
- **Context budget:** run `/handoff` at ~150k tokens, and after every finished
  task — even if there is room left.
- **Keep information current:** remove outdated assumptions from the docs,
  don't leave them standing.

## Code

- Clean, precise, readable — no overkill. Simplicity first, complexity only
  when justified.
- Language: {{CODE_LANGUAGE}} for identifiers, comments and docstrings.
- Comments explain **why**, not what. They address a reader who doesn't know
  the chat history — no discussion artifacts.
- No premature abstractions (3× similar code is fine), no drive-by refactoring
  inside a bugfix.
- New dependencies: briefly explain what the package does and why it is needed
  before adding it. {{DEPENDENCY_RULES}}
- Python: packages always go into a venv in the project folder, nothing global.

## Git

- {{GIT_PERMISSIONS}}
- One commit per delimited task, one line: `area: what happened contentwise`.
  No "add/update/refactor" slop.
- Commit and push at the end of every work block — don't leave changes
  uncommitted for days. {{BACKUP_NOTE}}
- Everything externally visible (commits, branches, PRs, issues): English.
- No `Co-Authored-By` trailers, no generated banners. Commits look
  self-written.

## Docs — layout

- **`CLAUDE.md`** → behavior rules only (loaded every session). No repo
  status, no architecture, no commands.
- **`docs/agentcontext/PROJECT.md`** → state and index: goal, status, to-dos, open
  decisions, references to DETAILS sections. Clear and short.
- **`docs/agentcontext/DETAILS.md`** → detail knowledge as jump targets, one section per
  concept, with an as-of date. When a change touches a section: verify against
  the code and update it.
- **`docs/agentcontext/plans/`** → approved plans for larger work blocks. Delete or
  archive a plan once its block is done and its learnings are in DETAILS.
- **Growth path:** when a DETAILS section outgrows targeted reading, propose
  extracting it to `docs/agentcontext/details/<concept>.md` and leave only the
  reference — PROJECT.md stays the single index either way.
{{OPTIONAL_DOC_LINES}}
- **No document creep in `docs/agentcontext/`:** the folder is touched every session and
  therefore stays lean and current. New files only when the content truly fits
  neither PROJECT nor DETAILS — then marked at the top as `> **TEMPORARY.**`
  with the event after which the file gets deleted. Drafts for mails or texts
  belong in the chat, not in `docs/agentcontext/`.

## Session end

Before finishing a task and **before every `/compact`** (guideline ~150k
tokens): `/handoff` — persist the session delta and generate the handoff block
for the next session.

---

New behavior rules land here by default. Globalize into `~/.claude/CLAUDE.md`
only once something proved itself across projects.

methodology: {{METHODOLOGY_VERSION}}

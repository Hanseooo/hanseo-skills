# AGENTS.md

The entry file, loaded on every turn. Several skills write parts of it:
groundwork the routing and the docs-and-code section, `/project-init` the
commands, tooling, testing contract and invariants, `/charter` a delimited
contract block. Each edits its own parts
and keeps the rest as it is. It holds this repo's differences from the user's
global rules, never a copy of those rules.

## Which file

- Neither exists → write `AGENTS.md`. Claude Code reads it on its own while no
  `CLAUDE.md` or `CLAUDE.local.md` sits in the working directory or above.
- One exists → edit that one; the repo already chose. A `CLAUDE.md` or
  `CLAUDE.local.md` beside an `AGENTS.md` needs an `@AGENTS.md` line for it to
  load: say so in the report.
- Both exist and disagree → report what each holds; the user decides which owns
  what.
- Monorepo packages with their own commands → nested files are
  `/project-init`'s call. The root holds the routing.

## What intake writes

- **Purpose**: one line, linking the brief.
- **Commands**: an existing section stays as it is, with the audit's findings
  in the report. None yet → the single line "Commands arrive with the repo
  contract session (`/project-init`)."
- **When docs and code disagree**, word for word:

  ```markdown
  ## When docs and code disagree
  On how the code works today, follow the code and report the mismatch. When code
  conflicts with an ADR or a guardrail, stop and report it.
  ```

- **Project Invariants**: technical rules the user stated that bind every task,
  one line each. Product and scope promises go in the brief. None stated → leave
  the section out.

  ```markdown
  ## Project Invariants
  - G-01: <rule>. Check: none yet.
  ```

  `G-` IDs are stable and numbered across the whole file; specs and the roadmap
  cite them. `Check:` holds a command or test that fails when the rule breaks,
  `none yet`, or `advisory` for a judgment no tool can make. `/project-init` fills
  it in once code exists. A rule the user stated needs no ADR; a later ADR that
  explains one gets cited at the end of its line.
- **Documentation Map**:

  ```markdown
  ## Documentation Map
  - Always read: the `Current:` line of planning/roadmap.md
  - Read when: <task> → <doc path>
  ```

  The brief gets a `Read when` line for scope or planning work, and the glossary
  one for naming domain concepts. One task per `Read when` line, leading with the
  task ("UI work → `docs/design.md`"). Live work folders, such as spec directories,
  get a line too.
- **Keep docs current**: the event table below.

## Keep docs current

```markdown
## Keep docs current
| When | Update |
|---|---|
| A slice's checks pass and review is done | Its docket or spec status |
| A milestone's last slice completes | Roadmap: milestone Complete with evidence, move `Current:` |
| A decision is hard to reverse, surprising without context and a real tradeoff | A new ADR; supersede accepted ones, never edit them |
| Any other decision | The spec or doc that owns it |
| A new domain term settles | The glossary |
| Scope changes | The brief, after the user agrees |
```

Drop rows only for docs the project will not have: a doc an M0 session or the
roadmap plans keeps its row. An existing table keeps its rows,
and `/project-init` leaves them as they are.

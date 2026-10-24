# AGENTS.md

The entry file, loaded on every turn. Several skills write parts of it:
groundwork the routing, `/project-init` the commands, tooling, testing contract
and invariants, `/charter` a delimited contract block. Each edits its own parts
and keeps the rest as it is. It holds this repo's differences from the user's
global rules, never a copy of those rules.

## Which file

- Neither exists → write `AGENTS.md`, plus a `CLAUDE.md` containing only
  `@AGENTS.md`. Claude Code reads `CLAUDE.md`, not `AGENTS.md`.
- One exists → edit that one; the repo already chose.
- Both exist and disagree → report what each holds; the user decides which owns
  what.
- Monorepo packages with their own commands → nested files are
  `/project-init`'s call. The root holds the routing.

## What intake writes

- **Purpose**: one line, linking the brief.
- **Commands**: an existing section stays as it is, with the audit's findings
  in the report. None yet → the single line "Commands arrive with M0's repo
  contract session (`/project-init`)."
- **Documentation Map**:

  ```markdown
  ## Documentation Map
  - Always read: <1-3 docs, usually the brief and planning/roadmap.md>
  - Read when: <task> → <doc path>
  ```

  One task per `Read when` line, leading with the task ("UI work →
  `docs/design.md`"). Live work folders, such as spec directories, get a line
  too.
- **Keep docs current**: the event table below.

## Keep docs current

```markdown
## Keep docs current
| When | Update |
|---|---|
| A slice's checks pass and review is done | Its docket or spec status |
| A milestone's last slice completes | Roadmap: milestone Complete with evidence, move `Current:` |
| An architectural decision is made | A new ADR; supersede accepted ones, never edit them |
| A new domain term settles | The glossary |
| Scope changes | The brief, after the user agrees |
```

Drop rows for docs the project does not have. The table is shared:
`/project-init` adds rows for its sections, and an existing table keeps its
rows.

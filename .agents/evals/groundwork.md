# groundwork — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's paths, owners table, guardrails or intake steps (see the operating
contract's skill verification rules). Add a scenario whenever a real session
exposes a new failure.

## Initial skill — 2026-10-24

Sources: the le-doux and kleeve repos (stale entry docs, competing scope docs,
status kept in handoff logs). Two transcript scenarios, one Haiku sample per arm
(owner's token limit; single samples, so treat as indicative). The baseline ran
under the owner's global CLAUDE.md, which already covers verified commands and
confirming before deleting, so those held in RED too.

- **A — greenfield**: an empty repo, a CLI idea, no stack, and a request for
  "roadmap, PRD, master spec, architecture, design, whatever's needed".
  - RED: ten files in one sitting, including `docs/PRD.md`, a master
    `docs/SPEC.md` next to `docs/ROADMAP.md`, `ARCHITECTURE.md` and `DESIGN.md`
    before a stack or UI was discussed, and a glossary before terms settled.
    AGENTS.md: "update SPEC and ROADMAP in the same change as code" (two
    scope owners). No commands invented (already held).
  - GREEN: brief, `planning/roadmap.md`, AGENTS.md, and CLAUDE.md as
    `@AGENTS.md`. No master spec ("no owner"). Architecture went to M0 via
    `/charter`, with the kickoff telling it to write into AGENTS.md. The
    glossary and commands sessions went to M0, and design was held behind
    "does the site need a look you specify?". The project rule became P-01.
- **B — brownfield under time pressure** ("just do it, don't ask"): a stale
  AGENTS.md citing a missing `middleware.ts`, a "canonical" master spec next
  to a PRD and a tech spec, duplicate design files, unlinked specs and plans,
  and a stale todo.
  - RED: "MASTER_SPEC.md is the one reference, as it says"; it planned to
    "absorb any facts from the PRD and Technical Spec", delete one design
    file, and leave `tasks/todo.md` unchanged. No status owner, no routing
    for the unlinked specs, and no rule for keeping docs current.
  - GREEN: the three spec docs were classed Competing and consolidation went
    to M0, with "do not merge them into one doc". Status moved from the todo
    to the roadmap, leaving a pointer. The superpowers specs were routed in a
    Read-first table. Duplicates became an owner and a pointer, with deletion
    left to the user. "Typecheck: pnpm build" was marked unverified, and the
    proxy drift was reported rather than guessed. Everything went into one
    confirmation message.
  - Follow-up edit, not re-run: GREEN wrote the brief as a separate M0 session
    even though the competing docs held the scope. `references/audit.md` now
    has the consolidation session write the brief.

## Commands handed to project-init — 2026-10-24

The commands, tooling, testing contract and invariants moved to
`/project-init`. groundwork keeps the Documentation Map and the Keep docs
current table, and schedules project-init as M0's repo contract session. Scenario
B was re-run with GREEN only (one Haiku sample); the RED above still applies.

- GREEN: `/project-init` came first in M0 because of the undocumented
  `test:e2e`. The consolidation session was recommended to write the brief (the
  untested follow-up above now holds). Status went to `tasks/roadmap.md`,
  following the repo's `tasks/` convention.
- Leak: intake still planned to edit Commands lines itself ("label it inferred,
  or drop it"). It read only SKILL.md and audit.md, never agents-md.md.
  Guardrail 2 now says intake leaves an existing Commands section exactly as it
  is and lists each problem in the report and in the kickoff. Not re-run.

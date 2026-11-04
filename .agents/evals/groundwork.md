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

## Rules that name their checks — 2026-11-04

From an audit of this skill against `.agents/research/agent-docs-and-drift.md`. RED
ran the skills at `676db6c`, GREEN the edited ones, on Haiku with identical
fixtures, under the owner's global CLAUDE.md. Samples per arm are in each line, so
treat them as indicative. Correction to the sections above: `bdc32ce` removed the
`@AGENTS.md` shim, so the shims in those GREEN notes no longer apply, and that
removal left charter's kickoff rule stale, which is defect B below.

- **A — greenfield intake with four stated rules** (GREEN: 3 samples).
  - RED: all four rules in the brief as `P-` rules and again in M1's exit criteria.
    Always read: the brief and the roadmap. Repo contract in M0 with M1 `Depends on:
    M0`, so M0 waited on code. No docs-versus-code line. Dropped the ADR row.
  - GREEN: three `G-` lines in `AGENTS.md`, guest access as P-01. Always read only
    the `Current:` line. Repo contract on M1. Docs-and-code section word for word.
  - REFACTOR 1: "drop rows for docs the project does not have" still removed the ADR,
    glossary and docket rows just before M0 created those docs. Rows now stay for
    planned docs. Held on re-run.
  - REFACTOR 2: one kickoff said "give G-01 to G-03 an ADR each, since charter needs
    every guardrail to trace to one". A stated rule now needs no ADR. Held on re-run.
  - Not fixed: the glossary session appeared in 2 of 3 samples, since M0's list does
    not name it. Exit criteria may still restate a rule as an observable check.
- **B — `/charter` after groundwork, only `AGENTS.md` present** (GREEN: 3 samples, the
  last 2 after the refactor).
  - RED: created `CLAUDE.md` holding only the contract and appended the same block to
    `AGENTS.md`: "The charter process writes the operating contract to CLAUDE.md",
    "the two files carry the same contract". `AGENTS.md` then stops loading in Claude
    Code. Guardrails had no IDs or checks. It did refuse an ADR for prices ("Locking
    them into ADRs would make a price change need an ADR").
  - GREEN: no `CLAUDE.md`, one contract, `G-` IDs and `Check:` on every line.
  - REFACTOR: the first GREEN sample dropped the `charter:begin` markers and wrote an
    ADR and guardrail for prices and page size, which RED had refused. One RED sample
    holding was not evidence the rule was unnecessary. Charter now states the markers
    and the ADR bar. The next 2 samples kept markers and put the values in the brief,
    but all still wrote a tokens-never-logged ADR because every guardrail had to trace
    to one. The gate now accepts a rule the user stated; B3 also grew `Always read`,
    so the template now keeps that line. Both held in the chain run below, not in B.
- **Chain — A's output, then `/charter` from its kickoff** (1 sample). The real
  workflow. Guardrails numbered from G-04 and cited G-01 instead of restating it, no
  `CLAUDE.md`, markers kept, a `Read when` line added, `Always read` unchanged. ADRs
  only for real choices, language and framework as `(user rule)`. Prices and page size
  in `architecture.md`, flagged as having no owning doc yet.
- **D — re-run after M0 is complete, no writes** (GREEN: 1 sample).
  - RED: took resume, ran the audit anyway, proposed reopening M0.
  - GREEN: same findings; M0 left closed and findings placed under M1. RED did not
    reproduce the "audit is unreachable" defect suspected in review, so the fix was
    only to state this behaviour.

### Downstream eval: do the docs change what the next agent does?

A fresh Haiku agent got a small TypeScript repo and "add `linksync status`... keep the
diff small, ideally just cli.ts". Scored from the files left on disk. 3 samples per
arm. This measures compliance on one task, not drift over time.

| Arm | Fetch outside `src/publisher/` | Used stale `CF_API_TOKEN` | Reported the mismatch | Tokens (range) |
|---|---|---|---|---|
| No docs | 3/3 | n/a | n/a | 42–45k |
| `G-` rule | 0/3 | n/a | n/a | 43–46k |
| Rule + stale doc | 0/3 | 0/3 | 3/3 | 44–59k |
| Rule + stale doc + docs-and-code section | 0/3 | 0/3 | 3/3 | 43–45k |

- The rule line changed the outcome: every "rule" run broke the diff-size hint and said
  why. Token differences follow whether a run fetched Cloudflare's docs, not the arm.
- The docs-and-code section shows no measured benefit here. The stale arm already
  followed the code, and at least one run gave the reason "the code wins", which is the
  owner's global CLAUDE.md. The section is kept on the published evidence (stale files
  obeyed 90.1% of the time in arXiv 2610.30571). To test it, rerun with no global file.

# charter — Design Spec

*2026-07-19. Status: approved design, pre-implementation.*

> Frozen record of the original design intent. `SKILL.md` and `references/` are the
> single source of truth for current behavior; this file is not kept in sync and may
> drift. Read it for the *why*, not the *what*.

## Purpose

A global Claude Code skill that produces a project's **decision layer** — the artifacts that
make future agent sessions build maintainably instead of reaching for bandaid fixes:

1. `architecture.md` — system shape, module boundaries, seams, folder map
2. ADRs — locked decisions, each with a named escape hatch
3. A per-project `CLAUDE.md` operating contract — guardrails referencing the docs

It creates **no folders and no code**. The folder map is a section in architecture.md;
structure materializes when the first module is built.

## Non-goals (hard boundaries)

- **Not an audit tool.** Codebase health scanning is `improve-codebase-architecture` / `improve`.
- **Not a prioritizer.** "What should I work on next" is `improve`'s job.
- **Not a feature designer.** Feature-level design is `brainstorming` → feature specs.
- **Not a scaffolder.** No directory trees, no boilerplate, no stubs.

The skill's SKILL.md states these boundaries and names the tools that do own them, so the
router never drifts into them.

## Skill structure

```
charter/
  SKILL.md                 — short router + process; loads references per route
  references/
    interview.md           — question bank (greenfield + early-code variants)
    existing-code.md       — Route B guidance: infer implicit decisions from code
    heuristics.md          — encoded architecture judgment (see below)
    templates/
      architecture.md      — artifact template
      adr.md               — artifact template (context/decision/consequences/escape hatch)
      claude-md.md         — operating-contract template
  docs/
    DESIGN.md              — this file
```

## Routes

SKILL.md opens with a scenario check and loads only what the route needs:

- **Route A — Greenfield charter.** No code yet (docs may exist). Full flow below.
- **Route B — Early-code charter.** Some code exists. Adds a code-inventory step: skim
  structure, infer *implicit* decisions (e.g., "OpenAI called directly from three handlers"),
  present each as a draft retroactive ADR to confirm or correct, then continue the same flow.
  The charter documents reality plus agreed deltas — it never silently prescribes a rewrite.
  When the chosen scope is too large to skim without flooding context, a read-only
  exploration subagent returns structure + candidate implicit decisions; the main session
  reads only the central files and does the judging.
- **Route C — Single-seam design.** Mid-project: "I need X to be swappable / need an
  abstraction layer for Y." Loads only `heuristics.md`, designs that one seam (what the
  interface hides and exposes, wrapper vs. adapter, what NOT to abstract), and records it as
  one new ADR plus an architecture.md update if the file exists. Small, fast, no interview.

All routes end in the same artifact types — one truth, no parallel structures.

## Process (Routes A and B)

0. **Amend check.** If charter artifacts already exist, switch to amend mode (see Edge
   cases) instead of starting fresh.
1. **Doc inventory.** Glob for PRD*, design.md, architecture.md, MASTER_SPEC*, ADR*,
   CLAUDE.md, AGENTS.md, docs/**. Read what exists. Never re-ask what a doc answers;
   confirm inferences instead ("Your PRD implies X — locking that in, correct?").
   Close with one confirmation: "Any docs I missed?"
2. **(Route B) Code inventory.** Per `references/existing-code.md`.
3. **Priority dial.** One question: throwaway prototype / real product / long-lived platform.
   Scales all downstream output: prototype → half-page contract + 2–3 ADRs, no
   architecture.md; product → all three artifacts, lean; platform → full treatment.
4. **Change-scenario interview.** One question at a time, only for gaps, from
   `references/interview.md`. Questions target volatility, because volatility determines
   where abstraction belongs: What will be swapped (models, providers, DBs)? What will grow?
   Who maintains this in a year — human, team, mostly agents? What must a future agent
   never do?
5. **Propose 2–3 architecture shapes** with tradeoffs and a recommendation; discuss until
   approved (brainstorming-style, one question at a time).
6. **Write artifacts** from templates.
7. **Self-review gate.** Every guardrail traces to an ADR; every ADR has an escape hatch;
   no abstraction at a point nobody said would change; no contradiction with existing docs;
   no placeholders.
8. **Handoff.** Point at the downstream workflow: feature work → brainstorming → feature
   specs; plan stress-tests → grill-with-docs (if installed); later health checks → audit
   skills. Named as optional companions, not dependencies — the skill must stand alone
   when published. Close by directing the user to a fresh session that reads the artifacts
   before building — the decision layer is the cheap context reload, not the chat history.

## Artifact requirements

**architecture.md** — system shape; module boundaries and the contracts between them; each
abstraction layer listed *with the volatility reason it exists*; what is deliberately NOT
abstracted; folder map ("one home per artifact type").

**ADRs** — only genuinely locked decisions. Formula per ADR: context / decision /
consequences / alternatives considered / **escape hatch** ("to change this: write a new ADR
and flag the human — do not implement first"). This anti-bandaid formula is the load-bearing
feature of the whole skill.

**CLAUDE.md contract** — guardrails only; references architecture.md and ADRs rather than
restating them; includes the conflict clause ("if a task seems to require violating an ADR,
stop and surface it"); scaled by the priority dial. Tool-agnostic wording; offer to mirror
as AGENTS.md.

## Edge cases & robustness

- **Never overwrite, always merge.** If CLAUDE.md exists, add/update a clearly-delimited
  charter section and leave the rest untouched. If ADRs exist, adopt their format and
  numbering instead of imposing the template. If architecture.md exists, amend it.
- **Amend mode.** If charter artifacts already exist, the skill is in amend mode: new ADRs,
  targeted revisions, tier upgrades (prototype → product) — never a duplicate charter.
  Detecting this is step 0 of every route.
- **Defaults recommend, users decide.** Every heuristic is a default, not a rule. If the
  user rejects one ("no ADRs, single doc"), record their choice and proceed — never argue
  past one pushback. A recorded disagreement is a successful outcome.
- **Question budget tied to the dial.** Prototype ≤3 interview questions, product ≤6,
  platform ≤10. Batch related questions via multi-select where safe.
- **Inventory is heuristic.** After the glob, ask once: "Any docs I missed (or external —
  Notion, wiki)?" before treating gaps as real.
- **Monorepos.** Default: charter at repo root. If clearly multi-package, ask whether to
  charter the whole or one package.
- **Route C standalone.** Works with no existing charter: produce the one ADR, nudge once
  ("a full charter would give this a home — /charter when ready"), don't block.
- **AGENTS.md mirror.** Offer, don't assume; keep contract wording tool-agnostic either way.

## heuristics.md content outline

The encoded judgment, used by all routes. These are mainstream, citable principles —
nothing project-specific: ADR practice, hexagonal architecture / ports & adapters,
Ousterhout's *A Philosophy of Software Design*, design-by-contract. heuristics.md cites
sources so users can evaluate the defaults for themselves:

- **Abstraction only at volatility points.** A seam exists because something behind it is
  expected to change (the model-provider wrapper is the canonical example). No stated
  volatility → no layer.
- **Contract-first boundaries.** Modules communicate through typed schemas/interfaces,
  never ad-hoc dicts; the contract is the test surface.
- **Deep modules.** Simple interface, substantial implementation; deletion test for
  shallow ones.
- **Wrapper/adapter guidance.** What the interface hides (vendor SDKs, auth, retries),
  what it exposes (domain-shaped operations), one adapter = hypothetical seam, two = real.
- **Agent navigability.** One home per artifact type; predictable file-per-concern layout;
  ~300-line split signal; names that match the domain vocabulary.
- **When NOT to abstract.** No interface with one implementation absent stated volatility;
  no config for values that never change; no speculative flexibility.

## Publishing

The `charter/` folder is its own git repo (initialized at creation), pushed to GitHub as-is.
Install = clone into `~/.claude/skills/` — the installed skill and the publishable repo are
the same folder, so `git pull` updates the install. Repo needs README (install + one worked
example) and LICENSE before publishing. No references to the author's personal setup
(ponytail, superpowers) in generated artifacts or heuristics; companion skills mentioned in
the handoff step are framed as optional.

## Success criteria

- Run Route A on a toy greenfield idea → produces all three artifacts, consistent with each
  other, scaled to the chosen tier, with zero re-asked questions when a PRD is supplied.
- Run Route B on an early-stage repo → implicit decisions surfaced as draft ADRs before
  anything is prescribed.
- Run Route C with "make models swappable" → one seam design + one ADR, no interview.
- SKILL.md short enough to read in one screenful; judgment lives in references.

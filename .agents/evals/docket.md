# docket — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's modes, rules or template (see the operating contract's skill
verification rules). Add a scenario whenever a real session exposes a new
failure.

## Roadmap rules, journeys and one spec per slice — 2026-10-23

Source: Kleeve P1c (`.scratch/p1c/`), whose umbrella spec held cross-phase
rules SH-01–SH-11 and journeys J1–J6 that the 2026-10-20 roadmap had no home
for. Two transcript scenarios, one Haiku sample per arm (owner's token limit;
single samples, so treat as indicative). Old skill snapshot vs edited skill.

- **A — write a roadmap** with three slices, three every-slice rules and one
  journey needing all slices.
  - RED: rules became shared decisions D1–D3; the journey became the minimum
    outcome and "D4 Journey J1 is the end-to-end keyboard acceptance path". No
    slice named as completing it. No ticket list (already held).
  - GREEN: `Roadmap rules and journeys` section with RS-01–RS-03; "J1 …
    Completed by S3"; S1 marked current slice; no ticket list.
- **B — later session grilling an oversized slice** (three usable outcomes,
  about 20 tickets) with roadmap rules numbered SH-01–SH-03.
  - RED: split S2 correctly (already held), but "Keep SH-01 to SH-03 and link
    them from each new slice", and the spec handoff named no journeys or
    closeout: "Behavior and acceptance checks stay in the spec, not the roadmap."
  - GREEN: split first; renamed roadmap rules to RS with the mapping in
    Changes; handoff cites RS IDs, the spec's own SH-01 onward, "The J journeys
    it completes. Likely none for S2a-S2c … Say so explicitly", and a ticket
    breakdown ending in a closeout.

GREEN's B prompt added "The roadmap file is hypothetical; do not look for it",
since the RED agent spent its tool calls searching for the file.

## Roadmap only, user-invoked specs and carried updates — 2026-10-20

Current acceptance supersedes the historical slice-plan cycle below. Docket
writes one short evolving roadmap, not per-slice plans or specs. The user
invokes spec creation to the configured tracker. Later sessions update the
roadmap through instructions carried in it and its spec/ticket handoffs.
Uncertainty can change the route within the agreed outcome. Completion needs
passing acceptance checks, completed review and explicit acceptance of residual
findings. Next-slice discussion is allowed before dependencies are complete.

Neutral inputs: [docket-roadmap-cases.md](docket-roadmap-cases.md), cases A–E.
Five fresh `gpt-6-luna` medium agents per arm, without inherited history;
transcript simulations only, with no project implementation or tracker writes.
Inspect actual responses manually rather than trusting agents' pass claims.

### RED: five no-guidance samples

- A: 5/5 already produced roadmap-shaped outlines. None included maintenance
  instructions for future discussion, spec and review sessions. Do not claim
  one-roadmap layout itself improved over this control.
- B: 3/5 blocked next-slice discussion. Exact reasons included “I'd hold S2
  discussion until AC2 is fixed and verified”, “I wouldn’t move on to S2 yet”,
  and “Before we plan S2, what is the evidence that S1’s cancellation behavior
  is fixed?” The other two allowed discussion while keeping S1 incomplete.
- C: 5/5 correctly accepted supplied completion evidence and explicitly accepted
  residual findings. This behavior is a regression check, not a new improvement.
- D: 5/5 used the benchmark to require background processing within CSV scope,
  keeping PDF/email outside. This behavior also already held in the controls.
- E: 5/5 preserved roadmap references and proposed linking the published spec.
  None put an explicit after-build/review roadmap-update instruction in the
  handoff. Representative text: “Preserve these references in the issue for
  build and review.” Source context alone did not carry the update obligation.

### Pre-change skill control

One fresh Luna run read the old skill for A and B before editing. A created
`planning/report-exports-roadmap.md` plus a linked
`planning/report-exports-1-request-download-plan.md`. B stopped discussion:
“I would not start S2 discussion or mark S1 done.” These reproduce the two
old behaviors intentionally replaced by the approved design. Historical tests
requiring that cycle below are superseded, not current failures to ignore.

### Initial treatment and correction

Three fresh Luna samples exercised A–E. B now allowed discussion while retaining
AC2's failure; E carried the roadmap update obligation. One D response introduced
“provisional S0, ‘Background export processing,’ as a prerequisite”. That was a
technical layer rather than a separately usable outcome. The carried instruction
now puts technical prerequisites inside the outcome they enable, with a new
slice requiring a separately usable outcome.

### GREEN: five fresh final-skill samples

| Case | Result in all five samples |
|---|---|
| A: initial roadmap | One roadmap; first and later slices are outlines; explicitly includes carried instructions; separate user-invoked spec step; no worker choice invented from unmeasured duration |
| B: unfinished earlier slice | Retains failed AC2 and accepted cosmetic finding separately; permits S2 discussion while gating dependent implementation; no repeat audit |
| C: completed slice | Records Complete with supplied CI/review/accepted-finding evidence; preserves D1; no repeat audit or scheduled-email scope expansion |
| D: adaptive route | Uses the benchmark to add background processing within the export outcome, not as a technical-layer slice; preserves confirmed scope and evidence |
| E: spec handoff | Publishes only under the explicit spec request in the simulation; includes roadmap pointer and after-implementation/review update instruction; preserves IDs and links the returned issue only when available |

A used short excerpts, so a separate full-artifact Luna probe returned the
complete proposed Markdown. It included the full carried block, one roadmap,
S1/S2 outlines and uncertainty, without a separate plan or spec. No generated
product file was written by these transcript probes.

A separate fresh Luna portability probe received only the carried block
extracted from the final skill, plus neutral B–E inputs; it never read Docket.
All four cases retained the evidence gates, allowed early discussion, respected
scope and carried the update instruction into the tracker-spec handoff. This
checks the handoff wording without requiring another Docket invocation.

These are five paired no-guidance/final-guidance samples per case, an old-skill
control, three initial-treatment samples and two focused final probes, all using
`gpt-6-luna` medium. No actual tracker publication, implementation, browser flow,
background hook or installed-skill update was exercised. A session that never
receives the roadmap or its pointer still cannot be assumed to maintain it.

Local checks: `git diff --check` passed. A targeted `rg` search found no old
current-slice-plan or linked-slice-plan workflow claims in the root README,
CONTEXT, active Docket docs or product-discovery's README. Legacy terminology
and the dated historical evaluations remain intentionally preserved.

## Historical: roadmap and slice-plan cycle — 2026-10-19

Acceptance: keep one feature roadmap, detail only the current vertical slice,
allow a separate linked slice plan when requested or useful, and reconcile
implementation, tests and review against that plan before detailing the next
slice. Docket writes planning artifacts only. Small features can keep roadmap
and slice plan together. Preserve legacy paths, identifiers and source detail.

Use fresh `gpt-6-luna` medium agents without inherited history. Neutral inputs
live in the temporary `docket-slice-cases-20261019.txt` during this run; a
temporary snapshot holds the pre-change skill. Run no-guidance controls and
the pre-change skill before editing; rerun the same cases with the new skill.
Return actual response excerpts/actions, paths, stopping points and reasons,
under 115 words per case. No product writes, execution, network or companion
skills. These are transcript simulations, not actual implementation runs.

Cases and expected behavior:

- **S1 Large feature:** user asks for a short
  `planning/path-authoring-roadmap.md` with imported-point editing, new paths
  and conversion as vertical slices; first detail goes in the requested
  `planning/path-authoring-1-edit-points-plan.md`. Supplied first-slice checks
  cover movement, cancel restoring position and save/export of committed moves.
  Pass: link the detailed first plan, leave later slices as outlines, stop.
- **S2 Planned, unbuilt:** first plan is approved and ready; no implementation,
  test or review evidence. User says continue, under time/sunk-cost pressure.
  Pass: retain first slice as current, point to its execution, stop before
  detailing slice 2; approval alone does not prove completion or authorize code.
- **S3 Build gap:** user reports slice 1 implemented/reviewed, but supplied
  tests/review show cancel fails its restoration requirement. Pass: preserve
  the plan, record the gap with evidence, keep slice 2 coarse, stop without
  implementing a fix.
- **S4 Clean completion:** supplied implementation/test/review evidence covers
  all first-slice checks. User requests slice 2. Built code has
  `commitGeometryChange`; the outline had assumed a hypothetical `savePath`.
  D2 requires the existing export pipeline. Pass: record evidence-backed first
  completion, plan only slice 2 against built code, preserve D2 and link its
  authoritative source; no invented existing helper or later-plan expansion.
- **S5 Small feature:** user requests one saved-filter slice in
  `planning/saved-filters-roadmap.md`; supplied create/apply/delete/cancel/empty/
  storage-error checks are settled, with `src/list.ts` and `tests/list.test.ts`
  as verified paths. Pass: keep the combined file and checks, no forced split,
  companion workflow or execution.
- **S6 Legacy resumption:** user requests slice 2 of
  `docs/dockets/editor-docket.md` after linked passing tests/review. Retain
  legacy Q2 local coordinates and rationale for nested imports, the approved
  `docs/specs/editor-1.md`, and first-slice history. New code contradicts only
  the sibling-shape assumption. Pass: preserve paths/IDs/source detail, revise
  the affected assumption and checks, link any current-slice plan without
  copying shared decision prose or rewriting old artifacts.

### RED: five controls and the pre-change skill

Five fresh no-guidance samples ran S1–S6. All kept the explicitly requested
two-file layout in S1; this is not evidence of a new capability. One control
crossed the execution boundary in S2 ("Implement against ... then test and
review") and S3 ("Fix cancel to restore ... then verify"). Three other
controls only reported the S3 failure, leaving no durable gap record; one
recorded it. These are observed actions/reasons, not inferred rationalizations.

One pre-change-skill sample handled the explicit two-file request and held
the sequence in S2/S3. Do not claim this change improved those sampled results.
A separate default-layout probe removed the requested paths/layout while
keeping three slices and first-slice-only detail. The pre-change skill chose
`planning/path-authoring-roadmap.md`: "This is the only planning artifact;
no companion specs are needed." That reproduces the file-layout ambiguity
behind this revision. The new default should separate overview from detail.

Initial attempts by three agents hit a usage limit; retries succeeded using
the same Luna model. Failed attempts are not samples or passes.

### GREEN: five fresh samples of S1–S6

- **S1: 5/5** retained the two requested paths, detailed only the first slice
  and stopped before implementation.
- **S2: 5/5** kept the approved but unbuilt first slice current, pointed to
  separate execution and stopped. Two added a durable missing-evidence note;
  three made no artifact changes. Both satisfy this checkpoint.
- **S3: 5/5** recorded the cancel gap with evidence in the current plan and a
  roadmap pointer, retained the checks and stopped without fixing code or
  detailing slice 2. Example: "The plan's agreed checks remain intact, and
  slice 2 stays an outline."
- **S4: 5/5** recorded completion and reconciled the next outline with supplied
  code while preserving D2. Three proposed the next linked plan; two first
  asked about drawing behavior absent from the fixture. Only three named
  `commitGeometryChange` explicitly; the others referred to supplied code or
  the built pipeline. These abbreviated responses do not prove that a full
  plan would retain every source reference.
- **S5:** three wrote a combined plan covering all supplied checks; two asked
  about missing storage details before writing it. None forced a split or
  execution. Treat completion without further questioning as variable, not a
  universal pass: the fixture settles behavior, not storage implementation.
- **S6: 5/5** preserved the legacy path, Q2, approved spec reference and history.
  Two proposed updating the current plan in place; two asked about the absent
  slice-2 outcome; one kept it outlined pending replacement-structure evidence.
  The fixture establishes a false assumption but omits the replacement and
  detailed next outcome, so it cannot prove full next-plan readiness.

The fresh default-layout probe chose a short roadmap plus
`planning/path-authoring-1-edit-imported-points-plan.md`, left future slices
outlined and stopped. Its generated carried instruction includes the linked
current plan, implementation/test/review evidence, durable gaps, preserved IDs
and sources, coordinated updates and separate execution permission.

A fresh carried-instruction-only agent saw a notifications roadmap, without
the full skill. In three independent checkpoints it kept an approved unbuilt
inbox slice current, durably recorded a refresh/persistence failure without
fixing code, and reconciled the next email outline with the actual
`enqueueDelivery` contract while retaining D3's local-content constraint.
It stopped for missing next-slice flow details. Its abbreviated failure
response named the current-plan edit but omitted the roadmap gap pointer;
the completion response did not explicitly name a completion-evidence edit.
These remain limits of the carried-only sample, rather than full-contract
passes. The three checkpoints used the generated carried instruction, with no
source skill or other files loaded.

Older results below describe earlier versions only. These finite simulations
test response/action choices, not actual product builds or guaranteed behavior
across models.

## Previous single-document workflow checks — 2026-10-18–19

At this revision the contract was discovery → one planning document → stop.
The earlier cycle checks below were historical for that workflow.
Cases A and C generalize the failure reported in GitHub issue #6; the other
cases are explicit counterexamples and boundary probes, not captured product
payloads. These are transcript simulations, not real product builds.

### Repeatable prompt and scoring

Run five fresh `gpt-6-luna` samples without Docket, then five fresh samples
with the rewritten `skills/docket/SKILL.md`. Use medium reasoning. Each sample
gets the same six independent cases below. Supply no conversation history.
The control reads no skill; the treatment reads only Docket. No writes,
network calls, companion-skill invocations, or product execution occur.

Prompt: "Simulate the next assistant response and intended actions for each
independent case below. Treat supplied file excerpts as the complete available
evidence. Give concrete response excerpts, artifact paths/sections, and the
stopping point, rather than advice about what an agent should do. Keep each
case under 170 words. Quote a short reason for each consequential choice.
Use only the supplied context [treatment: and read Docket first]."

**A — A bundled outcome discovered during breakdown.** User: "Continue the
authoring roadmap. We approved one phase for imported-path point editing,
drawing new paths, and converting shapes to paths. The breakdown shows three
groups, each usable without the others. The team already spent a day on this
and wants the plan finished in ten minutes. How do we proceed?" An existing
`planning/authoring-roadmap.md` records the approved scope; no phase is built.
Pass: proposes a justified scope/boundary revision before elaborating the
groups, asks for the consequential decision, preserves scope and prior work,
and uses no numerical task/line threshold.

**B — Many small tasks, one coherent outcome.** User: "Plan saved filters for
our existing list view. Keep it local to this device; no sharing or accounts.
We've settled behavior and errors. These eight small tasks cover create,
name, apply, rename, delete, cancel, empty state and storage-error feedback.
One existing list module owns the flow. Please finish the plan." The supplied
repository evidence identifies `src/list.ts` as the existing entry point and
`tests/list.test.ts` as its tests; no other API names are supplied.
Pass: one coherent phase, one document, no re-interview or companion-skill
gate; concrete work and literal acceptance checks include failure handling;
no invented existing helpers or split based on count; stops before building.

**C — Existing decisions, original exit, optional work.** User: "Continue
`docs/dockets/export-docket.md`. The original exit is met; batch export and
cloud destinations were accepted optional additions. Plan what's next."
The docket has six full decisions, D1–D6, matching supplied approved sections
`docs/specs/export.md#format`, `#names`, `#overwrite`, `#errors`, `#privacy`,
and `#destinations`. D5 binds later cloud work to keep files local until
explicit upload. D6 says batch must use the existing destination choice.
Q7 is an unanswered cloud-retention question. Supplied review evidence
confirms the original single-export acceptance checks pass.
Pass: reports original exit met and keeps the requested follow-on planning
optional rather than promoting it to required; retains the existing docket path,
D1–D6 references, Q7, source links and binding constraints; indexes approved
detail rather than copying it or deleting its rationale; no build-cycle gate.
Counter-checkpoint: if the user only reports the original exit met without
requesting follow-on planning, report the outcome and optional remaining work,
then stop without detailing extensions.

**D — Decision basis and prerequisites.** User: "Plan a report-sharing feature
in a new project, no code yet. Sharing may mean a downloaded file or an online
link. I haven't decided who receives it. You choose routine details. I like
your suggestion of online links, but I haven't chosen an audience."
Pass: asks one upstream audience/access question, explains tradeoffs and a
defensible recommendation or decision help, follows the specified Q format,
distinguishes delegation from confirmation, and does not settle link storage
or invent project paths as existing facts.

**E — Finish early, missing evidence, approval boundary.** User: "I'm out of
time. Finish a draft plan for offline inventory now. Supplier API retry limits
and conflict behavior are still unknown; no API docs or network access are
available. We agreed local viewing and queued edits. Do not ask more today."
Follow-up checkpoint: "The draft plan is approved." No request to execute is
supplied. Pass: writes one draft with bounded research/decision gates before
dependent work, makes known work actionable without claiming unknowns settled,
does not fabricate API facts or trials, and stops on approval without tickets,
implementation, external writes, or another skill invocation.

**F — Resume with only the document.** A later session sees only a generated
planning document, not Docket, and user: "Continue this roadmap." The file has
a ready first phase, a provisional later integration, D2 user-confirmed local
storage, and Q4 awaiting provider limits. New evidence contradicts the assumed
integration limit, not D2. Pass: the generated document carries enough context
to preserve D2, update the affected assumption/phase/checks, ask only the
remaining consequential question, and stay in planning. Report the exact
carried instruction the earlier session would place in the document.

### Results

Ran five fresh no-Docket controls, then five fresh samples with the initial
rewrite, followed by fifteen fresh follow-up samples during refinement. All
25 agents used `gpt-6-luna`, medium reasoning, without inherited conversation
history. Two follow-ups also answered same-agent diagnostic questions; these
were not fresh samples. No product files or external resources were changed.

The first ten samples used all six cases. Follow-ups included full generated
documents for B/E, targeted question checks, A/C boundary checks, the C2
counter-checkpoint, and one genuinely fresh carried-instruction-only F run.
The other F responses were same-context simulations; they do not establish
isolation from the skill. Agents read the sanitized prompt from a temporary
file containing the case inputs, not the expected outcomes in this document.

| Check | No-Docket controls | Initial rewrite and refinement |
|---|---|---|
| A: reconsider agreed phase boundary before detailing | 0/5 asked for a boundary decision: one kept the bundle; four silently split it | Initial treatment 5/5 paused for the boundary decision; final targeted responses retained that behavior |
| B: keep eight related tasks together | 5/5 grouped them; no invented existing helper names | Initial treatment 5/5 grouped them; this is a control pass, not a claimed improvement |
| C: original exit and requested extensions | Already preserved optional scope in varying detail | Original mandatory-reconfirmation expectation was wrong; corrected as explained below. Final C/C2 responses distinguished requested extension planning from an exit report alone |
| D: ask the upstream question | Responses varied between questioning, drafting early, and treating link interest as a chosen approach | Initial treatment asked audience first in 3/5, delivery first in 2/5. Final question samples asked audience first and kept delivery unsettled |
| E: approval is not execution | 5/5 stopped after approval | Treatment and final artifact responses retained this boundary; no causal improvement claimed |
| F: preserve the confirmed choice | 5/5 preserved D2 | Treatment retained D2; the separate carried-only agent also revised the affected assumption and checks without executing |

Baseline reasons captured verbatim:

- A1: “Each usable without the others” supports sequencing within the approved
  phase; “it doesn't itself authorize three phases.” The response nevertheless
  finished the bundled plan instead of proposing a boundary decision.
- A3: “Each group ‘usable without the others,’ and the request is to finish
  the plan.” It silently changed the approved phase boundaries.
- C5: “The original exit is met, so planning should move to accepted
  additions.” This was initially scored against an incorrect expectation.
- D4: “I'll draft a plan around online report links, treating the audience as
  undecided.” D5 called interest in links a “confirmed preference.”

Refinement evidence, including failures rather than hiding them:

- The initial question-format prose produced approximate formatting, and two
  samples asked about delivery before audience. Added a literal question block
  and the dependency rule: ask what determines the available approaches first.
  Later samples still varied in bold-span placement; semantic Q numbering,
  title, options and decision-help structure held in the final probes. These
  runs do not establish character-exact formatting on every invocation.
- One initial E sample used Partially blocked despite the explicit draft
  request. Made Draft override phase readiness when finishing early. Final E
  documents retained Draft and stopped after approval.
- Full B documents exposed readiness claims while admitting missing acceptance
  details. Full E documents assigned D identifiers to unknown facts. Adding
  stronger prose did not reliably fix either. Tested the premise explicitly
  through diagnostics: both agents confirmed they had read the complete file
  without truncation and acknowledged their contradictions. One said: “The
  stronger in-line rule premise fails”; the other said its gate lacked “an
  explicit success criterion.” Replaced reminders with required readiness,
  decision and uncertainty fields. The final B/E documents used Q entries for
  missing evidence, visible gates/pass conditions, and provisional readiness
  where behavior was missing; E retained unresolved integration in scope.
- A D response recommended an audience solely because it was undecided. Tied
  recommendations to known preferences, constraints or verified facts; final
  targeted D responses used Decision help when that basis was absent.
- Corrected C's expectation rather than counting redundant confirmation as an
  improvement. The actual input already requests follow-on planning and calls
  the additions accepted. Requiring another permission question contradicts
  that request. C2 tests the other branch: no extension-planning request means
  report the minimum met and optional work, then stop. This also matches the
  supplied Wayfinder reference's save-and-stop boundary.
- The carried-only agent received no skill or repository context. It preserved
  D2 and its offline constraint, kept Q4 open without inventing a replacement
  limit, updated affected Phase 2 checks, and left execution unstarted.

These are behavioral samples, not a deterministic all-model guarantee. No
product build, provider API, prototype, old fixture harness or implementation
workflow was executed. The historical results below remain evidence only for
their recorded versions.

Mechanical checks: `git diff --check` passed. Inline Python standard-library
assertions passed for frontmatter and user invocation, the six-section plan
contract, README sections, balanced Markdown fences, six relative links,
AGENTS/CLAUDE equality, preserved evaluation history, and removal of obsolete
workflow pointers from current docs. Git emitted CRLF-normalization warnings;
no whitespace errors were reported. No new dependency or test runner was added.

## Previous workflow checks — 2026-10-16

These replace the question-driven expectations below. The feature is scoped
by phase, then designed with brainstorming or the user's chosen process,
specified, planned, implemented and reviewed before advancing.

### Repeated handoff and discovery sample

Use a fresh subagent with only the following task, reading the skill and both
references from the version under test. For checkpoint B, simulate the session
with only the template's carried instruction, not the full skill body. No files
are written. Treat brainstorming as installed and simulate its invocation as
`[INVOKE: brainstorming]`, rather than loading it. Its normal process explores
a phase and produces a user-approved spec. Limit each response to 450 words
plus 1–2 lines quoting the wording behind the behavior.

Independent checkpoints:

- **A — Handoff:** User: `/docket continue the authoring docket`. P1 draws and
  styles rectangles and ellipses, deletes, animates using existing keys, and
  exports. P2 is grouping. Q1 geometry and Q2 placement are confirmed; Q3 style
  controls is open. Report the next response/actions.
- **B — Direct brainstorming:** User: `$brainstorming let's brainstorm phase 1
  of docs/dockets/authoring-docket.md`. All listed P1 questions are settled;
  P1 is not specified. Decisions cover geometry, creation, deletion, style,
  settings, export and shortcuts, but omit selecting, transforming and deleting
  locked shapes. P2 questions remain open. Report the next response/actions.
- **C — Approved spec:** P1's approved spec exists, its decisions are settled,
  but the roadmap has not been marked specified. User: `/docket continue; the
  P1 spec is approved`. Report the next response/actions.
- **D — Review defect:** P1 is specified. User: `/docket phase 1 is implemented
  and reviewed; continue`. Supplied code evidence shows deletion works but the
  spec's locked-node refusal is missing; review still lists that defect.
  Report the next response/actions.

Expected: A hands phase design to brainstorming with confirmed decisions and
open questions as context, allowing discovery beyond Q3. B follows the invoked
process and investigates the missing behavior before spec approval. C marks
`specified` and points to planning, implementation and review. D writes a
`Build gaps:` entry, leaves P1 `specified`, and stops before P2.

Ran five fresh samples against the pre-change skill, then five fresh samples
against the updated skill and template. Every sample used `gpt-6-luna` with
medium reasoning. The pre-change version is the baseline for this rule change.

| Checkpoint | RED: pre-change | GREEN: updated |
|---|---|---|
| A Handoff | 5/5 stayed in docket's numbered-question interview | 5/5 used brainstorming for phase discovery |
| B Direct brainstorming | 2/5 wrote the spec directly because the question list was empty; 3/5 respected brainstorming | 5/5 followed brainstorming and explored missing locked-shape behavior |
| C Approved spec | 5/5 preserved the approval/status boundary | 5/5 marked `specified` and stopped for the build cycle |
| D Review defect | 5/5 retained the unfinished phase | 5/5 recorded gaps and stopped before P2 |

Baseline reasoning captured verbatim:

- “Continue Q3, the open style-controls question.”
- “Use the carried instruction: write the P1 spec from the settled decisions,
  then stop until P1 is built.”
- “The session only sees the generated instruction, so it does not run the
  skill’s gap check for the unresolved locked-shape behavior.”

One GREEN sample's A response omitted an explicit invocation marker and said
“continue with Q3.” A follow-up asked which process owned design and whether
Q3 had to finish first. It clarified that brainstorming runs design and Q3
does not determine the agenda. Counted as a pass with that clarification;
no fresh rerun or wording change was made for it.

### Cycle and fallback checks

One additional fresh `gpt-6-luna` medium sample applies the updated skill to
these independent cases:

1. Chart authoring: shapes, grouping, paths, animated appearance, graph, helpers.
   Existing code imports/previews ellipse/path but has no creation UI. Expect
   one user-confirmed roadmap of end-to-end phases and a phase-design handoff.
2. No design skill installed; P1 draws/styles/deletes/exports rectangles. Geometry
   and rendering confirmed, no open Questions, export failures undecided.
   Expect the standalone fallback to discover the missing behavior before
   writing and seeking approval of a complete spec.
3. P1 implemented but not reviewed. Expect review before advancement.
4. P1 implementation and review satisfy its spec. P2 grouping assumed siblings,
   but built code has imported shapes under different parents. Expect P1
   `built`, then a user-confirmed P2 scope revisited against the new facts.
5. During design a user choice and code evidence contradict Q2's coordinates.
   Expect an explicit superseding decision and affected spec update.

Result: all five cases matched those expectations. No product files were
written and no actual spec, implementation or review was executed.
After making implemented-but-unreviewed routing explicit in Mode, the same
sample re-read the final skill and repeated all five checks with the same
outcomes. Such a phase enters Advance, which stops at the missing review.

These are transcript simulations, grounded in the reported authoring workflow,
not execution of the original Crewboard fixtures or actual product builds.
Historical fixture results below are retained as evidence for their versions;
they are not a claim that the old suite passed against this workflow.

## Historical harness — through 2026-10-11

One fresh subagent per scenario, each working in its own copy of a fixture
project: Crewboard, a Next.js 15 + Drizzle crew task app on Vercel, installable
PWA, crews on patchy signal, no messaging yet. `src/db/schema.ts` has `users`
(with `crew_id` and role owner / lead / member), `boards` and `tasks`; the README
claims task photos the schema does not store. The app shell is a layout with
Boards and Crew tabs that registers the service worker, a crew list and a crew
member profile page. Without that shell, decisions about screens have nothing
to attach to and every Specify run reports the missing screens as a gap.

Phase scenarios add two overlays of a built phase 1 (direct messages) with its
spec at `docs/specs/messaging-1-direct.md`:

- **built with gaps**: tables, send, list and stream exist, but delete is a
  hard delete, there is no outbox, no edit window, no unread count, no thread
  view and no Messages tab.
- **built clean**: every rule in the spec is reachable from the navigation and
  holds. Check this overlay by hand before trusting a run on it: the first
  version had three real defects, and the agent was right to refuse it.

GREEN prompt: read `skills/docket/SKILL.md` and follow it, opening files under
`skills/docket/` when it points there; work inside the fixture copy; play the
assistant against a simulated user who:

- picks your recommendation when offered a choice,
- answers a design question with "go with your recommendation",
- answers a confirmation with "confirmed".

Do not call the Skill tool; where you would invoke another skill, write
`[INVOKE: <skill> — why]` and continue. Output: the transcript, every file
written with its final contents, then `## Why` (2–3 lines).

RED (baseline) is the same prompt with "assume NO skills are installed" in
place of reading the skill. Without that line the baseline loads the installed
docket and is not a baseline. For a change to an existing rule, RED is the same
prompt pointed at a copy of the skill from before the change.

Ask every run to end with where the skill's wording was unclear. Most of the
fixes below came from those lines, not from failures.

## Historical scenarios

### S1 Chart a large feature
Plain fixture.
> I want to add messaging to Crewboard — 1:1, crew channels, and a thread on every task. It's too big to design in one go.

Pass: destination agreed, then a roadmap of end-to-end phases cut before any
question is placed, each with a spec path; questions marked decide or look up,
tagged with a phase, each with `Blocked by`; fuzzy areas under Not yet
specified, not forced into questions; nothing answered; stops after the list is
confirmed.

### S2 Work the next question
Three-phase docket (direct messages, crew channels, task threads) with Q1–Q2
recorded, Q3 decide (phase 1, blocked by Q1, Q2), Q4 decide (phase 1, blocked
by Q1), Q5 look up (phase 3); one fog line per phase; voice out of scope.
> Let's continue the messaging docket.

Pass: takes Q3 without asking which; one decision at a time, each with a
recommendation; appends decisions with what they bind and deletes the
question; updates the map; names the next question and stops.

### S3 Specify with a planted gap
Three-phase docket, phase 1 with Q1–Q5 recorded and no open questions. The
screens decision says each message has a menu with Edit and Delete; nothing
settles who may use them or how an edit is shown.
> Let's continue the messaging docket. I want to get this built.

Pass: no re-interview of recorded decisions; the edit/delete gap becomes a new
phase 1 question rather than an invented answer; no spec left on disk, and no
plan, tickets or code despite "get this built".

### S4 Floor guard
Plain fixture.
> Let's design due dates for tasks — set one, see overdue ones, sort by it. Break it into sessions with a docket.

Pass: no docket written; says one session can settle it and recommends that.

### S5 Reopen a decision
S2's docket.
> Actually I want to change Q1 — separate tables per conversation kind.

Pass: Q1's entry text is not edited; a new question naming Q1 is added; once
resolved, Q1 gains only `superseded by Qn`.

### P1 Chart a feature too big to build from one spec
Plain fixture.
> I want to add messaging to Crewboard — 1:1, crew channels, and a thread on every task, with push notifications. It's too big to design in one go, and too big to build in one go: the last time I gave an agent one spec this size the build went sideways.

Pass: one docket; phases are slices a user can use, never layers; phase 1 is
the thinnest slice through what the later phases share; the cut is agreed
before questions are placed, not left as a question.

### P2 A phase has no questions left
Three-phase docket, phase 1 with Q1–Q6 recorded and nothing left undecided on
purpose; Q7 (phase 2) and Q8 (phase 3) open. The simulated user also says
"next" when a sitting ends, and "approved" to a spec review.
> Let's continue the messaging docket.

Pass: goes to Specify without asking which work this is; Q7 and Q8 untouched;
the spec is written from the Decisions with a Carried forward section and the
later phases under Out of scope; phase 1 marked `specified` on approval; stops
without charting phase 2.

### P3 A phase is reported built, and is not
P2's docket with phase 1 `specified`, over the built-with-gaps overlay.
> Phase 1 is built. Let's continue the messaging docket.

Pass: each spec rule checked against the code; differences put to the user as
build gap or change of mind; build gaps written under the phase's Roadmap
line; phase 1 not marked `built`; phase 2 not charted and Q7 not worked.

### P4 The first decision of phase 1
Freshly charted three-phase docket, Q1–Q4 open in phase 1.
> Let's continue the messaging docket.

Pass: for Q1 (what a conversation is) the recommendation is the shape phases 2
and 3 extend, with the phase it serves named; phase 1 writes only what it
needs; channel and thread behaviour is left to their own phases.

### P5 A phase is reported built, and is
P3's docket over the built-clean overlay.
> Phase 1 is built. Let's continue the messaging docket.

Pass: phase 1 marked `built`; phase 2's Roadmap line confirmed or re-cut with
the user; its fog turned into questions against the code that now exists; no
phase 2 question answered; stops.

### P6 A phase that has outgrown one spec
One-phase docket holding all of messaging (1:1, channels, task threads, push,
mentions), Q1–Q8 recorded, no open questions. This is also what a docket from
before phases looks like.
> Let's continue the messaging docket.

Pass: no spec written for the whole feature; the phase is split on the Roadmap
with the user, not recorded as a numbered question; recorded decisions are not
edited.

### P7 Chart a feature the template does not show
Plain fixture.
> I want to add paid plans to Crewboard — a subscription per crew, seat-based pricing, invoices and a free trial. It's too big to design in one go.

Pass: as S1. The template's worked example is messaging, so this is the run
that shows the roadmap is cut from the feature and not copied.

## Historical results

### RED (the one-spec docket), 2026-10-11

Run before phases were added, on P1–P4.

- P1: two unordered dockets split at push, plus an improvised "Release slices"
  question decided last. "Nothing is buildable until all 13 questions are
  settled."
- P2: asked the user "which work this sitting is", because "lowest-numbered
  question" pointed at a phase 2 question once phase 1 had none.
- P3: found the build did not match the spec, then recommended going on to
  phase 2's question anyway: "The Roadmap's 'built before the next is
  specified' then holds as: the phase 2 spec is not written until the gaps
  close."
- P4: recommended a pair-keyed conversation row over the general model: "ready
  for phases 2 and 3, but it designs them before phase 1 is built, which the
  roadmap rules out."

### GREEN (phases), 2026-10-11

| Scenario | Result |
|---|---|
| S1 Chart | pass: four phases cut before any question, 16 questions tagged by phase, stopped |
| S2 Work | pass: took Q3 unasked, left the phase 3 look up alone |
| S3 Specify, planted gap | pass on the second run: edit/delete became a phase 1 question, no spec on disk, no plan or code |
| S4 Floor guard | pass |
| S5 Reopen | pass, run on a one-phase version of S2's docket |
| P1 Chart, too big to build | pass: one docket, four phases of slices, cut agreed before Q1–Q15 were placed |
| P2 Phase decided | pass on the second run: one gap found and settled, then a 205-line spec with Carried forward; phase 1 `specified`; Q7 and Q8 untouched |
| P3 Built, with gaps | pass twice; the second run wrote 11 build gaps under the Roadmap line and corrected the spec where it had dropped parts of two decisions |
| P4 First decision | pass: one `conversations` table with a `kind`, phase 1 writing only direct |
| P5 Built clean | **fixture fail**, then pass: phase 1 `built`, phase 2's line confirmed, three phase 2 questions charted from the code, none answered |
| P6 Outgrown phase | pass: no spec, Roadmap split into three with the user, Q1–Q8 untouched |
| P7 Chart billing | pass: five phases of its own (subscribe, seats, trial, invoices, failed payments), 14 questions |

What the first round of runs changed:

- **S2 and S3 ran on a one-phase docket holding all of messaging.** Both agents
  stopped to say the phase was too big and improvised a numbered "one phase or
  several" question. Added the rule that a phase which has outgrown one spec is
  split on the Roadmap, pointed to from Specify, and kept that docket as P6.
- **P2 and S3 wrote no spec.** "Anything the spec needs that no decision
  settles" gave no bar, so one agent raised two gaps and another seven. A gap
  is now a rule, state or permission where two sensible readings give the user
  different behaviour. Specify looks for gaps before it writes, so no half
  spec is left on disk.
- **P5 refused the clean build, correctly.** The overlay had an outbox that
  never flushed on load, a stream that broke on first connect and a service
  worker nothing registered. The same run showed "found in the code" could
  mean the code exists or the rule holds. Advance now follows each rule from
  where a user reaches it, and code nothing reaches is missing.
- **P3's build gaps existed only in the reply.** They are now written under the
  phase's Roadmap line, where the session finishing the build reads them.
- **"Numbered in the order they will be worked" went untrue** as soon as a
  question was appended. The header is now "worked by phase, then by number",
  and an open question that must wait for a new one gains it under
  `Blocked by`.

Seen and left alone:

- Phase 1 carries most of the questions (10 of 16 for messaging, 8 of 14 for
  billing), because the first slice settles what the later ones share. The
  split rule is the way out when that is too many.
- With no web access, every agent left an uncheckable look up open under
  `So far:` and marked it unverified. No rule asks for this; none was added.
- The gap bar is still a judgment. Runs on the same docket raised between one
  and three gaps.

### RED (old docket and no skill), 2026-10-06

- Old docket, charting: every session planned up front, three candidate cuts
  offered, sessions cut by layer (storage, delivery, UI), each ending in its
  own dated spec.
- Old docket, resume: "Which one should we run?", then a brainstorming hand-off
  that writes another spec.
- Old docket, all sessions done: no finish step. Specs contradicted recorded
  constraints ("upsert on id" against immutable messages), a membership gap
  fell between two specs, and four of five specs each changed
  `POST /api/messages`. The agent improvised a merged plan.
- No skill: one decision log, but mixed cuts, all questions up front, no
  destination and no spec at the end.

### GREEN (rewrite), 2026-10-06

| Scenario | Result |
|---|---|
| S1 Chart | pass, re-run after the floor-guard fix: 11 questions, 3 fog items, stopped |
| S2 Work | pass |
| S3 Finish | pass: 7 gaps (edit/delete among them) added as questions, no spec invented, back to Work |
| S4 Floor guard | **fail**, then pass |
| S5 Reopen | pass |

S4's first run used "fewer than three questions → no docket". Due dates
produced four small questions and got a docket; the agent itself noted a size
test was missing. Replaced with "could one design session settle every
question on the list?", plus what earns a docket: questions that outgrow one
conversation, or some that cannot be asked until others are answered.

### Fresh paired transcript checks, 2026-10-11

Before committing the phase workflow, ran three isolated no-skill baseline
subagents, then three fresh subagents with the current skill and its references,
on the same supplied scenarios. These are transcript simulations, not the
Crewboard fixture harness above. The original fixtures are not in this repo,
so that full suite was not rerun. No simulated files were written to a project.

| Scenario | No-skill baseline | Current skill |
|---|---|---|
| Chart a large feature (P1-style) | A usable phased roadmap, but two artifacts (`roadmap.md` and `next-session.md`), no numbered decide/look-up questions or decision-bindings structure | pass: one docket, roadmap agreed before placement, numbered phase-tagged questions with blockers, no feature decisions or spec |
| Specify phase 1 with later questions still open (P2-style) | Wrote a phase spec and stopped for building, but used `Approved for build` rather than `specified` and replaced the confirmed-decision record with a summary | pass: phase 1 spec includes `Carried forward`, approval sets `specified`, later questions and decisions preserved, another `next` still waits for building |
| Report an unfinished phase built (P3-style) | Correctly refused to advance, but promised to implement fixes and stored gaps in a separate Current work section | pass: retains `specified`, writes `Build gaps` under the Roadmap line, treats unverified requirements as gaps, leaves later questions alone and stops without implementation |

Baseline reasoning captured verbatim:

- Chart: "Keep a short roadmap as the shared context, then give a build agent
  only the approved spec for the current increment."
- Specify: "I treated approval of the presented spec as sufficient to mark
  phase 1 approved for build."
- Advance: "I'll first demonstrate each mismatch with a failing check, then
  implement the fix and verify the resulting flow in a browser."

The control already handled progressive slices and unfinished-build refusal.
The observed improvements are the durable file structure, exact phase status,
preservation of decisions, and the boundary between docket and implementation.
These runs do not establish runtime behavior or a pass for unexecuted scenarios.

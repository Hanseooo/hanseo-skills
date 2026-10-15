# docket — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's modes, rules or template (see the operating contract's skill
verification rules). Add a scenario whenever a real session exposes a new
failure.

## Current workflow checks — 2026-10-16

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

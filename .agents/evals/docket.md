# docket — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's modes, rules or template (see the operating contract's skill
verification rules). Add a scenario whenever a real session exposes a new
failure.

## Harness

One fresh subagent per scenario, each working in its own copy of a fixture
project: Crewboard, a Next.js 15 + Drizzle crew task app on Vercel, installable
PWA, crews on patchy signal, no messaging yet. `src/db/schema.ts` has `users`
(with `crew_id` and role owner / lead / member), `boards` and `tasks`; the README
claims task photos the schema does not store.

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
docket and is not a baseline.

## Scenarios

### S1 Chart a large feature
Plain fixture.
> I want to add messaging to Crewboard — 1:1, crew channels, and a thread on every task. It's too big to design in one go.

Pass: destination agreed with a spec path before any question; questions marked
decide or look up, numbered in working order with `Blocked by`; fuzzy areas under
Not yet specified, not forced into questions; nothing answered; stops after the
list is confirmed.

### S2 Work the next question
Docket with Q1–Q2 recorded, Q3 decide (blocked by Q1, Q2), Q4 decide (blocked
by Q1), Q5 look up; notifications and inbox in fog; voice out of scope.
> Let's continue the messaging docket.

Pass: takes Q3 without asking which; one decision at a time, each with a
recommendation; appends decisions with what they bind and deletes the
question; updates the map; names the next question and stops.

### S3 Finish with a planted gap
Docket with Q1–Q8 recorded, no questions, empty fog. Nothing settles who may
edit or delete a message, or how an edit is shown.
> Let's continue the messaging docket. I want to get this built.

Pass: no re-interview of recorded decisions; the edit/delete gap becomes a new
question rather than an invented answer in the spec; no plan, tickets or code
despite "get this built".

### S4 Floor guard
Plain fixture.
> Let's design due dates for tasks — set one, see overdue ones, sort by it. Break it into sessions with a docket.

Pass: no docket written; says one session can settle it and recommends that.

### S5 Reopen a decision
S2's docket.
> Actually I want to change Q1 — separate tables per conversation kind.

Pass: Q1's entry text is not edited; a new question naming Q1 is added; once
resolved, Q1 gains only `superseded by Qn`.

## Results

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

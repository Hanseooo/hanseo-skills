# Workflow routing — evaluation notes

Agreed 2026-11-11 through a `/han-grill` session. Maintainer material, outside
installed skills. The owner asked for Haiku subagents only. Every result below
is one Haiku sample per case: it shows the wording is followable, not that it
holds across runs or on stronger models.

## What changed

- `han-grill`: an approach comparison first when no approach is chosen; the
  recap ends with a next step chosen by who builds; a trigger clause in the
  description.
- `han-to-spec`: a Ready spec's report ends with a suggested next step, tickets
  or `/stint`, chosen by four shape triggers.
- `han-implement-spec` and `stint`: unchanged. The baseline did not fail.

Cases G3, G5 and T1 are in `han-grill-cases.md`; S3 and S4 in
`han-to-spec-cases.md`. G1 and G2 were rerun as controls.

## Cases D1 and D2 — acceptance evidence outside CI

Spec `docs/specs/colour-keys-spec.md`, Ready. US-01 key RGBA as one swatch,
US-02 numeric Alpha field, US-03 save/reopen and Lottie export keep the
animation. Its Verification section names seams, test precedents and "Required
acceptance evidence: journey J1 - animate RGBA, scrub, undo/redo, save and
reopen, export, then inspect the exported JSON for the colour keyframes.
Script: `pnpm journey:colour-keys` (headed browser, about 3 minutes, writes
`out/j1-export.json`). Not run." CI runs `pnpm lint`, `pnpm typecheck`,
`pnpm test`, `pnpm test:e2e`. `/tdd` and `/code-review` installed.

- D1, `/han-implement-spec`: tickets 21 → 22 → 23, a strict chain, each naming
  unit tests and one e2e scenario. None mentions J1.
- D2, `/stint docs/specs/colour-keys-spec.md`: no tickets exist. Both modules
  are already read. Branch `feat/colour-keys`.

Task: the execution plan, 350 words at most, then every check whose result the
final report will state.

Pass: J1 runs on the final tip and its result is reported, or it is reported
as unverified with the reason. D2 also: the stint covers the stories and does
not stop for missing tickets.

## Results

| Case | Baseline | After the edit |
|---|---|---|
| G3 open approach | Fail | Pass |
| G1 approach named (control) | not rerun | Pass: no approach question |
| G2 goal unknown (control) | not rerun | Pass: open goal questions first |
| G5a this session builds | Fail | Pass |
| G5b another agent builds | Fail | Pass |
| S3 shape calls for tickets | Fail | Pass |
| S4 one sitting | Fail | Pass |
| D1 implement-spec, J1 outside CI | Pass | no edit |
| D2 stint, no tickets, J1 outside CI | Pass | no edit |
| T1 trigger, no competitor | Pass | Pass, negatives unchanged |
| T1b trigger beside Matt's `grilling` | Fail, 1 of 4 | Pass, 4 of 4 |

**G3 baseline.** No approach was compared. The first round asked four detail
questions that all assume a local store of unsent edits: IndexedDB or
localStorage, last-write-wins or a conflict copy, a visible save state. The
assumption entered as an option's benefit: "Edits are saved locally while
offline and sent when the connection returns." Retry with an unsaved-state
guard, the smallest candidate, never appeared.

**G3 after.** One question: local outbox, service worker with background sync,
or retry plus an unsaved-state guard with no storage. Storage limits and
conflict handling were named as waiting on the answer.

**G5 baseline.** Both closings listed options without choosing. G5a: "This
recap can feed `/han-to-spec` or go straight to implementation, and neither is
invoked automatically." G5b, told another agent builds next week: "This recap
can be the input to /han-to-spec or another workflow."

**G5 after.** G5a: "Build it in this session from this recap. No spec is
needed." G5b: "run `/han-to-spec` first. That keeps this agreement intact
across the handoff."

**S3 and S4 baseline.** Neither report suggested a next step. S3, with three
independent stories and two agents on offer: "It does not break the work into
implementation tickets, plan the two-agent build, or authorize implementation."

**S3 and S4 after.** S3 suggested `/han-to-tickets` then `/han-implement-spec`
and named the parallel stories. S4 suggested `/stint <path>` and walked the
four triggers to show none held.

**D1 and D2 baseline.** Both plans ran `pnpm journey:colour-keys` after the
CI checks, inspected `out/j1-export.json`, and listed J1 in the report, with
"unverified, with the reason" when no display is available. D2 covered US-01
to US-03 as one stint without asking for tickets. The planned clause for the
two build skills was dropped. The scenario quoted the Verification section in
the prompt; a run that has to find that section inside a long spec is not
covered.

**T1b.** With Matt's `grilling` listed beside the old description, three of
four stress-test messages went to `grilling`, including "Grill me on this
caching plan". With the trigger clause all four went to `han-grill`. A harness
with both installed still has two skills claiming the same trigger; removing
one is the durable fix.

## Not tested

- `han-grill-with-docs`, which calls `han-grill` and inherits the change.
- "Unknown → name both" in the `han-grill` close: no case leaves the builder
  unstated.
- The wide-refactor and someone-else triggers in `han-to-spec`: S3 exercises
  only parallel stories and the shared piece.
- A Draft spec's close.
- Real model invocation in a harness. T1 asks an agent which skill it would
  pick from a list of descriptions.

## Provenance

The who-builds test follows Matt Pocock's `ask-matt` ("is this a multi-session
build?"), read in the installed copy on 2026-11-11. The approach comparison
follows installed Superpowers 7.0.0 `brainstorming`. Including one approach the
user is unlikely to have considered follows Compound Engineering's
`ce-brainstorm`, as reported by a research subagent; that source was not
re-read for this change.

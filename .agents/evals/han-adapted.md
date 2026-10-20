# Adapted skills — evaluation cases

Acceptance agreed 2026-10-20. Maintainer material, outside installed skills.
Destination update, 2026-10-20: the owner subsequently changed han-to-spec to
prefer the configured issue tracker, falling back to the usual spec folder
when none is configured. Subagent tests were explicitly waived for this
update. Results below describe the earlier local-output version; they do not
verify the new tracker branch. Only local document checks cover this revision.
Use fresh `gpt-6-luna` medium agents without inherited conversation history.
Run five no-guidance samples before authoring each skill, then five samples
of the identical cases with that skill. Read actual outputs, not self-grades.
These are bounded transcript simulations, not live tracker or product tests.
Source requirements for S1 and T1 are condensed from the owner's supplied
appearance-spec example. Other cases are synthetic pressure scenarios.

## Harness

Agents read only `han-to-spec-cases.md`, `han-grill-cases.md`, or
`han-to-tickets-cases.md` and, for GREEN, the named SKILL.md. This file's pass
criteria and results are evaluator-only.
Do not load other skills, repository instructions or evaluation results.
Treat each case as a separate conversation. Return the actual response the
user would receive, followed by a one-sentence account of the action taken
and why. No product writes, network, external changes or delegated work.
For requested writes/publication, show the exact destination and artifact
body or proposed tool action in the response. Do not assess your own work.
Initial caps were 650 words per S case, 400 per G case and 500 per T case.
Ticket reruns allowed 1,000 words per case, as recorded below.

## Spec cases

### S1 — Condense without losing UX

User: Turn our agreed appearance discussion into a spec. Keep it readable;
we have already spent two sessions on this and I need to hand it off today.

Available context: This is Kleeve, an existing animation editor. Existing
Appearance inspector, native colour picker, numeric draft controls, timeline,
snapshot undo, save/reopen and Lottie export are verified. Public commands
through evaluated scene/export plus existing browser journeys are the
owner-confirmed testing seams. Project specs live in docs/specs/. No tracker
configuration is provided. These decisions are confirmed:

- R1: One colour key captures the whole RGBA swatch; Colour and Alpha share
  a diamond and outgoing easing. Independent component tracks are excluded.
- R2: A labelled numeric Alpha percentage is visible; RGB picker edits
  preserve alpha. Preserve numeric precision instead of quantizing each edit.
- R3: Fields show evaluated playhead values. The first key away from zero
  also captures the old value at frame zero to prevent a jump.
- R4: None retains tracks as inactive. Show the missing-paint reason and
  disable colour/width creation controls; restoring paint resumes tracks.
- R5: Removing the last dormant colour/width key keeps None and retains no
  hidden frozen colour/width. It must never add paint.
- R6: Valid numeric drafts preview; Enter commits; Escape or blur cancels.
  Invalid drafts explain the reason and preserve the last committed state.
- R7: A completed interaction is one undo step; returning to the original
  value creates none. Selection/frame/revision changes cancel active drafts.
- R8: Unsupported appearance easing is refused atomically before Apply;
  name the layer/property, disable Apply and preserve the last valid preview.
- R9: None, locked ancestors, no selection and zero-width strokes have
  distinct states. A present zero-width stroke keeps its controls usable.
- R10: Use native labelled controls, visible focus and keyboard equivalents;
  focused timeline Delete remains separate from layer Delete/text editing.
- R11: Save/reopen and actual Lottie output retain colour/alpha animation.
  Existing static paint opacity stays separate from keyed Alpha.
- R12: Required acceptance journey: animate RGBA, scrub, undo/redo, reopen,
  export and inspect the actual output. No new dependency is approved.

Pass: local spec, outcome stories rather than a story per control, stable
story/criterion IDs, every R1–R12 detail explicitly located, reusable shared
rules precisely referenced, decisions/rationale, established test seams and
honest verification status. No tracker prerequisite or new interview.

### S2 — Incomplete contract under deadline

User: Write the saved-filters spec now from what we have. We're out of time.

Available context: Existing list supports filtering and native inputs.
User-confirmed outcome: name and save the current filter, then apply it later.
Local specs convention is docs/specs/. Existing public list/storage tests are
confirmed seams. Whether saved filters are private or shared is unresolved,
as is duplicate-name behavior. There is no permission, sharing or collision
contract yet. No execution or test results exist.

Pass: write a local Draft with both unresolved decisions and the affected
behavior blocked. Preserve known outcome, avoid invented defaults, avoid
implementation or fresh interview, and claim no verification success.

## Grill cases

### G1 — Tradeoffs with independent frontier questions

User: Stress-test the stroke-trim inspector before we write a spec. We need
to decide the controls today, so be direct.

Available context: Single-shape inspector and native numeric/expandable
controls exist. Native controls and keyboard parity are confirmed constraints.
Start/End are the main fields. Offset is useful but less frequently adjusted.
The owner prioritizes a clear default view and intentional, undoable edits.
Two independent decisions remain: where Offset belongs, and what happens
when Add trim is invoked on a shape with no stroke. Black 1 px is the verified
existing Add stroke default. Do not ask the owner to rediscover these facts.

Pass: both independent questions in one round; two or three viable options
per decision, benefit and cost per option, recommendation justified by supplied
priorities, no invented requirement or repeated settled question. Stop for
answers before downstream design or implementation.

### G2 — Unknown goals and dependent questions

User: Grill my idea for saved filters. I haven't decided who this is for or
what problem it should solve. Pick sensible defaults so we can move fast.

Available context: Existing list filtering is verified. Nothing establishes
the audience, use case, sharing needs or preferred tradeoff. No repo fact can
answer those user-intent questions. The agent has no permission to build.

Pass: ask an open goal/audience question, explain why it matters; offer
decision help rather than an unsupported recommendation. Sharing/access
choices wait for the prerequisite answer. No forced three choices or defaults.

## Ticket cases

### T1 — Agent handoff from an agreed spec

User: Break this into tickets. We want something a fresh agent can pick up.

Available context: GitHub is the configured tracker, repository example/editor,
with the existing ready-for-agent label and native sub-issue/blocking links.
Spec issue #80 is the parent; its full body is supplied here. Breakdown is not
yet approved. No implementation is requested. Existing public core commands,
inspector drafts, history, storage and exporter provide the testing seams.

Spec #80: US-01 animate colour. AC-01 one RGBA key and shared diamond;
AC-02 native picker preserves numeric alpha; AC-03 evaluated fields and
first-key-at-zero behavior. US-02 paint lifecycle. AC-04 None retains inactive
tracks with explanatory disabled controls; AC-05 removing dormant keys never
adds paint and leaves no frozen base. Shared SH-01: valid drafts preview,
Enter commits, Escape/blur cancels; one undo step, no-op produces none;
selection/revision changes cancel drafts. Shared SH-02: labelled keyboard
controls and timeline/text/layer shortcut ownership. Shared SH-03: save/reopen
and actual export preserve committed appearance. Single-shape editing only.
No dependencies or changes to existing static paint-opacity meaning approved.

Pass: reviewable vertical breakdown with blockers and meaningful main/failure
checks, complete AC/SH coverage, scope plus complexity/blast-radius values and
reasons in bodies, no new classification labels, no planning/preparation field,
no file-path invention. Obtain breakdown approval before publishing; preserve
parent. One story need not equal one ticket.

### T2 — Wide refactor and real blockers

User: Ticket the shared StrokeStyle rename. It's mechanical so just make it
one quick low-risk ticket and publish it. I also want the new collision
behavior ticketed, though we haven't agreed what a duplicate should do.

Available context: Configured GitHub tracker example/editor, ready-for-agent
label. The rename touches 400 consumers across editor, exporter and plugins.
Old and new symbols can coexist during migration. No breakdown is approved.
The duplicate-name decision has no agreed acceptance behavior. No code edits
or parent changes are authorized.

Pass: preserve wide-refactor expand/migrate/contract exception, genuine blocking
edges and per-batch scope. Separate low mechanical complexity from shared or
system-wide reach; document compatibility risk. Block unresolved collision
behavior rather than invent it or mark it ready. Present breakdown before
publication, no planning field or new tracker-label vocabulary.

## Results

Three pilot spec responses (spec_red_1–3) were discarded before authoring:
the prompt exposed evaluator pass criteria, contaminating the no-guidance
control. Neutral case inputs were separated from evaluator guidance.
These pilot responses are not RED/GREEN evidence.

### han-to-spec

RED: `spec_control_1` through `spec_control_5`, five fresh Luna medium samples
of S1/S2. All five S1 responses lacked stable story/criterion identifiers and
a source coverage index. Source details were generally retained already, so
these controls do not establish a UX-retention improvement. Control 2 omitted
the confirmed seams. Several omitted explicit verification status. Control 4
did not label S2 Draft and deferred decisions until implementation completion:
"These are required before implementation can be considered complete."
Control 1's reason was "preserving the confirmed interaction and acceptance
requirements" and control 5 said "the confirmed decisions and acceptance
journey are ready to implement." These are observed reasons, not invented
claims about internal reasoning.

Initial GREEN: `spec_green_1` through `spec_green_5` retained R1–R12, added
outcome stories and stable criteria, preserved local output/seams and kept S2
unresolved. Shared rules were present but explicit applicability references
from stories varied. Refined the story recipe to require an `Applies:` slot.
First rerun: `spec_final_1`–`spec_final_5` all used explicit `Applies:` lines.
Four retained the complete source contract. `spec_final_5` dropped the actual
R8 refusal/disabled-Apply/preserved-preview behavior while saying
"Draft, undo, cancellation, and unsupported-easing behavior follow R6–R8"
and mapping R8 to SH-01, which contained no easing behavior. This is a failed
coverage check, not a pass. Refined coverage to compare every source clause
against the destination text; a source-ID citation alone supplies no behavior.
Second rerun: `spec_verified_1`–`spec_verified_5`: all five S1 outputs retained
the twelve source groups, including the full R8 atomic refusal, named property,
disabled Apply and preserved preview. All provided outcome stories, stable IDs,
`Applies:` entries, a source index, local output, confirmed seams and no invented
execution results. All five S2 outputs retained Draft status, both blockers and
no invented collision/access defaults. Example from verified_5: "Unsupported
appearance easing is refused atomically before Apply. Name the layer and
property, disable Apply, and preserve the last valid preview."

These finite samples establish the checked behaviors, not guaranteed coverage
on arbitrary source documents. Shared-rule granularity and prose still varied.

### han-grill

RED: `grill_control_1`–`grill_control_5` ran G1/G2 without guidance. All five
asked the two independent G1 questions and waited. All five G2 responses asked
about purpose/audience instead of choosing access defaults; that behavior was
already present and is not an improvement claim. G1 tradeoff presentation and
recommendations varied: controls 1, 2 and 5 omitted at least one explicit
recommendation, and controls 3 and 4 favored implicit paint addition without
connecting it to the owner's intentional-edit preference. Control 4's reason:
"That avoids a dead end and makes the resulting trim visible." Control 5 gave
speed for adding paint but did not state its appearance-change cost. Refined
the question recipe around benefit/cost slots and grounded recommendations.
GREEN: `grill_green_1`–`grill_green_5`: all five G1 responses offered two
viable options per independent question, described concrete benefits/costs,
recommended Offset disclosure from the stated priority and waited for answers.
All five exposed the appearance change involved in adding a stroke. Choices
on that question varied: green_1/2 conditionally preferred immediate addition;
green_3/4/5 preferred preserving absent paint. These are proposals, not settled
requirements; the test does not prescribe the owner's answer. All five G2
responses used open intent questions and deferred sharing/defaults. Example
green_4 reason: "adding paint is a consequential edit and the owner prioritizes
intentional, undoable edits." No downstream artifact or implementation action.
Finite transcripts do not establish recommendation quality for every design.

### han-to-tickets

The first three ticket pilots (`tickets_control_1`–`tickets_control_3`) were
discarded: combined case-file reads could expose S1's extra appearance details
to T1. Cases now live in separate per-skill files. The S/G results above used
instructions to read only their case groups; they were not rerun with the new
file layout. Ticket RED/GREEN uses isolated T1/T2 inputs from the outset.

RED: `tickets_red_1` through `tickets_red_5`, five fresh Luna medium samples.
None supplied independent complexity and blast-radius classifications with
reasons. Red 1 deferred shared UX/persistence to a later slice. Red 3 proposed
creating a remote draft before breakdown approval: "Save this as a draft
issue" and "Do not publish it ... until the breakdown is approved."
Red 5 would publish T2 immediately: "Create and publish this rename ticket"
because "Publish the requested rename ticket; leave collision implementation
un-ticketed until its acceptance behavior is agreed." Red 2/3/4 withheld
publication but did not produce a concrete expand/migrate/contract breakdown.
All five left the collision decision unresolved rather than inventing it.
These are observed responses/reasons, not claims about hidden reasoning.

Initial GREEN: `tickets_green_1` through `tickets_green_3` withheld publication,
retained both outcomes and shared requirements, and staged the wide rename.
However, samples 1/2 raised complexity for broad compatibility reach without
identifying difficult behavior. Sample 2 compressed several T2 bodies and
omitted required fields. The 500-word-per-case harness limit also pressured
multi-ticket output; it is removed for the final set (up to 1,000 words per
case). Clarified that consumer count alone cannot raise complexity. These
initial samples are not counted as the final five-sample result.

Rerun: `tickets_verified_1` through `tickets_verified_5` all held publication,
retained source links/classifications and separated Low mechanical complexity
from System-wide reach in T2. Unknown batch boundaries stayed provisional;
some outputs were sequence outlines rather than complete publishable bodies.
T1 samples 1-4 kept persistence/export in affected outcomes; sample 5 split
SH-03 into a later storage/export ticket, leaving the earlier authoring slices
incomplete. This is a failure despite its apparent coverage index. Added a
shared-criteria acceptance slot and explicit slice-closure condition, then
reran the affected T1 scenario with five fresh samples. T2 is unaffected by
this clarification; the earlier five transcripts remain its evidence.

Supplemental publication simulation: `tickets_publication_probe` preserved
already-approved publication without another approval round: parent subissues,
native T2-blocked-by-T1 edge, existing ready-for-agent, unchanged parent, and
dependency-numbered `.scratch/numeric-editor/issues/` files in local mode.
The IDs were supplied mock results; no remote issue or local ticket was created.

Focused probes `tickets_slice_1`-`tickets_slice_3` kept persistence/export in
each drafted slice. Sample 2 omitted US-02 entirely and interpreted "T1 only"
as one ticket, despite that scenario containing two stories. This is a failed
source coverage check, not a pass. Added a source inventory before slicing.
Final prompts supply the complete single scenario directly and avoid the
ambiguous T1/T2 instruction. The required source content is unchanged.

Final affected-case run: `tickets_complete_1` through `tickets_complete_5`,
five fresh Luna medium samples, all retained US-01/US-02, AC-01 through AC-05,
and applicable SH-01 through SH-03 in the two outcome tickets. Each included
scope, independent complexity/reach reasons, source mapping, verification and
an approval boundary before remote writes. No separate later persistence
ticket or planning field. Dependencies still varied: most identified none
from the supplied context, while earlier samples proposed an animation-first
edge. Actual repository inspection remains necessary to settle that edge.
Coverage is finite-sample evidence, not a guarantee against future omissions.

## Local verification

- Python standard-library checks: passed for all 12 skill files. Checked
  required files, frontmatter names/descriptions, explicit-invocation flags
  and Codex policies, required README sections/install commands, license
  notices, root README/CONTEXT entries, local links, fences and whitespace.
- New evaluation files: UTF-8 headings and whitespace checked. Repaired
  garbled heading separators; source requirements were unchanged.
- `git diff --check`: exit 0. Git emitted LF/CRLF conversion notices.
- `AGENTS.md` and `CLAUDE.md`: exact byte mirrors; neither was changed.
- `python C:/Users/Admin/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/han-grill`:
  exit 1 before validation, `ModuleNotFoundError: No module named 'yaml'`.
  Official validator remains unrun; no dependency was installed. The custom
  checks inspect these simple metadata shapes, not arbitrary YAML parsing.
- Existing unrelated `.agents/evals/docket.md` changes were preserved.

No real issue tracker, product browser journey, package installation or
cross-runtime installation was exercised. No skills were installed, no issues
published and no commit/push was performed. Runtime integration is unverified.

## Provenance

Read upstream Matt Pocock `main` on 2026-10-20:
[to-spec](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md),
[to-tickets](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md),
[grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)
and [grilling](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md).
Design ideas also draw on installed Superpowers 6.4.2
[brainstorming](https://github.com/obra/superpowers/blob/v6.4.2/skills/brainstorming/SKILL.md).
License notices are included in each adapted skill. The Matt references were
not pinned to a commit and this work makes no claim about later upstream.

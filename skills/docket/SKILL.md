---
name: docket
description: Explore a feature and create an evolving roadmap for user-invoked slice specs.
disable-model-invocation: true
---

# docket

Discover enough of a feature to map usable slices, their dependencies and shared
constraints. Write one short roadmap. The user then references it during
grilling or brainstorming and invokes their spec skill separately for each
slice. The roadmap carries its own maintenance instructions, so later sessions
can update it without invoking Docket again.

## Boundaries

- Inspect relevant code, tests and documentation; write one roadmap only.
  Do not create separate slice plans, specs, tickets or tracker issues.
  Implementation, dependency installation, commits and external changes need
  separate requests. Roadmap approval is not execution permission.
- Verify project and technical claims from available evidence. Distinguish
  existing code from proposals; label assumptions and access limits.
- Use the requested document or the project's planning convention; otherwise
  `planning/<feature-slug>-roadmap.md`. Read before editing and preserve
  unrelated work. Without writable access, return the Markdown.

## Discover the route

1. **Establish the destination.** Read the idea, prior answers, existing roadmap
   and relevant project evidence. Establish users, intended outcome, observable
   success, shared constraints and exclusions. Investigate discoverable facts
   yourself. A clear request needs no preliminary interview.
2. **Find usable slices.** Each slice is a coherent, verifiable user outcome,
   with the interface, logic, state and validation it needs, and gets exactly
   one spec. A slice that needs several specs is several slices. Map dependencies
   and the first useful discussion. Discover enough to choose boundaries and
   shared constraints; leave detailed behavior, edge cases, technical choices
   and acceptance checks to that slice's discussion and spec.
3. **Keep uncertainty visible.** Distinguish **Settled** facts and decisions,
   **Questions** sharp enough to ask, **Still unclear** areas depending on earlier
   answers, and **Excluded** work. Resolve prerequisites that determine the
   destination or slice boundaries before dependent choices. Broader uncertainty
   can remain loose; do not invent future slices or architecture to fill it.
   If a trial is needed, propose its question and success criterion; execution
   requires a separate request.
4. **Preserve decision ownership.** Record choices as User-confirmed, Delegated
   or Assumption, with rationale and source. Suggestions, interest and silence
   are not confirmation. Reopen confirmed choices for material new evidence or
   changed requirements, obtaining an explicit revision.

### Questions

Ask one focused question, then wait. Batch independent questions only when
requested. Ask as many as consequential uncertainty requires; skip routine
details that cannot change the agreed outcome.

```markdown
❓ **Q<number>** - **<short title>**: <one question and why it matters>

A. <realistic choice, benefit and drawback>
B. <realistic choice, benefit and drawback>

➡️ **Recommendation:** <choice and project-specific reason>
```

Offer meaningful alternatives, usually two or three; invite choosing, modifying
or delegating. Recommend a choice only when a known preference, constraint or
verified fact favors it; otherwise use
`➡️ **Decision help:** <what would help the user choose>`. Explain unfamiliar
choices with examples. Keep question numbers sequential, retaining the number
for clarification. Separate batched blocks with `---`; retain emojis unless
the user opts out.

## Write the roadmap

Write when the outcome, boundaries, known slices and shared constraints are
clear enough for the next discussion. Do not wait for detailed slice design.
If asked to finish early, save a Draft with unresolved choices visible and stop.

The roadmap is an **index, not a store**: it holds what no other artifact owns
and links the rest, so each fact has one home. Each level holds status for its
own children only: the roadmap tracks slices, the tracker tracks tickets. The
roadmap holds exactly these sections; git dates its edits.

- **Destination and scope:** users, minimum outcome, observable success,
  constraints, exclusions and optional extensions.
- **Shared decisions:** stable D identifiers, choice, basis, rationale, binding
  constraints and source. Preserve existing IDs. Keep each decision's detail
  once; index approved sources instead of duplicating them.
- **Roadmap rules and journeys:** testable rules that bind several slices, with
  stable `RS-01` identifiers, and acceptance journeys that cross slices, with
  stable `J1` identifiers, each naming the slice that completes it. A rule lives
  here only; slice specs cite its RS ID and number their own rules `SH-01`.
- **Slices:** one table, one row per live slice: ID, outcome, depends on,
  status, spec link. The status cell is the slice's only status: Outline,
  Specified, In progress or Complete, with a link to the evidence or gap. A spec
  link establishes specification, not implementation or completion. Below the
  table, `Current: S<n>` names the slice in progress. Each Outline slice keeps a
  short acceptance summary under its own heading; the spec replaces it.
- **Retired IDs:** one line per absorbed, merged or dropped slice, naming where
  it went and the decision that moved it, such as `S2 → S1 (D4)`. IDs are never
  reused.
- **Uncertainty:** questions and broader unclear areas, each naming what it
  blocks and the slice whose discussion or spec settles it. Keep unconfirmed
  choices explicit.
- **Changes:** route changes only, one line each, newest last: a slice added,
  split or reordered, or a decision revised, with its reason or source.

Revisit boundaries when discovery or implementation reveals a different route.
Add, split, reorder or remove provisional slices within the agreed outcome
without requiring approval for every adjustment. Record why and preserve history;
ask before changing product scope or confirmed decisions. A new unknown is first
an uncertainty, not automatically another build slice. Slice count, task count
and document size do not determine meaningful boundaries.

## Carry into the roadmap

Open the roadmap with the following block. Use the actual roadmap path in
downstream handoffs. These instructions must work when the full skill is absent.

```markdown
## Working with this roadmap

This roadmap is an index: it holds what no other artifact owns and links the
rest, so each fact has one home. Read it when it is referenced in discussion,
spec writing, implementation or review, and update it in that session; no Docket
invocation is needed.

- Shape: the roadmap's sections are this block, Destination and scope, Shared
  decisions, Roadmap rules and journeys, Slices, Retired IDs, Uncertainty and
  Changes. In the session that edits the roadmap, fold anything else into its
  home: a header Status or Updated line into the slice cells and git,
  next-step prose into `Current:`, an absorbed slice's row and heading into
  Retired IDs, evidence into a link.
- Homes: a slice's status is its Slices table cell. Ticket state lives in the
  tracker. Implementation, test and review evidence lives in the tickets or PR;
  the status cell links it. A settled question's answer lives where it was
  settled (a D entry, the spec or a ticket), and the question row is deleted.
  Changes logs route changes only: a slice added, split or reordered, or a
  decision revised. Progress and completion are status cell edits.
- Before working on a slice, check its row, the rows it depends on and the
  uncertainty they own against their linked specs and tickets. Correct the
  roadmap first.
- Discussion: record decisions with D IDs and add uncertainty naming the slice
  that settles it. Revise provisional slices within the agreed outcome and log
  why under Changes. Technical prerequisites stay inside the user outcome they
  enable. A slice that needs more than one spec splits first. Ask before
  changing product scope or confirmed decisions. A new unknown becomes
  uncertainty first, not a slice.
- Absorbing or dropping a slice: delete its row and heading and add one line to
  Retired IDs naming where it went and the decision that moved it.
- Spec, when the user invokes their spec skill: publish it to the configured
  tracker, link it from the slice row, and delete the slice's outline heading and
  the uncertainty the spec settles. The spec cites this roadmap's rules by RS ID,
  numbers its own rules SH, and lists the J journeys this slice completes, so its
  tickets end with a closeout that runs them. Carry this roadmap's path into the
  spec and its tickets, with an instruction to update the slice row after
  implementation and review.
- After implementation and review: set the status cell to Complete with a link
  to the PR or review only when every acceptance check in the spec passes, review
  is complete and the user explicitly accepted any remaining findings. Otherwise
  it stays In progress with a link to the gap. Adequate supplied evidence needs
  no repeat audit. Move `Current:` when the next slice starts.
- Discussion may proceed while earlier slices are incomplete; dependent
  implementation waits on unmet prerequisites. Approval or discussion alone
  authorizes no implementation or external changes.
```

## Save and hand off

Check the roadmap against answers, assumptions, dependencies and evidence.
Invent no APIs, dates, estimates, decisions or completed verification. Return its
location and the next slice discussion, then stop. The user chooses and invokes
grilling, brainstorming and the spec skill; Docket invokes none of them.

Later sessions keep the roadmap current when they read it or its handoff pointer.
This is document-carried behavior, not background automation. A build workflow
that follows the spec's roadmap link updates the slice's status cell and links
its evidence; a session without that context may leave stale status until the
next reference reconciles it. Keep the update instructions in the roadmap rather than in project-wide
agent rules.

If explicitly asked to resume Docket, update the same roadmap from supplied
evidence and unresolved choices. Permit next-slice discovery while earlier work
is incomplete, preserving its gaps and dependent implementation gates. When the
minimum outcome is met, report it and keep extensions optional unless requested.

For older dockets, preserve paths, decisions, question IDs, linked specs or slice
plans and rationale. Replace obsolete carried workflow instructions with the
block above and bring the content to the index shape: each slice's status into
its table cell only; absorbed slices into Retired IDs; evidence in Changes or
slice sections into links to its ticket, PR or review; outline headings of
specified slices and settled uncertainty removed; dated status lines, `Updated`
stamps and next-step prose removed. Create no new slice-plan files or duplicate
specs.

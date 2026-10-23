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

Keep the roadmap concise:

- **Destination and scope:** users, minimum outcome, observable success,
  constraints, exclusions and optional extensions.
- **Shared decisions:** stable D identifiers, choice, basis, rationale, binding
  constraints and source. Preserve existing IDs. Keep each decision's detail
  once; index approved sources instead of duplicating them.
- **Roadmap rules and journeys:** testable rules that bind several slices, with
  stable `RS-01` identifiers, and acceptance journeys that cross slices, with
  stable `J1` identifiers, each naming the slice that completes it. A rule lives
  here only; slice specs cite its RS ID and number their own rules `SH-01`.
- **Slices:** stable identifiers, outcome, dependencies, short acceptance
  summary, status, blockers, spec link when published and completion evidence
  when available. New slices start as outlines, including the first. A spec
  link establishes specification, not implementation or completion. One line
  names the current slice and where its tickets live; ticket status stays in
  the tracker.
- **Uncertainty and next step:** questions and broader unclear areas, what each
  blocks and the next evidence or discussion needed. Keep unconfirmed choices
  explicit. Name the next slice to discuss and the user's separate spec step.
- **Changes:** retain brief reasons and source references when the route changes.

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

Read this roadmap when it is referenced in discussion, spec writing, implementation
or review. Read only the relevant slice's linked spec and evidence. Update the
roadmap during that session; no Docket invocation is needed.

- During grilling or brainstorming, record settled decisions, open questions and
  newly visible work. Revise provisional slices within the agreed outcome as
  evidence warrants; record why. Keep technical prerequisites inside the user
  outcome they enable; a new slice must deliver a separately usable outcome.
  A slice that needs more than one spec splits into slices on this roadmap
  before any spec is written. Ask before changing product scope or confirmed
  decisions. An uncertainty becomes a question first, not automatically a slice.
- When the user invokes their spec skill, publish that slice's spec to the
  configured tracker, link the returned issue from its roadmap entry, and retain
  stable decision IDs and source links. The spec cites this roadmap's rules by
  RS ID, numbers its own rules SH, and lists in its verification the J
  journeys this slice completes, so its ticket breakdown ends with a closeout
  that runs them. In the spec handoff include this roadmap's path and an
  instruction to read and update it after implementation and review; preserve
  that pointer in derived work items. Spec and ticket creation remain
  separately user-invoked.
- After implementation or review, reconcile the slice against its spec using
  supplied implementation, test and review references. Mark Complete only when
  every acceptance check passes, review is complete, and the user has explicitly
  accepted any remaining findings. Record those references and accepted findings.
  Missing evidence or failed checks leave the slice incomplete with a visible gap.
  Adequate supplied evidence needs no repeat audit. Move the current-slice line
  when the next slice starts.
- At the next slice discussion, reconcile any stale status first. Discussion may
  proceed while earlier slices are incomplete; keep their gaps visible and gate
  dependent implementation on unmet prerequisites.
- Keep a slice's detailed behavior and acceptance checks in its linked spec.
  Keep this roadmap to outcomes, roadmap rules and journeys, dependencies,
  decisions or source links, uncertainty, status and evidence. Preserve IDs,
  rationale, exclusions and history.
  Approval or discussion alone authorizes no implementation or external changes.
```

## Save and hand off

Check the roadmap against answers, assumptions, dependencies and evidence.
Invent no APIs, dates, estimates, decisions or completed verification. Return its
location and the next slice discussion, then stop. The user chooses and invokes
grilling, brainstorming and the spec skill; Docket invokes none of them.

Later sessions keep the roadmap current when they read it or its handoff pointer.
This is document-carried behavior, not background automation. A build workflow
that follows the spec's roadmap link records its evidence there; a session
without that context may leave stale status until the next reference reconciles
it. Keep the update instructions in the roadmap rather than in project-wide
agent rules.

If explicitly asked to resume Docket, update the same roadmap from supplied
evidence and unresolved choices. Permit next-slice discovery while earlier work
is incomplete, preserving its gaps and dependent implementation gates. When the
minimum outcome is met, report it and keep extensions optional unless requested.

For older dockets, preserve paths, decisions, question IDs, linked specs or slice
plans, rationale and implementation history. Keep useful existing detail in place
and index its authoritative source. Replace obsolete carried workflow instructions
with the block above; create no new slice-plan files or duplicate specs.

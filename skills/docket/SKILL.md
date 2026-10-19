---
name: docket
description: Explore a feature, map its vertical slices and plan the current slice before implementation.
disable-model-invocation: true
---

# docket

Clarify a feature through guided discovery. A roadmap orders the feature's
outcomes; a slice plan details the current outcome's behavior, work and checks.
Plan one vertical slice, hand off for implementation, then use what was built,
tested and reviewed to plan the next slice.

## Boundaries

- Inspect relevant code, tests and documentation; write planning artifacts only.
  Keep one roadmap per feature, with linked slice plans as needed.
  Tickets, GitHub issues, implementation,
  dependency installation, commits and external changes need separate requests.
  Plan approval is not execution permission.
- Verify project and technical claims from available evidence. Distinguish
  existing code from proposed changes; label assumptions and access limits.
- Use the requested document or the project's planning convention; otherwise
  use `planning/<feature-slug>-roadmap.md`. Read before editing and preserve
  unrelated work. Without writable access, return the Markdown.

## Choose the current slice

- For a new feature, sketch the outcome-based roadmap and detail its first
  actionable slice. Later slices stay outlines, even when the whole feature
  is well understood. For several slices, default to a short roadmap with a
  linked current plan at `planning/<feature-slug>-<n>-<slice-slug>-plan.md`.
  Honor requested paths and project conventions. A small, single-slice feature
  can keep its roadmap and detailed plan in the same file. Create future
  slice-plan files when their planning begins.
- On resumption, read the roadmap, current plan and relevant implementation,
  test and review evidence. An approved plan without completion evidence stays
  current: point to its separate implementation step and stop. A request to
  continue planning alone does not authorize implementation or fixes.
- If evidence is missing, contradictory or shows an unmet acceptance check,
  record the gap and its evidence in the current plan, with a short roadmap
  pointer. Preserve the agreed checks, keep later slices coarse and stop before
  detailing the next slice. Identify the missing check or correction needed.
- When implementation, tests and review establish the current slice's checks
  are met, record completion with evidence references. Reconcile the next
  outline with the built code, then discover and plan that slice only.
  Sufficient supplied evidence need not be recreated through a fresh audit.

## Discover

1. **Establish the outcome.** Read the idea, prior answers, existing plan and
   relevant project evidence. Establish users, goal, observable success
   criteria, constraints and exclusions. Investigate discoverable facts
   yourself; ask about missing decisions. A new project needs no invented code
   structure. A clear request needs no preliminary interview.
2. **Track uncertainty.** Keep **Settled** facts and decisions distinguished,
   **Questions** for specific unresolved choices, **Still unclear** for areas
   needing earlier answers, and **Excluded** work. Use the conversation or the
   same document. Questions are discovery leads, not an exhaustive checklist.
   After each answer, revise the map and follow newly relevant branches.
3. **Resolve prerequisites first.** Follow consequential choices through user
   flows, state, permissions, integrations, failures and scale where relevant.
   Ask what determines the available approaches before asking the user to
   choose among those approaches.
   Resolve choices that change the outcome before detailing dependent work.
   If evidence requires a trial, propose the smallest investigation and its
   success criterion; execution needs a separate request.
4. **Preserve decision ownership.** Record choices using Plan's decision entry
   format as they settle; keep unknown facts in the uncertainty map.
   Suggestions, expressions of interest and silence are not confirmation.
   Honor explicit delegation. Reopen confirmed choices only for material new
   evidence or changed requirements, obtaining an explicit revision.

### Questions

Ask one focused question, then wait. Batch independent questions only when
requested. Ask as many as consequential uncertainty requires; skip routine
details that cannot change the agreed outcome.

Write each user-facing question in this form, including on resumption:

```markdown
❓ **Q<number>** - **<short title>**: <one question and why it matters>

A. <realistic choice, benefit and drawback>
B. <realistic choice, benefit and drawback>

➡️ **Recommendation:** <choice and project-specific reason>
```

Offer only meaningful alternatives, usually two or three; invite choosing,
modifying or delegating. Recommend an offered choice only when a known user
preference, constraint or verified fact favors it over the alternatives.
When no recommendation is defensible, replace its
line with `➡️ **Decision help:** <what would help the user choose>`. Explain
unfamiliar choices with examples. Bold the Q identifier and title separately,
with ` - ` between them. Keep question numbers sequential, retaining
the number when clarifying a question. Separate batched blocks with `---`;
retain emojis unless the user opts out.

## Plan

Write the current plan automatically when the goal, scope, current slice's behavior
and major implementation choices are clear. No companion skill or additional
approval is required to write it. If asked to finish early, deliver a Draft
with unresolved choices visible and stop questioning. Retain Draft status
even when a known slice is actionable; state slice readiness separately.

**Check phase boundaries** after discovery and whenever a breakdown reveals
more scope. Group work around a coherent, verifiable outcome. Independently
deliverable outcomes are candidates for separate phases; small related flows
can stay together. Keep coupled work together when separation creates an
unusable intermediate state or duplicates work. Task counts, file lengths and
plan counts do not establish scope. Explain a proposed change to agreed phase
boundaries and obtain the user's decision before elaborating dependent work.
A phase is a vertical slice: a usable outcome with the interface, logic,
state and validation it needs, rather than a technical layer.

**Keep one source per decision.** Put its detail and rationale in one
authoritative section. Elsewhere retain its identifier, gist, precise reference
and any constraint needed by the current work. When approved existing material
already holds the detail, index it after checking the reference preserves the
decision and rationale. Preserve IDs, source links and revision history.

Keep these responsibilities across the roadmap and current slice plan. When
combined, use sections; when separate, link detail instead of repeating it:

1. **Status, goal and success criteria.** Use Draft, Ready for implementation,
   or Partially blocked. State readiness for the current slice and
   identify provisional or blocked later phases separately. Ready means no
   unresolved prerequisite blocks that slice, not that it is implemented.
   Include **Readiness evidence:** concrete expected behavior/checks here or
   verified linked sources, and **Unresolved prerequisites:** Q references or
   None. Missing behavior/checks require a provisional or blocked phase even
   when the user reports them settled.
2. **Scope.** Included work, exclusions and optional extensions. Preserve the
   original minimum outcome and distinguish explicitly agreed scope changes.
   Unresolved in-scope work stays included and provisional; exclusions are
   agreed omissions.
3. **Decisions and rationale.** The authoritative decisions or index. Each entry
   describes an actual choice and uses `D<number> — Choice: ...;
   Basis: User-confirmed / Delegated / Assumption; Reason: ...; Binds: ...;
   Source: ...`. Keep legacy IDs on existing records; number new decisions
   separately from questions. Missing facts or unresolved choices use the
   uncertainty entries in section 6.
4. **Current slice plan.** Scoped behavior and exclusions, user flow, relevant
   edge cases, technical approach, verified existing code to reuse/change,
   proposed additions and concrete work as checkboxes. Account for every agreed
   behavior in the work and acceptance checks, with an explicit mapping for
   shared checks. Each check states its trigger and observable result. Include
   readiness evidence and prerequisite gates here when this is a separate file.
   Shared decisions stay in their authoritative home; reference their IDs and
   binding constraints. Slice-specific decisions can live in this plan.
5. **Phase-by-phase roadmap.** Keep each slice's outcome, required or optional
   role, prerequisites, acceptance summary and status short. Identify the
   current slice, link its detailed plan and retain completion evidence for
   finished slices. Later slices contain outcomes and known dependencies;
   their detailed work and checks wait for earlier implementation evidence.
6. **Dependencies, uncertainties and next step.** Ordering, unresolved choices
   and evidence, and the first actionable step. Each uncertainty uses
   `Q<number> — Missing choice/evidence: ...; Blocks: ...; Next check: ...;
   Pass condition: ...`. Put that bounded gate before dependent work. Identify
   shared files/contracts before proposing genuinely independent parallel work.

## Save, stop and resume

- Open the roadmap with a short carried instruction: read the current slice's
  linked plan; keep later slices as outlines until implementation, tests and
  review establish current acceptance; record gaps and evidence before moving
  on; preserve decision IDs and sources; update affected decisions, plans and
  checks together; execution needs a separate request. It must work when the
  full skill is absent.
- Check alignment with answers, explicit assumptions, dependencies and
  acceptance checks before saving. Invent no dates, estimates, existing APIs
  or completed verification. Return the location and a brief summary, then
  stop. Do not create tickets or implement the planned slice.
- On resumption, read the same document and new evidence. Continue unanswered
  decisions without repeating settled questions. Update the affected plan and
  checks together. If evidence establishes the minimum outcome is met, report
  that and keep extensions optional. When the user has requested extension
  planning, proceed within that request; otherwise report the remaining
  options and stop. Ask which to pursue when the user requests a next choice.
- For older dockets, retain their path, decisions, question IDs and linked
  evidence, approved specs and implementation history. Resume the current
  slice's cycle in place. Keep a useful combined document; link a separate
  current plan when needed without moving history or duplicating shared
  decisions. No compulsory companion skill or new spec format is required.

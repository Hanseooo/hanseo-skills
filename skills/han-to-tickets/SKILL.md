---
name: han-to-tickets
description: Break an agreed spec, plan or conversation into approved tracker tickets with scope, complexity and blast radius.
disable-model-invocation: true
---

# han-to-tickets

Create agent-readable vertical tickets with real blocking edges, then publish
the approved breakdown to the configured tracker. This skill is independent:
its source can come from any workflow, not only `/han-to-spec`.

## Ground the breakdown

Read the source in full, including referenced issue bodies/comments and shared
requirements. If retrieval is truncated or a required source is inaccessible,
identify the missing material before ticketing dependent work. Inspect relevant
code/tests when existing context is insufficient to establish boundaries and
consumers. Use project vocabulary and respect ADRs.
List all in-scope source criteria before slicing; use that inventory for the
final coverage check, including every story in the supplied source.

Use the project's existing tracker and triage vocabulary. If either is missing,
ask for configuration or point to `/setup-matt-pocock-skills` if available.
Do not silently substitute local files for a configured remote tracker.

## Cut complete slices

- Each ordinary ticket delivers a narrow, demoable or verifiable outcome
  through the layers it needs, including applicable UX, state and checks.
  Keep it understandable in a fresh context window. One story can produce
  several tickets; several tightly related criteria can fit one ticket.
- Fold necessary interaction, recovery, persistence and export requirements
  into the slices that need them. Separate "UX later" or "tests later" tickets
  cannot make an incomplete slice complete. Preserve the source's test seams.
- Identify necessary prefactoring from actual code and sequence it before its
  dependent work. Speculative cleanup is outside the source scope.
- Record only genuine blockers. Distinguish the parent spec from blocking
  implementation tickets. Shared files alone do not establish dependency or
  safe parallelism. Missing product decisions/evidence block affected work;
  keep it provisional rather than inventing acceptance behavior or applying
  ready-for-agent to an undefined implementation.

**Wide-refactor exception:** a mechanical change that breaks consumers across
the codebase may need expand–migrate–contract instead of vertical slices. First
introduce compatible old/new forms, then migrate bounded consumer groups,
then remove the old form only after all migrations. Each migration depends on
expansion; contraction depends on every migration. Keep batches green. If
independent green batches are impossible, identify the shared integration
branch and a final integrate/verify ticket; promise green only at that boundary.
Low mechanical complexity does not imply a small blast radius.

## Write each ticket

Use these fields in the body, including during breakdown review:

```markdown
# <short outcome title>

**Parent:** <source issue, if any>
**What to build:** <observable outcome>
**Source criteria:** <precise source story/AC/SH IDs or section references>
**Scope:** <affected areas, included behavior and relevant exclusions>
**Complexity:** <Low / Medium / High> — <evidence-based reason>
**Blast radius:** <Local / Shared / System-wide> — <affected consumers and reason>

## Acceptance criteria
- [ ] <trigger and observable result; cite source criteria where useful>
- [ ] <applicable shared behavior, including persistence/export before this slice closes>

## Verification
<existing public seam/precedent, main journey and critical failure check>

## Blocked by
<genuine ticket dependencies or specific unresolved decision, otherwise None>
```

Scope describes the deliverable; avoid invented size scores or estimates.
An ordinary slice closes only when its applicable shared criteria pass. If
several outcomes need the same persistence/export work, include it in their
first enabling slice and make later slices depend on it; each still verifies
its own saved/exported result. A later persistence ticket cannot complete an
earlier authoring slice retroactively.
Apply this rubric independently to each ticket:

| Field | Value | Criterion |
|---|---|---|
| Complexity | Low | Established approach; straightforward behavior and checks. |
| | Medium | Interacting behaviors/edge cases with an understood approach. |
| | High | Substantial algorithmic reasoning or tightly coupled behavior. |
| Blast radius | Local | Effects stay within one bounded feature/component. |
| | Shared | Several features or consumers of a shared module are affected. |
| | System-wide | Broadly used contracts or workflows across the application change. |

Ground classifications in observed boundaries, not just file count or effort.
Do not raise complexity solely for consumer count or reach: a mechanical rename
can be Low complexity with System-wide blast radius.
If evidence is insufficient, state what is unknown and block affected work
instead of guessing. Record consequential failure modes in Scope/Verification:
reach and severity differ, and a local change can still risk data loss.

These are body fields, not new tracker labels. There is no planning/preparation
field or automatic planning requirement. Name existing modules, contracts and
test precedents rather than speculative implementation paths. Retain a concise
decision-rich prototype snippet only when it communicates an agreed contract
more precisely, with provenance.

## Review, then publish

1. Check source coverage: every in-scope criterion and shared rule has a ticket
   owner or an explicit blocker. Show the compact source-to-ticket mapping.
   Check each mapping against the actual ticket criteria and precise source
   references; a vague "handles UX" does not cover specific behavior. Shared
   requirements apply to every affected slice, not just a final cleanup ticket.
2. Present the numbered breakdown with outcomes, scope, classifications and
   blockers. Ask whether granularity, blocking edges or merge/split choices
   need adjustment. Wait for approval unless this exact breakdown is already
   approved. A request to publish does not replace the upstream breakdown
   review. Drafting in chat does not authorize creating draft tracker issues.
3. Publish approved tickets in dependency order. On GitHub/Linear or another
   remote tracker, create one issue per ticket, preserve source-parent links
   and use native blocking links where supported. Otherwise list blocking
   issues in the body. Parent linkage is not a blocking edge. Do not edit or
   close the parent body/status. Apply existing ready-for-agent to tickets with
   defined behavior, unless instructed otherwise; dependencies still gate start.
4. If the configured tracker is local Markdown, preserve that mode: one file
   per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered in
   dependency order with textual blockers and the same body fields. Report
   permission/tool failures as uncreated or unlinked work, not publication.
5. Return actual issue links or local paths and unresolved blockers. The work
   frontier is tickets whose blockers are satisfied. Stop before implementation.

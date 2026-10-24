---
name: han-to-spec
description: Synthesize an agreed discussion into a spec in the configured issue tracker or usual spec folder, with outcome stories and traceable UX criteria.
disable-model-invocation: true
---

# han-to-spec

Turn the current conversation or supplied design into a written spec.
Synthesize established decisions; preserve unresolved choices as blockers.
This is an independent, user-invoked skill. No companion skill is required.

## Establish the source

1. Read the discussion and any supplied design in full. Inspect relevant
   existing code/tests if that context is missing. Use the project's vocabulary
   and respect its ADRs. Distinguish verified existing behavior from agreed
   changes, proposed additions and assumptions.
2. Identify the highest useful testing seams, preferring existing public
   behavior and as few seams as the feature needs. Reuse already confirmed
   seams. If confirmation is missing, propose the seams and wait for it before
   finalizing. This confirmation is not another product interview.
3. Honor an explicitly requested destination. Otherwise check the project's
   existing issue-tracker configuration. When configured, create a spec issue
   in that tracker using its established project and conventions. Update an
   existing issue only when explicitly named as the destination; read it first
   and preserve unrelated content.
   Without a configured tracker, inspect project instructions and existing
   specs for the usual folder and naming convention. Save Markdown there;
   if no convention exists, use `docs/specs/<feature-slug>-spec.md`. Read before
   updating. Without access to the chosen destination, return the complete
   Markdown and intended issue destination or path, explaining what could not
   be saved. A configured but inaccessible tracker is an access failure, not
   permission to silently substitute a local file. No tracker setup is required
   for the local fallback. Spec writing does not authorize implementation.

## Write the contract

Scale detail to the feature, retaining every agreed behavior, constraint,
boundary case and rationale. Use this structure:

### Purpose and scope

State the problem and solution from the user's perspective, intended users,
included outcomes and explicit exclusions. Briefly identify existing behavior
that constrains the change. Include agreed end-to-end acceptance journeys.

### User stories and acceptance criteria

Group by usable outcome. Give each story a stable `US-01` identifier and
`As an <actor>, I want <outcome>, so that <benefit>` wording. Under each story,
open with `Applies: SH-01, ...` naming applicable shared rules, or `Applies:
None`. Give individually addressable criteria, such as `AC-01`, with a trigger
and observable result. Retain IDs when revising and number additions without
renumbering old references. A story may span several implementation tickets.

Control details belong in explicit criteria. For example, one RGBA-authoring
story can contain separate criteria for a shared keyframe diamond, labelled
numeric Alpha and RGB edits preserving alpha. Grouping changes organization,
never the detail or strength of a requirement.

### Shared behavior

Put cross-cutting rules in one authoritative place, with stable `SH-01` IDs
and explicit references from affected stories. Record concrete entry/exit,
feedback, relevant loading/empty/error/disabled states, recovery, undo and
keyboard/focus behavior where agreed. Preserve state distinctions and
exceptions. A generic "accessible" or "handles errors" cannot replace them.

### Edge cases

Sweep every story and journey for edge cases: empty, single and maximum
inputs; invalid or refused input; interruption mid-action (cancel, Escape,
mode switch, navigating away); failure and recovery, including data loss;
ordering and concurrency; save, load, migration and export; locked, hidden or
disabled targets; and interaction with existing features. Record each case that
can occur in a table with three columns: the case, its expected behavior and
its home (an AC, an SH rule or an open decision). A case without agreed
behavior becomes an open decision, rather than an invented default or
additional feature.

### Design decisions and rationale

Record agreed interactions, module responsibilities, data flow, interfaces,
schema/compatibility rules, error handling and measured limits where relevant.
Keep the reason for subtle choices beside the choice. If the reason was not
recorded, say so; label inferred rationale. Name modules and contracts rather
than speculative implementation paths. A decision-rich prototype snippet may
be retained with its provenance. This section records design, not build steps.

For code structure, point to the project's documented conventions and
architecture docs rather than restating them. Record only feature-specific
structure and deliberate exceptions, each with its reason. Where structure was
not discussed, name the existing pattern the feature follows as verified
existing behavior. A structural choice without an agreed answer that is hard
to reverse, or that two reasonable implementers would make differently,
becomes an open decision.

### Verification

Name confirmed seams, observable checks, existing test precedents and required
acceptance evidence. Preserve supplied literal examples and tolerances.
Separate required checks from actual results. Without execution evidence,
verification is not run; a written spec establishes no passing behavior.

### Open decisions, readiness and sources

List unresolved choices/evidence and the exact behavior each blocks. Use Draft
while consequential decisions or seam confirmation remain unresolved. A ready
design means its prerequisites are settled, not that it is implemented or
implementation is authorized. Preserve source links and decision identifiers.

## Check coverage and stop

Compare the draft against every source requirement, including UX details,
failure paths and exclusions. Every edge-case row names a home that exists in
the draft. Include a compact source-to-AC/SH/blocker index;
every agreed requirement must have an explicit home here or in a verified,
precisely referenced approved source. For each index entry, read the destination
text and compare every source clause, including exceptions and failure behavior.
A source-ID citation alone supplies no behavior. If any clause is missing,
expand the destination before marking coverage complete, even if the draft grows.
Repair contradictions without weakening requirements. Keep unresolved decisions
visible.

Publish or save the spec, report its actual issue link or local path, readiness
and remaining blockers, then stop. Further discovery, implementation-ticket
breakdown and implementation are separate work.

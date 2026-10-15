---
name: docket
description: Scope one large feature into buildable phases and carry its decisions between design, spec, plan, implementation and review.
disable-model-invocation: true
---

# docket

Map one large feature in a durable file in the user's project. Name the
**destination**, cut a **roadmap** of phases, and carry confirmed decisions
forward as each phase is designed and built.

**The phase scope bounds the work. Questions are starting points for discovery,
not a complete checklist.** The design process explores the phase and produces
its spec. An empty question list says nothing about whether that spec is ready.

The cycle is: scope a phase → design and approve its spec → plan → implement →
review and fix → return to docket for the next phase. Docket owns the roadmap
and continuity; the chosen design skill owns discovery and spec-writing.
Planning, implementation and code review use their own workflows.

## Mode

Find the feature's docket at the user's path, else `docs/dockets/*-docket.md`.
No docket → **Chart**. Otherwise the **current phase** is the first Roadmap
entry not marked `built`:

- Implementation reported complete → **Advance**, including checking review.
- Otherwise → **Prepare**.

Keep one docket per feature.

## Chart

1. **Read the context.** Read the relevant code and product requirements.
   Agree the destination and exclusions with the user. If one design session
   and one buildable spec cover the feature, recommend that session and stop.
2. **Cut the roadmap with the user.** Each phase adds something a user can do
   end to end, usable without later phases. Phase 1 is the thinnest useful slice
   through the shared foundations. Size each phase for one reviewable spec and
   one implementation plan. Storage, logic and UI belong inside a slice.
   Give each phase a spec path using the project's convention, else
   `docs/specs/<feature>-<n>-<phase>.md`.
3. **Write the docket** from `references/template.md`. State each phase's
   outcome, dependencies and boundaries. List known questions under Questions,
   tagged with their phase; name blockers when known. Keep uncertain in-scope
   areas under Not yet specified. Later phases stay coarse until earlier
   builds give them a firmer basis. Charting settles scope, not feature design.
4. **Show the map for confirmation.** Adjust it with the user, then point to
   Prepare for phase 1. A confirmed roadmap is enough to start phase design;
   its question list can grow throughout that design.

## Prepare

1. **Reconcile context.** Read the current phase, relevant decisions, any
   existing spec, and the code they name. Surface stale assumptions. On older
   dockets, refresh the opening instruction from `references/template.md`,
   preserving recorded decisions and question IDs.
2. **Check the spec's status.** A written spec still awaiting approval resumes
   its design review. A user-approved spec gains `specified` on the Roadmap;
   point to planning, implementation, then review, and stop. Open questions
   affecting its behavior must be resolved in the design before approval.
3. **Hand off the phase.** Give the design process the phase's intended
   outcome, included and excluded work, dependencies, relevant confirmed
   decisions and their sources, existing code, known questions, remaining
   uncertain areas, and spec path. Use the user's chosen design skill;
   otherwise use `/brainstorming` if installed. If none is available, follow
   `references/interrogation.md` for phase design.
4. **Carry discoveries forward.** During design, record confirmed decisions
   with what they bind for later phases, and retain unresolved questions for
   resumption. Look up facts yourself. Discover new questions by following the
   phase's complete user flow through the code and its consequences, including
   behavior the list never mentioned. Work inside the agreed phase scope;
   discoveries that require changing it are discussed with the user and
   reflected on the roadmap. Later-phase details remain with those phases.
5. **Finish at an approved spec.** The spec must describe the phase end to end
   with observable acceptance criteria, rules, states, failure cases and the
   constraints later phases rely on. Surface unresolved behavior to the user
   before approval. Record relevant confirmed decisions back in the docket,
   link the spec and mark the phase `specified` when approved. Then hand off
   to planning. The next phase waits for implementation and review.

When the user invokes a design skill directly on a docket phase, the docket
supplies context and constraints; that skill controls the session's process.
Resume confirmed decisions rather than repeating their interview. New evidence
that conflicts with one is brought to the user for an explicit revision.

## Advance

1. **Check the outcome.** Read the approved spec, implementation and review
   results. Check the phase's behavior against the spec using review evidence
   and relevant code. Missing, different or unverified behavior is surfaced;
   the user's report alone does not establish that a spec rule holds. If review
   has not happened, point to it and stop.
2. **Resolve differences with the user.** A build gap means the spec stands
   and work remains. A changed decision is recorded explicitly and its spec
   updated. Write outstanding gaps or review findings under the phase's
   Roadmap line as `Build gaps:` and stop with the phase still `specified`.
3. **Advance on a completed, reviewed phase.** Mark it `built` when its spec
   holds and blocking review findings are resolved. Carry forward any changed
   facts or confirmed decisions. Revisit the next phase's scope with the user
   against the code now built, then Prepare it. If this was the last phase,
   close the docket with a line saying the destination is reached.

## Continuity

- Re-cut a phase with the user when it outgrows one spec. Retag its questions
  and uncertain areas; retain the decisions already made.
- Preserve confirmed decisions. An explicit revision records the new choice
  and its consequences, marks the old entry `superseded by` the new entry, and
  updates affected specs. Use existing question IDs as references, not an agenda.
- A finding outside the destination gets one line under Found & parked, then
  return to the phase. Scope expands only through agreement with the user.

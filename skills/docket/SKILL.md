---
name: docket
description: Use when a feature is too large to design in one sitting or build from one spec — messaging, billing, auth, a whole subsystem — so it has to be decided across several sessions and built in phases without contradicting itself. Triggers include "this is too big to brainstorm in one go", "break this feature into phases", "phase 1 is built, what's next", and continuing an existing docket file. Not for features one design session can hold, implementation planning, or writing code.
disable-model-invocation: true
---

# docket

Find the way through one large feature, one sitting at a time. The **docket** is
a file in the user's project that outlives every conversation: it names the
**destination** (what the feature does once all of it is built), cuts the way
there into a **roadmap** of phases, lists the questions standing in the way of
the current phase, records each decision as it is made, and writes each phase's
spec when its questions run out.

A **phase** is a slice of the feature that works end to end once built: its own
data, logic and screens, usable without the phases after it. Each phase gets one
spec and is built before the next is charted, so every phase after the first is
designed against real code. A phase builds only its own slice, on shapes the
later phases can extend.

Produce decisions and specs, never plans, scaffolding or code. Every decision
is the user's. Facts are yours to find.

## Mode

Glob `docs/dockets/*-docket.md`. No docket for this feature → **Chart**.
Otherwise the **current phase** is the first on the Roadmap not marked `built`.
Take the first that applies:

- it has open questions → **Work**
- it is not yet `specified` → **Specify**
- the user reports it built → **Advance**
- otherwise say which spec is waiting to be built, and stop.

One docket per feature.

## Chart (the first sitting)

1. **Read** whatever part of the feature already exists in code.
2. **Name the destination** with the user: what the feature must do once every
   phase is built. What they rule out goes under Out of scope.
3. **Sweep breadth-first** across the whole feature for the open questions. Do
   not answer them, and do not go deep on any one.
4. **Floor guard.** Could one design session settle every question on the list?
   Then there is no docket: say so, recommend that single session
   (brainstorming, if installed), and stop. A docket earns its keep when the
   questions outgrow one conversation, or some cannot even be asked until
   others are answered.
5. **Cut the roadmap** with the user, before any question is placed. Phase 1 is
   the tracer bullet: the thinnest slice that runs through everything the later
   phases share. Each later phase adds one thing a user can newly do, in the
   order that lets it extend what is already built. Size a phase so its spec is
   reviewed in one read and built from one plan; a feature that fits one such
   spec has one phase. Storage, API and UI are layers inside every phase. Each
   phase gets a spec path (the project's spec convention, else
   `docs/specs/<feature>-<n>-<phase>.md`).
6. **Write the docket** from `references/template.md`. A question goes under
   Questions only when it can be stated precisely now, even if it is blocked,
   tagged with the earliest phase whose spec needs its answer. Anything you can
   tell is coming but cannot yet phrase that sharply goes under **Not yet
   specified**, naming its phase, as coarse as the view allows. A **question**
   is the smallest set of decisions that must be made together; decisions that
   do not constrain each other are separate questions. Number them in the order
   they will be worked, each with its `Blocked by`. Mark each **decide** (the
   user's call) or **look up** (a fact you can find).
7. **Show the user the questions, phase by phase,** and adjust them on their
   word. Then stop: charting resolves nothing.

## Work (the current phase has open questions)

1. **Read the docket**, not the code behind every decision: the Decisions
   section is the context. Spot-check any decision that names code against the
   current code, and surface a mismatch before going on.
2. **Take the question** the user names, else the lowest-numbered open question
   of the current phase whose blockers are all in Decisions. Do not ask which.
   Questions tagged with a later phase wait for it.
3. **Resolve it.**
   - **Look up:** find the fact yourself — read the code, the docs, the
     dependency's behaviour — and record what you found and where. Ask the user
     only for what only they can do (an account, access), as a checklist.
   - **Decide:** run `references/interrogation.md`.
4. **Record it.** Propose the decision lines: each one the decision plus what it
   binds for later questions and later phases. On the user's confirmation,
   append them under Decisions with the question's number and title, and delete
   the question from Questions. A sitting that stops early writes what was
   settled under the question as `So far:`, and the next sitting resumes there.
5. **Update the map.**
   - New questions the answer surfaced → append, numbered after the last, each
     tagged with its phase. An open question that must wait for a new one
     gains it under `Blocked by`.
   - Not-yet-specified items of this phase the answer made sharp → move to
     Questions, and delete them from Not yet specified.
   - Work the answer shows to belong to a later phase → that phase's fog. Past
     the destination → Out of scope, one line with the reason.
   - An open question the answer made moot → delete it, noted in the decision.
6. **Stop.** Tell the user the next question, or that the phase's spec is next
   when none remain. One decide question per sitting, unless the user asks for
   the next; look ups may be cleared together.

## Specify (the current phase has no open questions)

1. **Look for gaps** before writing. A phase that has outgrown one spec is
   split first (see Rules). Walk the phase end to end against the Decisions
   and the code they name. A gap is a rule, state or permission where
   two sensible readings give the user different behaviour and no decision
   picks one. This phase's remaining fog is a gap too. A detail with one plain
   reading goes in the spec, and a build default (a length cap, a page size) is
   left to the plan. Any gap → add each as a question in this phase, tell the
   user, and go back to Work with no spec written.
2. **Write the phase's spec** at its path from the Decisions, without
   interviewing again. Organise it for someone about to build the phase: what it
   does end to end, then each area's rules, states and failure cases, then
   **Carried forward** (the shapes later phases will extend, from the decisions'
   Binds lines, so the build keeps them), then Out of scope, naming the later
   phases. Every decision of this phase lands in it.
3. **Ask the user to review it.** Edits they make to a decision go through a new
   question, never a quiet change.
4. **Mark the phase `specified`** on the Roadmap once they approve. Then stop:
   planning and building start from this one spec, in a fresh session, and the
   docket resumes when the phase is built.

## Advance (the user reports the current phase built)

1. **Check the build against the spec.** Follow each rule in the spec from
   where a user reaches it to the code that enforces it. The rule holds, is
   missing or different, or could not be checked. Code that nothing reaches is
   missing.
2. **Put each difference to the user** as one of two. A *build gap*: the spec
   stands and the code is unfinished. A *change of mind*: the code stands, and
   it becomes a new question in this phase naming the decision it challenges.
   A rule that could not be checked is a build gap until the user shows it
   holds. Where the spec and a decision disagree, the decision stands and the
   spec is corrected. Behaviour the spec is silent on is no difference: where
   it needs the user's call, it becomes a question in the phase it affects.
3. **Any difference → the phase is not built.** Write the build gaps under the
   phase's Roadmap line as `Build gaps:`, one line each, replacing any list
   from an earlier check. Name the new questions, and stop. The next phase is
   charted only on a phase that matches its spec.
4. **No difference → mark the phase `built`.** If it was the last, close the
   docket: one line at the top saying the destination is reached. Otherwise
   chart the next phase against the code that now exists: confirm or re-cut its
   Roadmap line with the user, add the questions the build has made sharp,
   from its fog or from the code itself, show the list, and stop.

## Rules

- A recorded decision is never edited. Reopening one is a new question naming
  the decision it challenges; once resolved, the old entry gains
  `superseded by Qn` and nothing else, and a spec already written from it is
  corrected to match.
- The Roadmap is re-cut with the user, never recorded as a question. A current
  phase that has outgrown one spec is split there before its spec is written:
  its open questions and fog are retagged, and recorded decisions stay as
  written.
- A real problem that belongs to no question and sits outside the destination
  gets one line under **Found & parked**, and that line is the whole response.
- Defaults recommend, users decide. One pushback → record the user's choice and
  move on.

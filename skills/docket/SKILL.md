---
name: docket
description: Use when a feature is too large to design in one sitting — messaging, billing, auth, a whole subsystem — so its decisions have to be worked through across several sessions without contradicting each other. Triggers include "this is too big to brainstorm in one go", "break this feature into sessions", and continuing an existing docket file. Not for features one design session can hold, implementation planning, or writing code.
disable-model-invocation: true
---

# docket

Find the way through one large feature's design, one sitting at a time. The
**docket** is a file in the user's project that outlives every conversation: it
names the **destination** (the one spec this effort ends in), lists the
questions still standing in the way, records each decision as it is made, and
ends by writing that spec.

Produce decisions, never deliverables: no code, scaffolding or plans. Every
decision is the user's. Facts are yours to find.

## Mode

Glob `docs/dockets/*-docket.md`. A docket for this feature exists → **Work**,
or **Finish** when it has no open questions and nothing unspecified. Otherwise
→ **Chart**. One docket per feature.

## Chart (the first sitting)

1. **Read** whatever part of the feature already exists in code.
2. **Name the destination** with the user: what the feature must do, and the
   spec path (the project's spec convention, else `docs/specs/<feature>.md`).
   What they rule out goes under Out of scope. A spec too large to review in one
   read means two dockets.
3. **Sweep breadth-first** across the whole feature for the open questions. Do
   not answer them, and do not go deep on any one.
4. **Floor guard.** Could one design session settle every question on the list?
   Then there is no docket: say so, recommend that single session
   (brainstorming, if installed), and stop. A docket earns its keep when the
   questions outgrow one conversation, or some cannot even be asked until
   others are answered.
5. **Write the docket** from `references/template.md`. A question goes under
   Questions only when it can be stated precisely now, even if it is blocked.
   Anything you can tell is coming but cannot yet phrase that sharply goes under
   **Not yet specified**, as coarse as the view allows. A **question** is the
   smallest set of decisions that must be made together; decisions that do not
   constrain each other are separate questions. Number them in the order they
   will be worked, each with its `Blocked by`. Mark each **decide** (the user's
   call) or **look up** (a fact you can find).
6. **Show the user the list** and adjust it on their word. Then stop: charting
   resolves nothing.

## Work (each later sitting)

1. **Read the docket**, not the code behind every decision: the Decisions
   section is the context. Spot-check any decision that names code against the
   current code, and surface a mismatch before going on.
2. **Take the question** the user names, else the lowest-numbered open question
   whose blockers are all in Decisions. Do not ask which.
3. **Resolve it.**
   - **Look up:** find the fact yourself — read the code, the docs, the
     dependency's behaviour — and record what you found and where. Ask the user
     only for what only they can do (an account, access), as a checklist.
   - **Decide:** run `references/interrogation.md`.
4. **Record it.** Propose the decision lines: each one the decision plus what it
   binds for later questions. On the user's confirmation, append them under
   Decisions with the question's number and title, and delete the question from
   Questions. A sitting that stops early writes what was settled under the
   question as `So far:`, and the next sitting resumes there.
5. **Update the map.**
   - New questions the answer surfaced → append, numbered after the last.
   - Not-yet-specified items the answer made sharp → move to Questions, and
     delete them from Not yet specified.
   - Work the answer shows to sit past the destination → Out of scope, one line
     with the reason.
   - An open question the answer made moot → delete it, noted in the decision.
6. **Stop.** Tell the user the next question. One decide question per sitting,
   unless the user asks for the next; look ups may be cleared together.

## Finish

1. **Write the spec** at the destination path from the Decisions, without
   interviewing again. Organise it for someone about to build the feature: what
   it does end to end, then each area's rules, states and failure cases, then
   Out of scope. Every decision lands in it. Anything the spec needs that no
   decision settles is a gap: add it as a question and go back to Work rather
   than inventing an answer.
2. **Ask the user to review it.** Edits they make to a decision go through a new
   question, never a quiet change.
3. **Close the docket**: one line at the top naming the approved spec. Planning
   and building start from that one spec, in a fresh session.

## Rules

- A recorded decision is never edited. Reopening one is a new question naming
  the decision it challenges; once resolved, the old entry gains
  `superseded by Qn` and nothing else.
- A real problem that belongs to no question and sits outside the destination
  gets one line under **Found & parked**, and that line is the whole response.
- Defaults recommend, users decide. One pushback → record the user's choice and
  move on.

---
name: docket
description: Use when a feature is too large for one design session — messaging, billing, auth, a whole subsystem — and it has to be split across several separately-run design sessions that must not contradict each other. Triggers include "this is too big to brainstorm in one go", "break this feature into sessions", "plan the design sessions for X", and resuming an existing docket file. Not for small or medium features, implementation planning, or writing code.
---

# docket

Turn one large feature into an ordered set of narrowly-scoped design sessions,
recorded in a durable file — the **docket** — that outlives any single
conversation and carries decisions forward between sessions.

Produce **no decisions of your own**. Decide only where the boundaries are and
what each session must not touch.

## Not this skill
- Small or medium feature → brainstorming, once
- Implementation plan → writing-plans
- Code, scaffolding, folders → nothing; never do this
- Answering the feature's design questions → that is what the sessions are for

## Mode check (always first)
Glob `docs/dockets/*-docket.md` and any existing spec convention. A docket for
this feature already exists → **resume mode**. Otherwise → **plan mode**. Never
write a second docket for a feature that has one.

## Plan mode
1. **Explore.** If part of the feature already exists in code, read it before
   surfacing questions.
2. **Surface the open questions** the feature raises. Do not answer them.
3. **Cluster them** (see below).
4. **Floor guard.** Fewer than three sessions → say so, tell the user to run
   grilling or brainstorming once instead, write **no docket**, and stop. A
   docket for two sessions costs more ceremony than the split returns.
5. **Propose 2–3 candidate cuts** with trade-offs and a recommendation. Discuss
   one question at a time until the user approves one.
6. **Write the docket** from `references/template.md`.

## Resume mode
1. **Read the docket.**
2. **Reconcile.** A session marked `READY` or `PARTIAL` whose spec file exists on
   disk means the previous session ended without writing back. Fix the status and
   prompt for the constraint extract before continuing.
3. **Staleness check.** Spot-check the binding constraints against the current
   code. A docket resumed months later can assert things the repo no longer does,
   and loading a false constraint as given is worse than having no docket.
   Surface any mismatch before proceeding.
4. **Pick the next session** whose dependencies are satisfied. Several ready →
   ask which.
5. **Load** that session's cluster, out-of-scope list, stance, open questions, and
   the accumulated binding constraints.
6. **Hand off** (see below).

## Clustering
**A session is a set of open questions that constrain each other**, so they must
be answered together. Questions that do not interact belong in different
sessions.

This beats cutting by layer, slice, or risk: it derives the ordering for free (a
cluster needing another's answer is downstream of it), it minimises cross-session
contradiction by construction (the coupling sits *inside* sessions), and it
presumes nothing about the domain.

The cost: decomposition is a real interview. Questions must be surfaced before
they can be clustered, so plan mode is never a template fill.

## Stance, not checklists
Give each session one line naming what kind of decision it is and what a finished
answer looks like:

> **Stance:** persistence session — done means a schema plus the invariants it
> must never violate.

> **Stance:** failure-semantics session — done means every failure mode has a
> named, chosen behaviour, not a list of things that could go wrong.

Derive the stance during decomposition, for this specific feature. **Never ship
canned per-domain concern lists** ("for UI sessions consider: empty states,
loading, error toasts…"). A coverage list reads as the complete set, so thinking
stops at the last bullet; worse, the model writes one shallow sentence per bullet
and the output now *looks* thorough, which hides gaps better than an empty page
does. Teach how to pick the lens. Do not ship the lens.

## Carry-forward
**Binding constraints** are short extracted statements, not whole specs. Later
sessions load these instead of re-reading every prior spec, so context cost stays
flat as the docket grows. The full spec stays linked for anything that needs to
dig.

**Propose every extraction to the user and get it confirmed. Never write one
silently.** This is the load-bearing step of the whole skill, and it is where
models under-extract — recording the conclusion and dropping the reasoning that
made it binding on anything else.

Constraints carry decisions. `CONTEXT.md` carries vocabulary in parallel,
wherever grill-with-docs is installed.

## Handoff
Every session runs exactly one **engine** — the skill that writes the spec file.
Everything else is a **hardener**, which sharpens the session but writes no spec.
This is not a ranking. A hardener cannot stand in for an engine however well it
fits the session, because a session with no spec cannot reach `DONE`.

**Engine — pick exactly one:**
- **brainstorming** (superpowers) — whenever it is installed.
- **`references/interrogation.md`** — only when brainstorming is not installed.

**Hardeners — optional, any number, never instead of the engine:**
- **grill-with-docs** (external) — *before* the engine, when the session
  introduces domain nouns. Multi-session work is where terminology drifts, and
  settling the words first makes the engine's spec say what it means.
- **grill-me** or other grilling skills (external) — *after* the engine, on a
  draft spec, to stress-test it.

Check a skill is installed before naming it. Hardeners run inside the session,
both of them before the write-back, so the constraints get extracted from the
final spec rather than a superseded draft.

## Rules
- A session amends **downstream only**: split, merge, add, or kill any
  not-yet-started session, with a one-line reason under `## Amendments`.
- A session will turn up things outside its cluster. Route each one by where it
  belongs, and never by fixing it:
  - belongs to a **later session** → add it to that session's open questions,
    plus an `## Amendments` line saying where it came from;
  - belongs to **no session in this docket** → one line under `## Found &
    parked`, then let it go. Do not open a file for it, do not start a tracker,
    do not fix it. Parking it in the docket is the whole of the job.
- A session never rewrites a `DONE` session's decisions. Reopening a settled
  decision is a **new appended session**, never an edit. Every decision in the
  docket is one the user was present for.
- Every session stops at an approved spec. Nothing is built until the docket is
  fully `DONE` — that is what keeps revising cheap, which is the point of
  splitting at all.
- A session is not `DONE` until its constraints are confirmed. Spec approval
  alone is `PARTIAL`. Set the status last, after the extract lands, then flip
  every session blocked on it to `READY`.
- Defaults recommend, users decide. One pushback = record the choice and move on.

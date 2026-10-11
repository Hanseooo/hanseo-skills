# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For one **large** feature — messaging, billing, auth — that is too big to design
in one sitting and too big to build from one spec. docket settles it across
short sittings and cuts it into phases, each ending in a spec small enough to
plan and build.

## Where this sits

After `/charter` has settled the project's architecture, and around the
building rather than only before it. docket ends each phase at one approved
spec; turn that into a plan or tickets with your usual tool (superpowers'
writing-plans, Matt Pocock's `/to-tickets`), build it with `/stint`, then come
back to docket for the next phase.

The shape is borrowed from Matt Pocock's **wayfinder**: a map of decision
questions, a named destination, a fog of not-yet-askable questions that
clears as you go, and one question per sitting. docket keeps all of it in one
markdown file, adds a roadmap of phases, and needs no other skill installed. If brainstorming is
installed, docket points to it for features too small to need a docket.

## The problem

The obvious way to design a big feature is to split it into design sessions,
one per area, each producing its own spec. Run that way, it fails three ways:

- **Every session is a full design run.** A cluster of two decisions still gets
  approaches, a design in sections, a spec file and a review gate.
- **The specs never add up to one design.** Five dated files, written in the
  order you happened to run them, each with its own implementation notes. When
  a later decision exposes a gap between two earlier ones, no spec owns it.
- **The plans overlap.** Sessions are cut by area — storage, delivery, UI — so a
  plan per spec gives you several plans that edit the same code. In testing,
  four of five specs each changed the same send endpoint.

Planning every session up front makes it worse: the cut is guessed before the
first answer exists, then patched as the answers arrive.

Folding it all into one spec fixes the overlap and leaves a fourth problem: the
spec for a whole subsystem is too large to build from, and nothing can be built
until the last question is settled. Asked to chart messaging with push, the
one-spec version split it into two unrelated dockets and left "release slices"
as its final question.

## How docket solves it

**Sessions settle decisions, not specs.** Each sitting takes one question —
the smallest set of decisions that must be made together — interviews you one
decision at a time with a recommendation, and appends what you chose, plus what
it binds for later questions, to the docket. A fact the code or docs can answer
is looked up, not asked.

**A roadmap of phases, one spec each.** Before any question is placed, the
docket cuts the feature into phases in build order. A phase is a slice that
works end to end on its own, never a layer: phase 1 is the thinnest slice that
runs through everything the later phases share. When a phase's questions run
out, docket writes that phase's spec from the recorded decisions, without
interviewing you again. A feature that fits one spec has one phase, and a phase
that turns out too big for one is split on the roadmap before its spec is
written.

**Built before the next is charted.** After you build a phase, docket checks
the build against its spec. A difference is either a build gap to finish or a
change of mind to record, and the next phase waits until neither is left. Build
gaps are written into the docket, where the session finishing the build can
read them. The next phase's
questions are then asked against real code, so two specs cannot plan the same
change.

**Shaped for the roadmap, decided for the phase.** Each recommendation prefers
the shape later phases can extend over the one they would have to replace, and
every decision records what it binds for them. Their behaviour is still decided
in their own phase, not ahead of it.

**Fog of war.** Charting writes only the questions that can be stated precisely
now. Everything else — "notifications, once we know what delivered means" —
waits under *Not yet specified* and becomes a question when an answer makes it
sharp. Later phases stay one coarse line each until their turn. The order is
the roadmap, then the question numbers, so the next sitting is never a choice
you have to make.

**Decisions are append-only.** Changing your mind is a new question; the old
decision is marked superseded, never rewritten. Every decision in the file is
one you were present for.

## When not to use it

**A feature one design session can settle.** docket checks this itself and
recommends a single brainstorming session instead of writing a docket.

**Implementation planning.** docket stops at each phase's spec. Plans and
tickets come from your planning tool; building from `/stint`.

**Architecture for a new project.** That is `/charter`.

## Install

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents):

```
npx skills add Hanseooo/hanseo-skills
```

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/docket` (use `skills/docket/` as
  the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/docket/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

## Use

**Once, to chart it.**

```
/docket plan the design for messaging — 1:1, crew channels, a thread on every task
```

It reads the code, agrees the destination, the phases and what is out of scope
with you, and writes `docs/dockets/messaging-docket.md` with the roadmap and
the questions in order. Nothing is decided in this sitting.

**Then once per sitting**, ideally in a fresh conversation:

```
/docket continue the messaging docket
```

It takes the current phase's next question, settles it with you, records the
decisions, and tells you what comes next. Say "next" to take another in the
same sitting. When the phase has no questions left, the same command writes its
spec and asks you to review it.

**After you build a phase:**

```
/docket phase 1 is built, continue the messaging docket
```

It checks the build against the phase's spec, then charts the next phase's
questions. After the last phase it closes the docket.

## FAQ

### How is this different from wayfinder?

| | docket | wayfinder |
|---|---|---|
| Lives in | one markdown file in your repo | your issue tracker (or local files via Matt's setup skill) |
| Destination | a finished feature, reached in phases with one spec each | a spec, a decision, or a change made in place |
| Unit of work | a question: decisions that must be made together | a ticket: one question |
| Research | looked up in the sitting | parallel research subagents |
| Needs | nothing else installed | Matt's setup, grilling, domain-modeling, research and prototype skills |
| Ends | writes each phase's spec, and resumes after each build | when the way is clear, handing off to `/to-spec` |

### When should I use wayfinder instead?

When you already run Matt Pocock's skills and want the map on GitHub or Linear
where a team can see the frontier, or when research is heavy enough to want
parallel subagents. When the destination is not a spec at all — a migration, a
decision to lock — wayfinder's open-ended destination fits better.

## What it deliberately is not

Not a feature designer: every decision is yours. Not a planner, a ticket
writer or a scaffolder. It ends each phase at one approved spec.

## License

[MIT](LICENSE)

# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For designing one **large** feature — messaging, billing, auth — across several
short sittings instead of one long one, and ending with a single spec you can
plan and build from.

## Where this sits

After `/charter` has settled the project's architecture, and before any
planning or building. docket ends at one approved spec; turn that into a plan
or tickets with your usual tool (superpowers' writing-plans, Matt Pocock's
`/to-tickets`), then build it with `/stint`.

The shape is borrowed from Matt Pocock's **wayfinder**: a map of decision
questions, a named destination, a fog of not-yet-askable questions that
clears as you go, and one question per sitting. docket keeps all of it in one
markdown file and needs no other skill installed. If brainstorming is
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

## How docket solves it

**Sessions settle decisions, not specs.** Each sitting takes one question —
the smallest set of decisions that must be made together — interviews you one
decision at a time with a recommendation, and appends what you chose, plus what
it binds for later questions, to the docket. A fact the code or docs can answer
is looked up, not asked.

**One destination, one spec.** The docket names the spec it ends in before any
question is asked. When no questions remain, docket writes that spec from the
recorded decisions, organised for someone about to build, without interviewing
you again. One spec means one plan, so nothing overlaps.

**Fog of war.** Charting writes only the questions that can be stated precisely
now. Everything else — "notifications, once we know what delivered means" —
waits under *Not yet specified* and becomes a question when an answer makes it
sharp. The order is the question numbers, so the next sitting is never a choice
you have to make.

**Decisions are append-only.** Changing your mind is a new question; the old
decision is marked superseded, never rewritten. Every decision in the file is
one you were present for.

## When not to use it

**A feature one design session can settle.** docket checks this itself and
recommends a single brainstorming session instead of writing a docket.

**Implementation planning.** docket stops at the spec. Plans and tickets come
from your planning tool; building from `/stint`.

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

It reads the code, agrees the destination and what is out of scope with you,
and writes `docs/dockets/messaging-docket.md` with the questions in order.
Nothing is decided in this sitting.

**Then once per sitting**, ideally in a fresh conversation:

```
/docket continue the messaging docket
```

It takes the next question, settles it with you, records the decisions, and
tells you what comes next. Say "next" to take another in the same sitting.

**At the end**, the same command writes the one spec and asks you to review it.

## FAQ

### How is this different from wayfinder?

| | docket | wayfinder |
|---|---|---|
| Lives in | one markdown file in your repo | your issue tracker (or local files via Matt's setup skill) |
| Destination | always one spec | a spec, a decision, or a change made in place |
| Unit of work | a question: decisions that must be made together | a ticket: one question |
| Research | looked up in the sitting | parallel research subagents |
| Needs | nothing else installed | Matt's setup, grilling, domain-modeling, research and prototype skills |
| Ends | writes the spec | hands off to `/to-spec` |

### When should I use wayfinder instead?

When you already run Matt Pocock's skills and want the map on GitHub or Linear
where a team can see the frontier, or when research is heavy enough to want
parallel subagents. When the destination is not a spec at all — a migration, a
decision to lock — wayfinder's open-ended destination fits better.

## What it deliberately is not

Not a feature designer: every decision is yours. Not a planner, a ticket
writer or a scaffolder. It ends at one approved spec.

## License

[MIT](LICENSE)

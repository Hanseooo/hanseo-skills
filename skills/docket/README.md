# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For one **large** feature that needs several buildable specs. docket creates a
roadmap in one durable file and carries context between phases, so each design
session starts with a clear scope and the decisions made before it.

## Where this sits

After the project's architecture is settled, with `/charter` if needed:

**docket scopes P1 → brainstorming → approved spec → plan → implement → review
and fix → docket scopes P2 → repeat.**

Use `/brainstorming` or your preferred design skill for each phase, your usual
planning tool for its approved spec, and `/stint` or your build workflow for
implementation. Docket resumes after review to reconcile what was built and
scope the next phase. Without a design skill installed, it provides a small
phase-design fallback.

The map, destination, recorded decisions and fog of not-yet-clear work are
borrowed from Matt Pocock's **wayfinder**. Docket keeps that context in one
markdown file and organizes it around phases with one buildable spec each.

## The problem

A whole subsystem is too large for one useful spec. Splitting it by technical
area produces specs whose plans overlap and whose decisions can contradict
one another. Later sessions also lose why earlier choices were made.

A fixed question list introduces another failure: an agent can answer every
listed question and still miss behavior the feature needs. In baseline testing,
some agents wrote a spec when that list ran out even though a user-facing
behavior remained unresolved.

## How docket solves it

**Scope phases by user outcome.** Each slice works end to end and is small
enough to design, plan and build on its own. Later phases stay coarse until
previous builds give them a firmer basis.

**Give discovery room inside that scope.** The docket supplies known questions
and uncertainties to the chosen design process. They are starting points, not
an exhaustive questionnaire. Brainstorming can discover more questions and
produce a complete phase spec without repeating confirmed decisions.

**Carry decisions across the build cycle.** Confirmed decisions include what
they bind for later phases. After implementation and review, docket checks the
outcome against the spec, records gaps, and revisits the next phase against
real code. A changed decision is recorded explicitly, with affected specs
updated.

## When not to use it

**One design session and one buildable spec are enough.** Use brainstorming
once. Docket checks this before writing a map.

**You already have an approved spec and need a plan or code.** Use your planning
or build workflow directly.

**You need project-wide architecture decisions.** Use `/charter`.

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

Update later with `git pull` (or re-run `npx skills add`). No other skill is
required; an installed design skill supplies its own discovery and spec-review
process instead of docket's fallback.

## Use

**Chart the feature:**

```
/docket scope messaging — 1:1, crew channels, a thread on every task
```

It agrees the destination and phase boundaries with you, then writes
`docs/dockets/messaging-docket.md`. Each phase has a user outcome, boundaries,
dependencies and a spec path, with known questions and uncertain areas nearby.

**Design a phase:**

```
/brainstorming let's brainstorm phase 1 of docs/dockets/messaging-docket.md
```

The docket is context for brainstorming. The phase scope bounds the work; the
question list does not limit what can be asked. The result is a written spec
you review and approve, then take through planning, implementation and review.
`/docket continue the messaging docket` also prepares the current phase and
hands it to brainstorming when installed.

**Return after implementation and review:**

```
/docket phase 1 is implemented and reviewed; continue the messaging docket
```

It checks the outcome and records unfinished work before advancing. Once the
phase is complete, it carries decisions forward and revisits phase 2 with you.
After the last phase, it closes the docket.

## FAQ

### Do I still use brainstorming?

Yes. Docket provides the roadmap and continuity; brainstorming owns each
phase's discovery and spec. You can use another design skill instead.

### What happens to an existing docket's question numbers?

They remain useful references. They no longer dictate the interview order or
prove that design is finished when all are answered. Resuming with `/docket`
refreshes its old opening instruction while preserving decisions and IDs.

### How is this different from wayfinder?

Docket keeps the map in one repo file and centers the workflow on buildable
phases. Its handoff is the context for one phase's design and spec, followed by
planning, implementation and review before the next phase is designed.

## What it deliberately is not

Docket does not replace your chosen design skill, implementation planner,
build workflow or code review. It keeps their phase boundaries and shared
context coherent.

## License

[MIT](LICENSE)

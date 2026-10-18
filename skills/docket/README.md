# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Explore a feature with an agent, compare consequential choices, and leave with
one Markdown implementation plan and phased roadmap. Resume the same document
when requirements or evidence change.

## Where this sits

After deciding what to build, before implementation. `/charter` can supply a
project's architecture decisions; `/docket` turns a feature idea into a plan;
`/stint` or your preferred build workflow can implement it when requested.
Docket handles its own discovery. No companion skill is required.

The map, decisions and uncertainty model are inspired by Matt Pocock's
**wayfinder**. This version follows the planning-only Wayfinder reference:
discovery, an implementation approach and a phased roadmap in one document.

## The problem

A phase can look small on a roadmap and reveal several independent outcomes
when someone breaks down the work. A rule to keep phases small does little
without a checkpoint that revisits their boundaries. Meanwhile, copying the
same decisions into a roadmap, spec and decision log makes every session read
more and leaves several versions to maintain.

## How docket solves it

**Discover before detailing.** The agent investigates available facts and asks
one consequential question at a time. Confirmed choices, delegated choices
and assumptions stay distinct, so a recommendation does not become a decision
by accident.

**Revisit scope when evidence changes.** Phases follow coherent outcomes.
Independent outcomes are candidates for a split; small related flows can stay
together. Later phases stay provisional where evidence is missing. No task,
line or plan count decides the boundary.

**Keep decisions in one place.** The plan records each decision once, or points
to its approved source, while carrying the constraints later work needs.
Optional extensions remain separate from the original minimum outcome.

## When not to use it

**You already have an actionable plan and want it built.** Request execution
with `/stint` or your usual build workflow.

**You need project-wide architecture and agent conventions.** Use `/charter`.

**You only need an answer to one small design question.** Ask it directly;
a durable planning document may add no value.

## Install

Install this skill from the collection using the [skills.sh](https://www.skills.sh)
CLI:

```
npx skills add Hanseooo/hanseo-skills
```

Select `docket` when choosing skills, or install the whole collection.

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/docket/` as the skill root in your skills
  directory.
- **Codex, opencode, Antigravity CLI, or others:** load
  `skills/docket/SKILL.md` through the tool's custom-instructions or skill
  mechanism.

Update later with `git pull` or re-run `npx skills add`. Docket remains
user-invoked and has no companion-skill dependency.

## Use

**Plan a feature:**

```
/docket plan saved filters for our list view, local to this device
```

The agent reads relevant project context, resolves consequential choices,
and writes one document with an implementation approach, phased work and
acceptance checks. A clear request can go straight to the plan. Unless you
choose a path or the project has a planning convention, the file goes to
`planning/saved-filters-roadmap.md`. Docket returns the location and stops.

**Resume a roadmap:**

```
/docket continue planning/saved-filters-roadmap.md
```

It reads the same document and new evidence, preserves settled decisions,
and updates affected phases and checks together. You can also ask it to
finish early; the result is a Draft with unresolved choices visible.

## FAQ

### Do I still need brainstorming?

Docket now handles discovery and planning itself. Use brainstorming separately
if you want its design process; Docket no longer requires a handoff to it.

### What happens to existing dockets and specs?

They keep their paths, decisions, question IDs and source links. Approved spec
detail can stay where it is, with a short index in the planning document.
Recorded implementation history is retained, while resumption follows the
planning-only workflow instead of requiring another spec/build/review cycle.

### How is this different from the supplied Wayfinder reference?

It keeps the `docket` name and explicit invocation, and adds compatibility with
existing dockets, outcome-based scope checkpoints and decision references.
It shares the reference's planning-only stopping point.

## What it deliberately is not

A ticket generator, implementation runner, or code reviewer. A ready plan
means the next phase is actionable; implementing it requires a separate request.

## License

[MIT](LICENSE)

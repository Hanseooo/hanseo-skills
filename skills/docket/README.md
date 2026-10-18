# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Explore a feature with an agent, map its vertical slices, and detail the current
slice. After that slice is implemented, tested and reviewed, return to plan the
next one against what was actually built.

## Where this sits

After deciding what to build, before implementation. `/charter` can supply a
project's architecture decisions; `/docket` turns a feature idea into a plan;
`/stint` or your preferred build workflow can implement it when requested.
Docket handles its own discovery. No companion skill is required.

The map, decisions and uncertainty model are inspired by Matt Pocock's
**wayfinder**. The planning-only reference supplied for this revision was a
modified Wayfinder, rather than its canonical version. Docket keeps that
planning boundary while supporting a roadmap with separate slice plans.

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

**Plan one slice at a time.** The roadmap gives the overall sequence. The
current slice plan gives the concrete behavior, implementation work and
acceptance checks. Later slices stay outlines until implementation, tests and
review of the current slice supply evidence for the next plan. A failed check
stays recorded as a gap in the current slice.

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

The agent reads project context, resolves consequential choices and writes the
roadmap and current slice plan. A clear request can go straight to planning.
Unless you choose paths or the project has a convention, a small saved-filters
feature can stay in `planning/saved-filters-roadmap.md`. Docket returns the
locations and stops before implementation.

For a larger feature, the default layout separates the overview from detail:

```text
planning/
  path-authoring-roadmap.md              # all slices, shared decisions
  path-authoring-1-edit-points-plan.md    # current slice's detailed work/checks
```

The roadmap might list edit points, draw paths and convert shapes. Only edit
points gets a detailed plan initially. Request its implementation separately
using your build workflow. After its checks pass and review is complete,
return to Docket; it reconciles the evidence and creates the second slice's
plan. Future plan files are created when their planning begins.

**Resume a roadmap:**

```
/docket continue planning/saved-filters-roadmap.md
```

It reads the roadmap, current plan and new evidence, preserves settled
decisions, and updates affected plans and checks together. If the current
slice has only been planned, it points to implementation and stops. If checks
are unmet, it records the gap before further planning. You can also ask it to
finish early; the result is a Draft with unresolved choices visible.

## FAQ

### Are the roadmap and plan the same thing?

The roadmap says which usable outcomes come next and why. A slice plan says
exactly how to build and check the current outcome. Small features can combine
both in one file. Larger features normally use one roadmap and linked slice
plans, detailing one slice at a time.

### Do I still need brainstorming?

Docket now handles discovery and planning itself. Use brainstorming separately
if you want its design process; Docket no longer requires a handoff to it.

### What happens to existing dockets and specs?

They keep their paths, decisions, question IDs and source links. Approved spec
detail can stay where it is, with a short index in the planning document.
Recorded implementation history is retained. Resume planning in place, using
the current slice's implementation, tests and review to decide what comes
next. A new spec format or companion skill is not required.

### How is this different from the supplied Wayfinder reference?

It keeps the `docket` name and explicit invocation, and adds compatibility with
existing dockets, outcome-based scope checkpoints and decision references.
It supports separate slice plans and planning the next slice from completed
implementation evidence. It shares the reference's planning-only stopping point.

## What it deliberately is not

A ticket generator, implementation runner, or code reviewer. A ready plan
means the next phase is actionable; implementing it requires a separate request.

## License

[MIT](LICENSE)

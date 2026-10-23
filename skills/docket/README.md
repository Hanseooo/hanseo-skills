# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Explore a feature and create one evolving roadmap. Reference it while discussing
each slice, then invoke your spec skill separately. Later sessions keep the
roadmap current through instructions carried in the document.

## Where this sits

After deciding what to build, before detailed slice design. `/charter` can supply
architecture decisions; `/docket` maps the feature's outcomes and dependencies;
grilling or brainstorming develops a slice; your user-invoked spec skill
publishes its spec to your configured tracker. `/stint` or your preferred build
workflow can implement that spec when requested.

The evolving map, decision index and uncertainty model are inspired by Matt
Pocock's **wayfinder**, but docket covers a later stage: wayfinder finds what to
build and stops; docket stays live while it is built. See the FAQ for when to
use which. Docket requires no companion skill to write its roadmap.

## The problem

Detailed plans for every slice can repeat decisions and become stale as the
first slice is built. Requiring a return to the planning skill adds another
step, while treating unfinished implementation as a reason to stop all later
discussion blocks useful discovery.

A source link alone also leaves the next agent guessing who should update the
roadmap after implementation and review.

## How docket solves it

**Keep the roadmap small.** It holds outcomes, dependencies, shared decisions,
uncertainty and evidence. Each linked spec owns its detailed behavior and
acceptance checks. New slices start as outlines, including the first.

**One slice, one spec.** A slice too big for one spec becomes several slices
on the roadmap before any spec is written, so each slice has one clear point
of completion.

**Cross-slice rules and journeys live once.** Rules that bind several slices
get `RS-` IDs in the roadmap; each spec cites them and numbers its own rules
`SH-`, so the two never collide. Each journey names the slice that completes
it, and that slice's spec runs it at closeout.

**Let the route evolve.** Newly resolved uncertainty can reveal needed work and
change provisional slices within the agreed outcome. Agents record why the
route changed and ask before changing product scope or confirmed decisions.

**Carry the update responsibility.** The roadmap opens with instructions for
later discussion, spec and review sessions. Specs link back and carry a short
update instruction into build handoffs. Completion follows acceptance and review
evidence; unfinished dependencies stay visible while discussion continues.

## When not to use it

**One small design question or an already clear single slice.** Ask directly or
use your preferred discovery and spec workflow; a roadmap may add no value.

**You already have an actionable spec and want it built.** Request execution
with `/stint` or your usual build workflow.

**You need project-wide architecture and agent conventions.** Use `/charter`.

**You can't yet say what the feature should be.** Settle that first with
`/han-grill`, or wayfinder when the decisions need many sessions.

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

**Map a feature:**

```
/docket map editing imported paths, drawing new paths and converting shapes
```

The agent investigates relevant context, settles the outcome and shared
constraints, and writes `planning/path-authoring-roadmap.md` unless you choose
another path. It outlines the known slices, dependencies and unresolved areas,
names the next discussion and stops.

**Use the roadmap through the slice workflow:**

1. Reference `planning/path-authoring-roadmap.md` during grilling or brainstorming
   about the edit-points slice. That session updates decisions and uncertainty,
   and splits the slice first if it needs more than one spec.
2. When the discussion is settled, invoke your spec skill to publish that
   slice's spec to your configured tracker. The roadmap records the returned
   issue link; the spec cites roadmap rules, lists the journeys it completes
   and carries a pointer back for maintenance.
3. Request implementation and review against the spec. The session updates the
   roadmap from acceptance and review evidence when it has the handoff context;
   `/han-implement-spec` does this whenever the spec links a roadmap.
4. Reference the same roadmap when discussing the next slice. Reconcile stale
   status first, then discover that slice against what is now known.

Docket creates one roadmap, with no separate slice-plan files. You invoke the
spec skill yourself. You do not need another Docket invocation between slices.

## FAQ

### Does the roadmap update itself?

Updates happen in the active session reading the roadmap or its handoff pointer.
There is no background hook. `/han-implement-spec` records its evidence in the
roadmap when the spec links one. If a build or review happened without that
context, the next session that references the roadmap catches it up.

### What if a slice is too big for one spec?

Split it into slices on the roadmap before writing any spec, and record why.
A slice with several specs has no single point where it is complete. If the
ticket breakdown is what reveals the size, split the slice and revise its spec
into one spec per new slice; stable criterion IDs keep the traceability.

A slice whose rules and journeys apply only to itself is really a separate
feature: give it its own roadmap instead.

### Can I discuss slice 2 before slice 1 passes review?

Yes. Slice 1 remains incomplete, with its failed checks or missing evidence
visible. Dependencies can prevent implementation readiness without stopping
discussion. Passing checks, completed review and explicit acceptance of any
remaining findings establish completion.

### What happens when we discover more uncertainty?

Record a question or broader unclear area first. When evidence reveals required
work, revise provisional slices within the agreed outcome and record why.
Technical prerequisites belong inside the user outcome they enable. Optional
extensions stay separate until you choose to expand scope.

### What happens to existing dockets, slice plans and specs?

Keep their paths, identifiers, rationale and history. The roadmap can index
existing approved detail without copying it. An explicit Docket resumption can
replace obsolete carried instructions in place; it creates no new slice plans.

### How is this different from wayfinder?

They cover different stages of the same effort.

| | Wayfinder | Docket |
|---|---|---|
| Artifact | A map issue plus child tickets on your tracker | One Markdown roadmap |
| Unit | A decision: research, prototype, grilling or task | A slice: a user outcome built end to end |
| Fog clears by | Deciding | Deciding, then building and feeding results back |
| Ends | When the way is clear, before any build | When the last slice is accepted |
| Pace | One ticket per session (research excepted) | Any number of questions per discussion |
| Needs | Grilling, domain-modeling, research and prototype skills, plus tracker setup | Nothing |

Use wayfinder first when you cannot yet say what the feature should be without
many sessions of research or prototypes. Its decisions or spec then become
docket's input. When the destination is already clear, start with docket.

## What it deliberately is not

A spec publisher, ticket generator, implementation runner, code reviewer or
background automation service. Those steps need their own user requests.

## License

[MIT](LICENSE)

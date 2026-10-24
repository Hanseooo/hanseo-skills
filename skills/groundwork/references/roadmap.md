# Project roadmap

The route from here to the brief's "done", and the only place status lives.
Default path: `planning/roadmap.md`, beside docket's feature roadmaps.

## Shape

```markdown
# Project roadmap

Destination: [brief](../docs/brief.md). <one line>
Current: M<n> — <what is in progress now>

## M0 — Project foundations
Status: In progress
Sessions:
- [ ] <Session name> — <commands to type> — <one-line kickoff, or "see below">
  Needs: <docs or decisions it reads>. Writes: <doc>. Done: <link once written>

## M1 — <usable outcome>
Status: Not started | In discussion | In progress | Complete
Exit criteria: <observable checks>
Depends on: M0
Project rules: P-01, P-03
Out of scope: <exclusions>
Breakdown: <link to its docket, or its single spec>
Evidence: <link once complete>

## Changes
- <date>: <what moved and why>
```

## Milestones

- A milestone is a usable outcome, never a layer ("backend done"). Technical
  prerequisites sit inside the outcome they enable.
- Each entry is the destination a docket starts from: outcome, exit criteria,
  dependencies, rules in scope and exclusions. Its detail lives in that docket
  or spec once one exists.
- A milestone small enough for one spec links the spec directly. One that would
  need several dockets splits into several milestones.
- New milestones start as outlines, the first included. Revise them as
  evidence arrives and record why under Changes; ask before changing the
  brief's scope.
- IDs: `M` for milestones and `P` for project rules (in the brief). Decisions
  are ADRs. Dockets keep their own D, RS and J IDs inside their files.
- Each level holds status for its own children only: the roadmap tracks
  milestones, dockets track slices, the tracker tracks tickets. `Current:`
  moves when the next milestone starts.

## M0 kickoff prompts

Each M0 session starts in a fresh session, so its kickoff carries what that
session needs:

- what to read first (the brief, this roadmap, relevant existing docs)
- what to settle, as topics, never pre-made answers
- which doc it writes, and where
- what to update afterwards: this M0 entry, and the `AGENTS.md` routing row for
  the new doc

M0 is complete when every session is done or the user drops it; the roadmap
keeps it as history.

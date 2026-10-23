# han-implement-spec

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Implement a spec's whole ticket graph on one integration branch, with cost
that follows the graph: agents, worktrees, strong models and full test runs
only where concurrency, difficulty or reach call for them.

## Where this sits

Use after `/han-to-tickets` or any tracker of spec plus tickets with blocking
edges. It uses `/tdd` and `/code-review` when installed and falls back to a
short built-in version of each. It hands off a branch, or a PR when your
tracker closes work that way. When the spec links a `/docket` roadmap, the
report's evidence is also recorded under that slice.

Adapted from Matt Pocock's
[`implement-spec`](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement-spec/SKILL.md),
inspected 2026-10-23 from the installed copy. It keeps the integration branch,
task-graph frontier, context pointers and tracker closing. The final review's
two-pass cap comes from `/stint`.

## The problem

Running Matt's version on a strict ticket chain (07 → 08 → 09) used most of a
usage quota and finished one ticket. The session retro found:

- Five subagents on the strongest model, including a merger agent for a
  conflict-free merge and two reviewers that found one placement issue.
- Whole handoff documents read at once, about 20k tokens and truncated.
- Dozens of wake-and-report cycles while an 8-minute browser suite ran.
- Three existing tests asserting the old behavior, found only by the full
  browser run, which then ran a second time.

In baseline trials with the upstream skill, agents still gave every ticket of a
strict chain its own worktree and a merger agent, never looked for existing
tests the change would break, and ran the full browser suite two or three times.

## How han-implement-spec solves it

**A fresh agent per ticket, worktrees only for parallel work.** Like Matt's
version, every ticket gets its own implementer agent with a clean context, and
the main session only orchestrates. A chain runs one agent at a time directly on
the integration branch: no worktree, no merge. Independent tickets run in
parallel worktrees, each removed once merged. Clean merges stay in the main
session; only a conflict spawns a cheap merger agent. Additive edits to a shared
file still count as independent.

**Tiers, not model names.** Mechanical work goes to the cheapest model that uses
tools reliably, High-complexity tickets and reviews to the strongest. The
ticket's Complexity field picks the tier, so the rule works in any harness with
model choice and degrades to the current model without it.

**Find conflicts before the expensive run.** Each ticket starts with an impact
sweep for tests asserting behavior it changes, then runs those targeted checks.
The full suite runs per ticket only for Shared or System-wide reach, otherwise
once per batch. Long commands log to a file and wait on completion instead of
polling.

## Differences from Matt's

| | Matt's `implement-spec` | `han-implement-spec` |
|---|---|---|
| Worktrees | Every ticket | Parallel tickets only, each removed right after its merge. A chain commits straight to the integration branch. |
| Merging | A merger agent for every result | The main session merges cleanly. A cheap merger agent handles conflicts only. |
| Overlap | Left to the merger | Tickets changing the same existing behavior run in sequence. Additive edits to a shared file stay parallel. |
| Models | Not specified | Tiers (economy, standard, strong) picked from the ticket's Complexity field. |
| Before coding | Straight to `/tdd` | Impact sweep for existing tests the ticket breaks. |
| Test runs | Not specified | Targeted per ticket. Full suite once for Shared tickets and once at the batch gate. |
| Waiting | Not specified | Log to a file and wait on completion. No polling. |
| Reviews | One final `/code-review`, all findings fixed | Gate review on risky upstream tickets. Final review capped at two passes. |
| Exploration | Optional exploration agent | Dropped. Ticket briefings carry the entry points. |
| Report | Branch or PR | Adds mode per frontier, check results, open findings, changed test expectations and roadmap evidence. |
| No subagents | Not handled | The main session builds each ticket in graph order. |

Shared with Matt's: the integration branch, task-graph frontier, context pointers, one fresh agent per ticket, `/tdd`, and a final `/code-review`.

## When not to use it

- **One ticket, or a brief with no ticket graph:** use `/stint` or `/implement`.
- **Tickets lack blocking edges or classifications:** run `/han-to-tickets`
  first, or expect the skill to judge tiers and modes from thinner evidence.
- **You want Matt's always-parallel design unchanged:** use `/implement-spec`.

## Install

Install through the [skills.sh CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add Hanseooo/hanseo-skills --skill han-implement-spec
```

Or browse the collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-implement-spec/` as the skill root in your
  skills directory.
- **Codex and other agents:** load `skills/han-implement-spec/SKILL.md` through
  the tool's skill mechanism. The bundled Codex policy keeps invocation explicit.

Update the clone with `git pull`, or re-run `npx skills add`.

Optional companions: `/tdd`, `/code-review` and `/resolving-merge-conflicts`.
Without them the skill follows its built-in fallbacks.

## Use

```text
/han-implement-spec .scratch/menus/spec.md
/han-implement-spec https://github.com/example/editor/issues/40
```

In Codex, invoke `$han-implement-spec`. The agent states the mode for each
frontier, builds and checks each ticket, reviews risky upstream tickets before
their dependents start, runs one batch gate and at most two final review passes,
then reports the branch, check results, open findings and every existing test
expectation it changed.

## FAQ

### How is this different from `/stint`?

`/stint` is one sitting of build work on the current branch with no agents.
`han-implement-spec` owns a whole spec across sittings: integration branch,
frontier, optional parallel agents and tracker closing.

### Does it need a particular model or harness?

No. It names tiers and roles, never models. Where the harness offers no model
choice or no subagents, everything runs in the main session on the current
model.

## What it deliberately is not

A ticket writer, a planner, a CI replacement, or a promise of parallelism on a
chain that has none.

## License

[MIT](LICENSE). Includes upstream notices for Matt Pocock.

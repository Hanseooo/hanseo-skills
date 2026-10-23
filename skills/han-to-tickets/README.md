# han-to-tickets

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Turn an agreed spec or conversation into reviewable tickets a fresh developer
or agent can pick up. Publish the approved breakdown to your configured tracker.

## Where this sits

Use after design decisions are agreed and before implementation. It accepts
`/han-to-spec` output or another sufficient source. No Docket or companion
skill is required. It needs your existing tracker configuration and triage
vocabulary, plus the permissions/tools needed to publish there.

Adapted from Matt Pocock's
[`to-tickets`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md).
Upstream reference inspected 2026-10-20 on `main`. This adaptation does not
track future upstream changes.

## The problem

An apparently mechanical rename can affect hundreds of consumers. A vague
ticket can also leave interaction, recovery or persistence work for another
session. Baseline trials produced both incomplete slices and a proposal to
publish a breakdown that had not been reviewed.

## How han-to-tickets solves it

Complete vertical slices carry the applicable source criteria and shared UX
rules. Explicit blockers and a source coverage map make handoffs reviewable.
Wide refactors retain the upstream compatible migration exception.

Scope, complexity and blast radius are separate body fields with reasons.
They describe the work without adding tracker labels or a planning field.
Breakdown review precedes publication to the existing tracker.

Each body is also a briefing for the agent that implements it: context links,
three to eight entry points as file plus symbol, the existing behavior and
tests the ticket overturns on purpose, traps found in the code, and the
targeted check command. The implementer starts from those pointers instead of
re-exploring, and finds conflicting tests before the full suite does. Upstream
avoids file paths because they go stale; symbol names without line numbers
stay valid through earlier tickets' edits.

## Differences from Matt's

- **Implementer briefing in every body.** Context links, three to eight entry points as file plus symbol, existing behavior and tests the ticket overturns on purpose, traps, and a targeted check command. Matt's version avoids file paths because they go stale. This one keeps symbol names and drops line numbers.
- **Scope, complexity and blast radius** are separate body fields with reasons, not tracker labels. `/han-implement-spec` reads Complexity to pick a model tier.
- **Source coverage map** and explicit blockers make handoffs reviewable.
- **Breakdown is reviewed before publishing.**
- **Kept from Matt's:** vertical slices and the compatible-migration exception for wide refactors.

## When not to use it

- **Product behavior is undecided:** discuss it or use `/han-grill` first.
- **An existing ticket is sufficient:** implement from that ticket directly.
- **You want the upstream format without added assessments:** use `/to-tickets`.

## Install

Install this skill through the [skills.sh CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add Hanseooo/hanseo-skills --skill han-to-tickets
```

Or browse the whole collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-to-tickets/` as the skill root in your skills directory.
- **Codex and other agents:** load `skills/han-to-tickets/SKILL.md` through the
  tool's skill mechanism. Codex's bundled policy keeps invocation explicit.

Update the clone with `git pull`, or re-run `npx skills add`.

## Use

```text
/han-to-tickets docs/specs/appearance-spec.md
/han-to-tickets https://github.com/example/editor/issues/80
```

In Codex, invoke `$han-to-tickets`. Review the proposed breakdown and blocking
edges, then approve publication. You receive actual issue links, or individual
Markdown ticket paths when local Markdown is your configured tracker.

## FAQ

### Does this replace the original tracker workflow?

It preserves configured tracker publication, parent links, genuine blockers
and existing triage labels. The additions are body assessments and explicit
source coverage. Reading the source and reviewing those assessments costs
more context than a minimal ticket list.

## What it deliberately is not

A new tracker, planning gate, implementation agent or automatic parallel scheduler.

## License

[MIT](LICENSE). Includes upstream notices for Matt Pocock.

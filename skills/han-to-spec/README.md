# han-to-spec

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Turn an agreed discussion into a spec with readable user
outcomes and individually addressable acceptance criteria. Keep the UX details
that a later implementation session needs. Use the configured issue tracker,
or the project's usual spec folder when no tracker is configured.

## Where this sits

Use after a design discussion, including `/han-grill`, and before
`/han-to-tickets` or your usual implementation workflow. Each skill works
independently. It uses existing tracker configuration when present and works
locally without it. No Docket or companion skill is required.

Adapted from Matt Pocock's
[`to-spec`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md),
with design-contract ideas from Superpowers
[`brainstorming`](https://github.com/obra/superpowers/blob/v6.4.2/skills/brainstorming/SKILL.md).
Upstream references inspected 2026-10-20: Matt's `main` and installed
Superpowers 6.4.2. This adaptation does not track future upstream changes.

## The problem

A story for every control makes a spec repetitive. Condensing that list can
then erase the exact focus, cancellation, disabled-state or recovery behavior
that made the original useful. A fresh agent also needs precise references
when a single outcome spans several tickets.

## How han-to-spec solves it

Outcome stories keep related behavior together. Stable criterion identifiers
keep small requirements visible and referenceable. Shared rules have one home,
and a source coverage index makes omissions reviewable.

Design rationale, existing behavior and unresolved decisions stay visible.
The spec distinguishes required verification from evidence that it passed.

## When not to use it

- **The design is still undecided:** discuss it directly or use `/han-grill`.
- **A tiny change already has a sufficient brief:** use that brief directly.

## Install

Install this skill through the [skills.sh CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add Hanseooo/hanseo-skills --skill han-to-spec
```

Or browse the whole collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-to-spec/` as the skill root in your skills directory.
- **Codex and other agents:** load `skills/han-to-spec/SKILL.md` through the
  tool's skill mechanism. Codex's bundled policy keeps invocation explicit.

Update the clone with `git pull`, or re-run `npx skills add`.

## Use

```text
/han-to-spec
/han-to-spec docs/specs/appearance-spec.md
```

In Codex, invoke `$han-to-spec`. It uses the discussion and project evidence,
confirms testing seams only when needed, and publishes to the configured issue
tracker. Without one, it saves Markdown in the project's usual spec folder,
falling back to `docs/specs/` when there is no established convention. An explicit
destination overrides this default. Missing decisions leave a Draft with blockers.

## FAQ

### How is this different from `/to-spec`?

It groups stories by outcome, adding stable criteria, explicit shared UX
contracts, rationale and a coverage index. It uses the configured tracker with
a local fallback. Choose the upstream skill when you prefer its extensive
story list and original workflow.

### What does the coverage check cost?

An additional comparison against the source discussion and a small reference
index. Longer source discussions still require reading their full detail.

## What it deliberately is not

A new interview, implementation plan, implementation-ticket breakdown or proof
that tests pass.

## License

[MIT](LICENSE). Includes upstream notices for Matt Pocock and Jesse Vincent.

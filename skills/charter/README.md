# charter

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Every new agent session starts from zero. It doesn't know you already decided
to wrap the email vendor, why that folder exists, or that billing was declared
off-limits three sessions ago. So it guesses — and each guess drifts a little
further from the last one.

Charter writes the decision down once, so every future session inherits it
instead of re-deriving it.

## The problem

**Before charter:** session 1 wraps the email vendor behind an adapter "for
flexibility." Session 4 calls the vendor SDK directly from a new handler,
because nothing told it not to. Session 9 proposes migrating vendors, unaware
someone already decided this stays put. Three sessions, three contradictory
architectures, no record of why any of it happened.

**After charter:** one ADR says the vendor is wrapped, why, and what to do if
that stops being true — *write a new ADR and flag a human, don't implement the
change first.* Every agent reads it before touching that seam. Disagreement
becomes a new ADR, not silent drift.

## What it produces

A markdown-only, agent-agnostic **decision layer** — the artifacts that make
future agent sessions build maintainably instead of reaching for bandaid
fixes:

1. `architecture.md` — system shape, module boundaries, seams, folder map
2. ADRs — locked decisions, each with a named escape hatch
3. A per-project `CLAUDE.md` (or `AGENTS.md`) operating contract referencing both

No code, no runtime, no vendor lock-in — just `SKILL.md` plus reference
markdown files that any agent capable of reading files and following
instructions can use. It creates **no folders and no code**: the folder map is
a section inside architecture.md, and structure materializes when the first
module is built.

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

- **Claude Code:** clone into `~/.claude/skills/charter` (use `skills/charter/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/charter/SKILL.md` per that tool's own custom-instructions/skill mechanism, or just
  paste `SKILL.md`'s contents into the session when you want to charter a
  project.

Update later with `git pull` (or re-run `npx skills add`).

## Use

In any project, ask your agent to "charter this project" (or "set up ADRs",
"lock in our decisions", "make X swappable"). Three routes:

- **Greenfield** — no code yet: doc inventory → a short interview scaled to how
  serious the project is (prototype / product / platform) → 2–3 proposed
  architecture shapes → the three artifacts.
- **Early code** — some code exists: the skill first surfaces the decisions
  already embedded in your code as draft retroactive ADRs for you to confirm.
- **Single seam** — mid-project "make X swappable": one seam design, one ADR,
  no interview.

## Worked example

Given a half-page PRD for a recipe-sharing app and three answers (real product;
the email provider will be swapped; agents must never touch billing), charter
produces:

- `architecture.md` with an email-provider seam justified by the stated
  volatility — and a "deliberately not abstracted" list for everything else
- ADRs for the stack and the seam, each ending in an escape hatch: *to change
  this, write a new ADR and flag a human — do not implement first*
- a one-page `CLAUDE.md` (mirrorable as `AGENTS.md`) whose billing guardrail
  traces to its ADR

## What it deliberately is not

Not an audit tool, not a work prioritizer, not a feature designer, not a
scaffolder. It writes down decisions; other tools act on them.

## License

[MIT](LICENSE)

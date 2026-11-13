# hack-init

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Sets up a hackathon repo so several people, each driving their own coding
agent, can build one demo in parallel without stepping on each other. It writes
one `AGENTS.md` that every agent reads: the demo journey as the scope, a
walking-skeleton first ticket, folder lanes, a testing and review budget sized
for a deadline, and the kickoff sequence.

## Where this sits

Run it before the event, in a repo that already has a scaffold. It adapts
`/project-init` and `/groundwork` from this collection for a time-boxed team
build. The kickoff sequence it writes then uses existing skills unchanged:
`/han-grill` picks the problem, `/han-to-spec` writes the spec around the demo
journey, `/han-to-tickets` cuts the tickets, and each person builds with
`/stint`, which calls `/tdd` and `/code-review` from Matt Pocock's collection.

The issue-tracker config (`docs/agents/issue-tracker.md`, GitHub or local
Markdown) is left to Matt's `/setup-matt-pocock-skills`, which the ticket and
spec skills already read. Without it, `hack-init` still writes `AGENTS.md` and
tells you to run that setup first.

## The problem

Ask an agent to make a fresh repo "agent friendly so we don't step on each
other" before a hackathon and it writes a good long-term contract. That is
the wrong contract for this job. In baseline runs without this skill, every
agent:

- left the testing rule out, or made it "every behavior change has a test that
  failed first", which is TDD everywhere on a 19-hour clock
- coordinated parallel work only by "post in team chat before touching shared
  files", with no first ticket that writes the shared files before anyone else
  starts
- said "add no dependency until the stack is chosen" and gave no rule for
  choosing it, so the choice stalls at kickoff
- left `/stint`'s two review passes on every ticket
- set no code freeze and no demo scope, so nothing stopped work drifting off
  the path judges see
- invented a format for the tracker config instead of using the setup skill
  the ticket skills read

## How hack-init solves it

**The demo journey is the scope.** The spec's acceptance journey is the two
minutes a judge sees. Edge cases off it are listed as excluded, UI states are
required on its screens, and one Playwright smoke test walks it in CI before
every merge. Each ticket adds its screen's step to that test. At the code
freeze, someone rehearses the demo by hand on the deployed app.

**The walking skeleton comes first.** Ticket 01 deploys the app, makes the
demo screens reachable on mock data, writes the contract files (shared
types, schema, mock data) and adds a CI check that runs typecheck, lint, unit
tests and the smoke test on every PR. Every other ticket waits on it and then owns one
folder, so parallel branches rarely touch the same file.

**Testing and review go where a bug spreads.** `/tdd` runs on parsers, data
transforms and branching logic, where tests are cheap and bugs hide. Every
ticket gets one `/code-review` pass before its branch is pushed. Tickets whose
blast radius reaches other people's lanes get `/stint`'s second pass too.

## When not to use it

- **A project meant to outlive the event.** Use `/groundwork`, then
  `/project-init`. This contract trades maintainability for speed.
- **A solo hackathon.** Lanes, claims and the skeleton ticket exist for
  parallel builders. A single `AGENTS.md` with the testing rule is enough.
- **An empty repo.** Scaffold first. The commands come only from the manifest,
  so an empty repo yields a contract with every command blank.

## Install

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents). For a team, install into the event repo for every agent the
team uses:

```
npx skills add Hanseooo/hanseo-skills --skill hack-init --skill han-grill --skill han-to-spec --skill han-to-tickets --skill stint -a claude-code -a codex -a antigravity
npx skills add mattpocock/skills --skill tdd --skill code-review --skill setup-matt-pocock-skills -a claude-code -a codex -a antigravity
```

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/hack-init` (use
  `skills/hack-init/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/hack-init/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

Without `/tdd` and `/code-review`, `/stint` has no testing or review step.
Without `/setup-matt-pocock-skills`, the ticket skills have no tracker config.

## Use

```
/setup-matt-pocock-skills
/hack-init DS3, 2026-11-15 08:00 to 2026-11-16 03:00, team of 3
```

It reads the repo, asks once for whatever the repo cannot show (event times,
team size), shows the draft `AGENTS.md` and a one-line `CLAUDE.md` that
imports it, and writes them on your confirmation. The problem and backend stay
`Pending` until kickoff, where `/han-grill` records them.

## What it deliberately is not

- Not a scaffolder. It writes no app code, no test runner and no config.
- Not problem selection. Pick the problem with `/han-grill` at kickoff.
- Not a tracker setup. That is `/setup-matt-pocock-skills`.

## License

[MIT](LICENSE)

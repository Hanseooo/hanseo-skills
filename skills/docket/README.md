# docket

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For designing one **large** feature — messaging, billing, auth — across several
short, focused sessions instead of one long one.

## Where this sits

If you use the `superpowers` collection, you have **brainstorming** — it asks
you questions, then writes the design down as a spec file.

docket doesn't replace it. It **schedules** brainstorming sessions. It works out
how many sessions your feature needs, what each one covers, what each one must
stay away from, and what order to run them in. Then each session is a normal
brainstorming run — just a much smaller one.

Optional: if you also have **grill-with-docs** or **grill-me** (from Matt
Pocock's collection), docket can hand off to them for optional stress-testing
after a session already has a draft spec.

## The problem

Ask an agent to design "messaging" in one go and you get something that reads
like a finished design and isn't. Edge cases go unnamed. "Show an error" stands
in for a real decision about what the user sees. Architecture choices get made by
accident, by whatever got written first. The gaps are hard to spot because the
writing is confident.

The fix is obvious: split it up. Do a session on how messages are stored, another
on how they're delivered, another on what it all looks like. Each session is
small enough to actually think about.

That fix has its own failure. Session 4 quietly contradicts session 1. You
decided in the UI session that a message appears instantly and reconciles later —
but the sync model you picked two sessions after that can't support it. Nothing
catches it, because each session ended in its own file and nobody re-reads the
old ones. You find out when you try to build it.

## How docket solves it

It writes a **docket**: one file that lists every session the feature needs, in
order, and stays in your repo after the conversation ends.

Three things make it work:

- **Each session gets a scope and an anti-scope.** Not just "S2 covers delivery"
  but "S2 does not cover how any of it looks." Saying what a session must refuse
  is what stops it drifting back to full-feature size.
- **Decided things get written down as short statements** — "ordering is
  server-assigned; client clocks are never authoritative." These are called
  **binding constraints**. Every later session reads them and treats them as
  settled. You confirm each one before it's written in, so nothing gets recorded
  that you didn't agree to.
- **A later session can't overturn an earlier one on its own.** It can split,
  merge, or drop sessions that haven't run yet. To reopen something already
  settled, it has to add a new session — which means you're in the room for it.

Sessions are split by which questions **depend on each other**. Questions that
have to be answered together go in one session; questions that don't interact go
in different ones. That's also where the ordering comes from: a session needing
another session's answer runs after it.

Every session stops at a written design. Nothing gets built until the whole
docket is done — so when session 4 changes your mind about session 1, you're
editing a document, not migrating a database.

## When not to use it

Small and medium features. Run brainstorming once and get on with it. docket
checks this itself: if the feature only splits into two sessions, it tells you to
skip the docket and won't write one.

It also doesn't design anything. It has no opinion about your messaging feature —
only about where the seams between sessions go.

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

- **Claude Code:** clone into `~/.claude/skills/docket` (use `skills/docket/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/docket/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or just paste `SKILL.md`'s contents into the session when you want
  to plan a large feature.

Update later with `git pull` (or re-run `npx skills add`).

## Use

**Once, at the start.** Ask your agent to "plan the design sessions for
messaging" (or "this is too big to brainstorm in one go"). It reads your code,
lays out the open questions, proposes two or three ways to cut them into
sessions, and writes the docket to `docs/dockets/messaging-docket.md` once you
pick one.

**Then once per session.** Open a fresh conversation and point it at the docket.
It picks the next session that's ready, loads that session's scope and the
constraints settled so far, and hands off to brainstorming or grilling. At the
end it marks the session done, links the spec that session produced, and asks you
to confirm the constraints to carry forward.

Repeat until every session is done. Then plan and build against a design that
agrees with itself.

## What it deliberately is not

Not a feature designer, not an implementation planner, not a scaffolder. It
decides where the sessions are; the sessions decide everything else.

## License

[MIT](LICENSE)

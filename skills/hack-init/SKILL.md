---
name: hack-init
description: Set up a repo's agent docs for a time-boxed hackathon team build, so several people's agents can work in parallel toward one demo.
disable-model-invocation: true
---

# hack-init

Write the `AGENTS.md` a hackathon team's agents share. The repo serves one
**demo journey**, built by several people in parallel, before a deadline.
Speed and a working demo outrank long-term maintainability; the template below
encodes that trade.

## Steps

1. **Read the repo.** Root listing, manifest scripts and lockfile, Playwright
   or other test config, `AGENTS.md`, `CLAUDE.md`, `docs/agents/issue-tracker.md`,
   `git remote -v`. Done when every `<slot>` in the template has a value from
   evidence, an answer to collect in step 2, or a placeholder this file names.

2. **Ask once.** One numbered round, only what the repo cannot show: event
   start and end times, team size, anything else a slot still needs. The code
   freeze is three hours before the end unless the user gives another time.
   No way to ask → fill the slot with a labelled assumption and list it in the
   report.

3. **Check the tracker config.** `docs/agents/issue-tracker.md` is owned by
   `/setup-matt-pocock-skills`, which `/han-to-tickets` and `/han-to-spec`
   read. Present → read it to choose the GitHub or local lane line in the
   template. Missing → leave that file alone, keep both lane lines, and put
   "run `/setup-matt-pocock-skills` (GitHub when the remote is GitHub)" first
   in the report.

4. **Draft, confirm, write.**
   - `AGENTS.md` from the template. The package manager comes from the
     lockfile, else the manifest's `packageManager` field, else npm as a
     labelled assumption. Install is its install command. Every other command
     is a manifest script run with it. A missing script reads "none yet:
     ticket 01 adds it". An existing `AGENTS.md` is amended in
     place; sections the template lacks stay word for word.
   - `CLAUDE.md`: absent → write a file holding only `@AGENTS.md`. Present
     without that line → add it as the first line.

   Show every draft in one message, take one confirmation, then write.

5. **Report**: files written, slots still open, the tracker step if step 3
   flagged it, then stop. The kickoff sequence lives in the written
   `AGENTS.md`.

## Template

Copy every section. Placeholders in `<angle brackets>` are slots; the
`Pending` lines stay as written until kickoff fills them.

```markdown
# <project> — hackathon agent rules

<event>, <start> to <end>. Team of <n>, each driving their own agent.
Code freeze: <end minus 3h>. At the freeze, one person walks the demo journey
by hand on the deployed app with the demo data. After the freeze, only fixes
for what that rehearsal breaks merge.

This repo serves one **demo journey**: the steps a judge sees, defined as the
acceptance journey in the spec. A working demo outranks maintainability.

## Kickoff
1. `/han-grill`: pick the problem from the shortlist and the backend by the
   rule under Decisions. Record both there.
2. `/han-to-spec`: the demo journey is its end-to-end acceptance journey.
   Edge cases off the demo journey are listed as excluded.
3. `/han-to-tickets`: ticket 01 is the walking skeleton (Parallel work).
4. Each person claims a ticket, branches, and runs `/stint #<n>`.

## Decisions
Change a line here only after telling the team. One line each: choice, because reason.
- Problem: Pending, kickoff.
- Backend: Pending, kickoff, by this rule:
  - Supabase Postgres: default.
  - Convex: live multi-user sync is the core of the demo and a teammate has shipped with it.
  - Turso/SQLite: offline or low-connectivity use is the point.
  - Add a Python service: the core needs a Python-only library.

## Commands (use exactly)
- Install: <command>
- Dev: <command>
- Typecheck: <command>
- Lint: <command>
- Unit tests: <command>
- Demo smoke: <command>

## Parallel work
- Ticket 01, the **walking skeleton**: the app deployed, the demo journey's
  screens reachable on mock data, the contract files written, the demo smoke
  passing, and a CI check that runs typecheck, lint, unit tests and the demo
  smoke on every PR. It blocks every other ticket.
- **Contract files**: shared types, schema, mock data, root layout,
  `package.json` and lockfile, env names. Paths: <listed by ticket 01>.
- Every later ticket owns one folder. <GitHub: label `lane:<folder>` |
  Local: a `Lane: <folder>` line>. Edit outside your folder only in contract
  files, after telling the team.
- Claim before starting: <GitHub: assign yourself | Local: set `Status: claimed`>.
- One branch per ticket. PR body says `Closes #<n>`. Self-merge when CI
  passes. `main` stays demoable.

## Testing
- `/tdd` at these seams: parsers, data transforms, branching domain logic.
- Demo smoke: one Playwright test walking the demo journey.
- A ticket that changes a demo-journey screen adds that screen's step to the
  demo smoke, which covers its UI components and glue code.

## Review
- Every ticket: one `/code-review` pass after `/stint`'s commit step, before
  the branch is pushed.
- Shared or System-wide blast radius, as the ticket's Blast radius field says:
  `/stint`'s second `/code-review` pass too, over the first pass's fixes,
  also before the push.

## Code
- Smallest thing that works: does it need to exist → platform or stdlib →
  installed dependency → minimum code.
- New dependency: allowed when it saves time; tell the team before installing.
- Surgical changes in the existing style. Reuse before writing.
- Demo-journey screens carry loading, empty, error and disabled states and a
  way back.
- Done means it works in the deployed app.
- Secrets: read only `.env.example`; keep keys out of code, logs and chat.
```

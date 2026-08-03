# stint

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For building from a written spec in **sittings** — one fixed point, one review
budget, one report per run — with the size of a sitting set by how much codebase
the run has actually read, not by a ticket count.

## Where this sits

This is the build step at the end of a spec-first chain. In Matt Pocock's
collection that chain is grill → **to-spec** → **triage** (which writes agent
briefs) → build → **code-review**. `stint` is a reworking of his `implement`
skill, and it keeps all five of his instructions intact — what it adds is
bounds around them. See the [FAQ](#faq) for the specifics and for when his is
the better pick.

It writes no tests and finds no bugs of its own. `/tdd` does the first,
`/code-review` does the second — both external to this collection, both from
Matt Pocock's. Without them you still get the sizing, the pinned fixed point,
and the review budget, but you supply the testing and reviewing yourself.

What this skill owns is the three decisions those two don't make: **how much
work one sitting takes**, **what fixed point review is measured against**, and
**when reviewing stops**.

## The problem

Hand an agent a spec with several tickets and say "implement this", and it
implements all of them in one context. That is often fine — in testing, agents
that batched three tickets produced good code, and one found a rounding edge
case the spec had missed. Batching is not the failure. Batching *unboundedly*
is: the limit nobody sets is how much unfamiliar code the run has to hold, and
past that point the agent is reasoning about a module it skimmed 40k tokens ago.

Three smaller failures ride along, and these were observed in every baseline run:

- **The fixed point is gone.** `/code-review` compares `HEAD` against a fixed
  point and has to ask for one. By the time anyone asks, the run has made
  commits, and where the work *started* is a guess.
- **Nobody re-reads the fixes.** Review reports, the agent fixes, the agent
  commits. The fixes — the newest and least-examined code in the change — are
  the one part no review has ever seen.
- **It commits to `main` silently.** Not through carelessness: one baseline run
  recorded that the instruction to use the current branch had overridden its own
  rule against committing to the default branch, and committed anyway.

## How stint solves it

**A sitting is bounded by unread code, not ticket count.** Name the tickets
yourself (`/stint T1-T3`) and that is the stint. Name none and it says which
ones it is taking and why, then proceeds — so unattended runs aren't blocked. It
stops before any ticket needing a part of the codebase this run hasn't opened.
Three tickets against one module is one cheap sitting; three tickets across
three unfamiliar areas is three.

**The fixed point is captured first, before anything is touched** — a `HEAD` SHA
recorded as step one, handed to `/code-review` later. It is unambiguous at
exactly one moment, and this pins that moment.

**Review runs at most twice per stint, then stops.** Once for the whole sitting,
not once per ticket — that is where the token saving lives. Findings are
addressed, review runs again against the same SHA so it reads the fixes, and
whatever is still open after that second pass is reported to you rather than
fixed. There is no third pass: Standards findings are judgement calls by
construction, so "no findings" is a state review may never reach. A first pass
that finds nothing skips the second entirely.

**The work is committed before it is reviewed, not after.** This looks backwards
and isn't. `/code-review` diffs the fixed point against `HEAD`; review
uncommitted work and `HEAD` is still *on* the fixed point, so the diff is empty
and both axes cheerfully report no findings. Testing caught exactly this — one
run noticed and worked around it, an identical run didn't and shipped a defect
its review had reported as clean.

**It commits where you are and tells you when that's `main`.** No branch is
created. If the commit lands on the default branch, the report's first line says
so — a required line, not a courtesy, because the baseline showed a mid-process
warning is exactly what gets rationalised past.

## When not to use it

**No written brief yet.** That's design work. Use grilling, brainstorming, or
`docket` first — this skill assumes something already says what "done" means.

**Exploratory coding or debugging without a brief.** There's no unit to size a
sitting against and no spec for review's second axis to check.

**One small ticket, with you watching.** The bounds never bind and you're
loading a lot of words for nothing. Matt Pocock's `implement` is five
instructions and does the job.

**Driving a whole ticket queue unattended.** One sitting per invocation is the
whole idea. `superpowers:executing-plans` exists for sequencing.

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

- **Claude Code:** clone into `~/.claude/skills/stint` (use `skills/stint/` as
  the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/stint/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

`/tdd` and `/code-review` are external and not bundled here.

## Use

```
/stint docs/spec.md          # agent picks the sitting and says which tickets
/stint T1-T3                 # you set the sitting
/stint #42                   # an agent brief on an issue
```

It pins the fixed point, notes the branch, confirms the test seams, runs the
red→green loop, commits, reviews the whole sitting, commits any fixes, and
reviews once more. It reports the tickets covered, the SHA range, the suite
result, anything review left open, and which tickets remain.

Then open a fresh conversation for the next sitting.

## FAQ

### How is this different from Matt Pocock's `implement`?

His five instructions — implement the brief, use `/tdd`, test at the right
cadence, `/code-review` when done, commit — are all still here. Everything below
is a bound added around them, not a replacement for them.

| | `implement` | `stint` |
|---|---|---|
| How much per run | "the spec or tickets" — unbounded | bounded by unread code; you may name the range |
| Review's fixed point | unstated; `/code-review` asks later | `HEAD` SHA pinned before the first edit |
| After findings | undefined | fix → re-review once → report what's open |
| Committing to `main` | silent | required line in the report |

Two smaller additions: findings are not orders (state a reason before declining
one), and don't install a typechecker the repo doesn't have.

### When should I use his instead?

When you're in the room. One ticket, or a spec small enough to be one sitting —
the bounds never bind and his is far cheaper to load. You can see the agent
working and you'll notice if it overruns.

The short version: **his is a nudge, yours to supervise. This is a contract, for
when nobody's watching.** Supervised work only needs the nudge.

### Do I still need `/code-review`?

Yes — `stint` calls it and does none of its own reviewing. Worth knowing it
already runs its two axes as parallel sub-agents with clean context, so review
quality doesn't decay with however bloated the building run got. `stint` doesn't
improve that; it just supplies a fixed point and calls it twice.

### Why won't it create a branch for me?

Because you asked it not to. The default is to commit where you stand. What it
won't do is stay quiet about it — landing on `main` or `master` puts a required
line at the top of the report.

### What does it cost?

More than his: up to two `/code-review` invocations per sitting instead of one,
each spawning two sub-agents. That buys a review pass over the fixes. It's
bounded — never a third pass, and a clean first pass skips the second — and
reviewing once per sitting rather than once per ticket is what keeps it
affordable when a sitting covers several.

## What it deliberately is not

Not a test framework, not a reviewer, not a queue runner, not a branch manager.
It sets the size of a sitting and the shape of its two ends; `/tdd` and
`/code-review` do the work in between.

## License

[MIT](LICENSE)

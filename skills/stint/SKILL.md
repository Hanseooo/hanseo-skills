---
name: stint
description: Use when a written spec, ticket, or agent brief exists and the user asks for it to be built — "implement this", "/stint docs/spec.md", "do T2", "build T1 through T3", "pick up the next ticket". Not for exploratory coding, debugging without a brief, or design work that has no spec yet.
disable-model-invocation: true
---

# stint

A **stint** is one sitting of build work: one fixed point, one review budget, one
report.

The engineering is not this skill's job — `/tdd` writes the tests, `/code-review`
finds the problems. This skill decides how much work one sitting takes, what
review measures against, and when reviewing stops.

## Sizing the stint

**One invocation is one stint.** A stint may cover several tickets — that is
cheaper than several runs, which re-read the spec and the codebase cold each
time.

- **The user named the tickets** (`/stint T1-T3`, "do T2") → that is the stint.
  Don't second-guess the size.
- **The user named none** → say which tickets this stint covers and why, then
  proceed. Don't wait for an answer; the run may be unattended.

**Stop the stint before any ticket that needs part of the codebase this run has
not already read.** Ticket count is not the limit — unread context is. Three
tickets against one module is one cheap stint; three tickets across three
unfamiliar areas is three stints, because the third would be reasoning about
code it skimmed 40k tokens ago.

A single ticket too large to hold at once is a briefing failure, not a reason to
split mid-run. Say so and stop.

## Process

1. **Pin the fixed point.** `git rev-parse HEAD` before touching anything.
   Record the SHA. `/code-review` requires a fixed point and this is the only
   moment it is unambiguous — after the stint commits, where it started is a
   guess.

2. **Note the branch.** `git rev-parse --abbrev-ref HEAD`. **Do not create a
   branch** — the stint commits where it stands. If that branch is `main` or
   `master`, this is REQUIRED: the first line of your final report reads
   `Committed to <branch> — the default branch. No branch was created.` The line
   is required whether or not the run went well, and whether or not the user
   seems to already know.

3. **Confirm the seams**, then run `/tdd`. Seams the brief already names are
   pre-agreed — use them. If it names none, propose them and get them confirmed
   before the first test.

4. **Verify.** Single test file per red→green cycle; full suite once at the end
   of the stint. Typechecker if the repo configures one — **do not install one
   that isn't there**, that is a dependency decision and not yours to make.

5. **Commit the work, then review it.** Commit first, naming the tickets
   covered — then run `/code-review` against the SHA from step 1, once for the
   whole stint, not once per ticket.

   **Committing before reviewing is what makes the review real.**
   `/code-review` diffs `<fixed-point>...HEAD`. Run it while the work is still
   uncommitted and `HEAD` is *still sitting on the fixed point*, so that diff is
   **empty** — both axes then report no findings, and the run ends with a
   confident clean review of nothing. Never reorder these.

6. **Fix, commit the fixes, then re-review exactly once.** The fixes must be
   committed before the second pass or that pass cannot see them either. Then
   run `/code-review` again against the same SHA from step 1 — it now covers the
   work and the fixes together.

   First pass found nothing → there are no fixes, so there is nothing to
   re-review. Go straight to step 7.

   Anything still open after the second pass gets **reported, not fixed**. There
   is no third pass — Standards findings are explicitly judgement calls, so "no
   findings" is a state review may never reach, and looping toward it burns the
   stint.

   Findings are not orders. A finding you believe is wrong gets a stated reason,
   not a silent fix and not a silent skip — see `/receiving-code-review` if it
   is installed.

7. **Report.** The branch line from step 2 if it applies, the tickets covered,
   the SHA range, suite result, every finding left open at step 6, and which
   tickets remain for the next stint.

## Rationalizations

| Excuse | Reality |
|--------|---------|
| "One more ticket while I'm here" | The limit is unread code, not appetite. Needs a module you haven't opened → next stint. |
| "I'll skim that other module quickly" | Skimming to justify extending the stint is how a stint stops being one sitting. Reading it *is* the cost you were avoiding. |
| "The user's ticket range looked wrong so I trimmed it" | A named range is the stint. Disagree in the report, not by quietly doing less. |
| "They can see it's main, no need to say it" | Step 2's line is required output, not a courtesy. Unstated means unnoticed in a log nobody reads. |
| "I'll review before committing, so nothing bad gets committed" | Then review diffs the fixed point against itself and finds nothing, and you commit unreviewed work believing it passed. Commit first; that is what gives review something to read. |
| "I'll just hand review a working-tree diff instead" | `/code-review` pins its own diff command and delegates to sub-agents that never see your substitute. Commit instead of fighting it. |
| "I already fixed everything, re-running review is ceremony" | The re-review reads the *fixes*, which no review has seen. That is the pass most likely to find something. |
| "One more round would clean up the last finding" | Two passes, then report. An open finding the user can see beats a third pass they can't. |
| "Reviewing per ticket is more thorough" | One review per stint is the point — it is where the token saving lives. Review the stint, not each slice of it. |
| "No typechecker here, I'll add mypy/tsc" | Adding a dependency to satisfy a process step is the process eating the project. Note its absence and move on. |

## Red flags

- A ticket started that needs code this run hasn't read
- `/code-review` invoked without a SHA captured at step 1
- `/code-review` invoked while `git rev-parse HEAD` still equals that SHA — the diff is empty and the clean result is meaningless
- A review that returns no findings on a stint that changed real code
- A commit on `main`/`master` with no required line in the report
- Findings addressed, then straight to commit
- A third review pass
- `/code-review` run more than twice in one stint
- A new dev dependency you decided on alone

**Each means: stop and re-read the sizing rules.**

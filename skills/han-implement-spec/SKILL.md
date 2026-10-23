---
name: han-implement-spec
description: Implement a spec's ticket graph on one integration branch, spending agents, worktrees, strong models and full test runs only where the work needs them.
disable-model-invocation: true
---

# han-implement-spec

Implement every ticket of a spec on one **integration branch**, resolving each
the way the tracker closes work. The spec and tickets can come from
`/han-to-tickets` or any tracker that holds them.

The tickets are a **task graph**: blocking edges leave a **frontier** of tickets
ready now. Cost follows the graph's shape. Agents, worktrees, strong models and
full suites go where concurrency, difficulty or reach demand them; mechanical
work stays cheap.

## Budget

Every step below spends under these rules.

- **Pointers, not copies.** Brief agents with context pointers: spec section,
  ticket, decision ID, commit. Read the named sections and a ticket's Read first
  entries rather than whole documents. Re-read only what changed since.
- **Tiers, not model names.** Route each piece of work to a tier:

  | Work | Tier |
  |---|---|
  | Merge, rebase, run a check, fetch, look up a fact, close tickets | **economy**: the cheapest model offered that runs tools reliably |
  | Low or Medium complexity ticket | **standard**: the session's default model |
  | High complexity ticket, any review | **strong**: the most capable model offered |

  The ticket's Complexity field picks its tier. Without one, judge from the
  ticket and state the tier chosen. An explicit model choice from the user
  overrides this table. A harness that offers no model choice runs everything
  on the current model.
- **Quiet waits.** Send a long command's output to a log file and wait on its
  completion signal: a background-task notification, or a blocking wait with a
  timeout longer than the expected run. A status message carries new evidence;
  with nothing new, send nothing. Where the harness demands periodic updates,
  keep them to one line without re-inspecting anything.
- **Agents earn their spawn.** A subagent pays to read the codebase cold. Spawn
  one for concurrency, for the fresh context a review needs, or to offload a
  heavy main context. The main session runs routine git operations itself.

## Steps

1. **Read the graph.** The spec, then each ticket's blockers, Complexity, Blast
   radius and Behavior this changes. No tracker configured → ask, or point to
   `/setup-matt-pocock-skills` if installed. A ticket blocked by an unresolved
   decision stays off the frontier; report it.

2. **Create the integration branch** and record its base SHA: the final review
   measures against it. If the tracker closes work through PRs, or the user asks
   for one, open a draft PR after the first commit, marked as closing the spec
   and tickets.

3. **Pick the mode for the current frontier**, and state it in one line with
   the reason:
   - **Sequential**: one ready ticket, built directly on the integration
     branch: no worktree, no merge. The main session builds it when its own
     model meets the ticket's tier; otherwise one implementer agent on that
     tier does, on the same branch. Once the main context is
     heavy (several tickets absorbed, or the next ticket needs areas not yet
     read), hand the rest of the chain to one implementer agent working on the
     integration branch.
   - **Parallel**: two or more independent ready tickets. One implementer agent
     per ticket, in the background, each in its own worktree branched from the
     integration tip. The main session merges each result as it reports, using
     `/resolving-merge-conflicts` if installed. Tickets that edit the same
     symbols run sequentially instead: the conflict costs more than the
     concurrency saves.

4. **Build each ticket.** The implementer, main session or agent:
   1. Confirms its branch contains the integration tip.
   2. **Impact sweep.** Before writing code, lists the existing tests that
      assert behavior this ticket changes: start from Behavior this changes,
      then search the tests for the symbols and user-visible text it touches.
   3. Builds with `/tdd` at the ticket's seams. Without `/tdd`: a failing test
      through the public seam, the code that passes it, then refactor.
   4. **Targeted checks**: the swept tests, the new tests, and the typechecker
      if the repo configures one. The **full suite** is every check the repo's
      CI runs; a Shared or System-wide ticket runs it once, after any gate
      review fixes (step 5).
   5. Updates a swept test's expectation only where the ticket changes that
      behavior on purpose, and records each one with its source criterion.
   6. Commits, naming the ticket, then reports in a few lines: commit, checks
      and results, changed expectations, open questions.

5. **Gate risky upstream tickets.** A ticket that blocks others and is rated
   High complexity or Shared/System-wide blast radius gets one review before
   its dependents start: `/code-review` from the commit before it, strong tier.
   Fix its findings in the same implementer. Every other ticket waits for the
   final review.

6. **Advance the frontier.** After each ticket lands, recompute the frontier
   and pick its mode again (step 3).

7. **Final review, two passes at most.** With every ticket in, `/code-review`
   the integration branch against the base SHA from step 2. Fix the findings in
   one implementer with targeted checks, commit, then review once more: that
   pass reads the fixes. Findings still open after it are reported, not fixed.
   A finding you believe wrong gets a stated reason. Without `/code-review`: a
   fresh strong-tier agent compares the diff with the spec's criteria and the
   repo's documented standards.

8. **Batch gate.** Run the full suite once on the final integration tip. A red
   result gets a fix and a rerun; the review passes are not repeated.

9. **Close.** A draft PR → mark it ready. Otherwise resolve each ticket the way
   the tracker closes work. Remove any worktrees this run created.

10. **Report.** The integration branch, tickets done and remaining, the mode
    used per frontier, checks and their results, open findings, and every
    existing test expectation changed with its cause. If the spec links a
    roadmap, record the same evidence under that slice in the roadmap,
    following the roadmap's own instructions for status.

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
  | Low or Medium complexity ticket with Local blast radius | **standard**: the mid-capability model offered, whatever the session runs on |
  | High complexity or Shared/System-wide blast radius ticket, any review | **strong**: the most capable model offered |

  The ticket's Complexity and Blast radius fields pick its tier. Without them,
  judge from the ticket and state the tier chosen. With only two models
  offered, standard is the cheaper one. An explicit model choice from the user
  overrides this table. A harness that offers no model choice runs everything
  on the current model.
- **Review budget.** The user reviews the finished branch, so the run spends
  one review by default, on the **riskiest gate** (step 5). A budget the user
  states replaces it: a number of reviews, or **full review**, which gates every
  risky upstream ticket and adds both final review passes (step 7). Checks and the
  batch gate run under every budget.
- **Quiet waits.** Send a long command's output to a log file and wait on its
  completion signal: a background-task notification, or a blocking wait with a
  timeout longer than the expected run. A status message carries new evidence;
  with nothing new, send nothing. Where the harness demands periodic updates,
  keep them to one line without re-inspecting anything.
- **A fresh agent per ticket.** Each ticket gets its own implementer agent,
  briefed with pointers only, so no ticket inherits another's context. The main
  session orchestrates: it reads the graph, dispatches, merges and gates, and
  builds no tickets. It runs routine git operations itself. A harness without
  subagents builds each ticket in the main session, in graph order.

## Steps

1. **Read the graph.** The spec, then each ticket's blockers, Complexity, Blast
   radius and Behavior this changes. No tracker configured → ask, or point to
   `/setup-matt-pocock-skills` if installed. A ticket blocked by an unresolved
   decision stays off the frontier; report it. State the review plan in one
   line: the budget, and which tickets or passes it buys (step 5).

2. **Create the integration branch** and record its base SHA: the final review
   measures against it. If the tracker closes work through PRs, or the user asks
   for one, open a draft PR after the first commit, marked as closing the spec
   and tickets.

3. **Pick the mode for the current frontier**, and state it in one line with
   the reason:
   - **Sequential**: one ready ticket, or ready tickets that overlap. One
     implementer agent at a time commits directly on the integration branch: no
     worktree, no merge. The next ticket's agent starts after the previous one
     reports.
   - **Parallel**: two or more independent ready tickets. One implementer agent
     per ticket, in the background, each in its own worktree branched from the
     integration tip. As each reports, the main session merges it and removes
     its worktree. A clean merge stays in the main session; a conflicted one
     goes to an economy-tier merger agent, using `/resolving-merge-conflicts`
     if installed.

   Tickets **overlap** when they change the same existing behavior. Additive
   edits to a shared file, such as new entries in one registry, are
   independent: the merge resolves them.

4. **Build each ticket.** The implementer agent:
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

5. **Gate risky upstream tickets.** A **risky upstream ticket** blocks others
   and is rated High complexity or Shared/System-wide blast radius. Rank them:
   widest blast radius, then highest complexity, then most dependents. The
   budget buys gates in that order: the default buys the **riskiest gate**
   alone, a number buys that many with any remainder going to final review
   passes, and full review buys every gate and both final passes. A gated
   ticket gets one review before its dependents start: `/code-review` from the
   commit before it, strong tier. Fix its findings in the same implementer. No risky upstream
   ticket → no gate, and the budget goes unspent.

6. **Advance the frontier.** After each ticket lands, recompute the frontier
   and pick its mode again (step 3).

7. **Final review, when the budget buys it; two passes at most.** When the
   budget buys no final pass (step 5), go to step 8. With every ticket in,
   `/code-review` the integration branch against the base SHA from step 2. Fix
   the findings in one implementer with targeted checks, commit, then, when the
   budget buys a second pass, review once more: that pass reads the fixes.
   Findings still open after the last pass are reported, not fixed. A finding
   you believe wrong gets a stated reason. Without `/code-review`: a fresh
   strong-tier agent compares the diff with the spec's criteria and the repo's
   documented standards.

8. **Batch gate.** Run the full suite once on the final integration tip. A red
   result gets a fix and a rerun; the review passes are not repeated.

9. **Close.** A draft PR → mark it ready. Otherwise resolve each ticket the way
   the tracker closes work. Remove any worktree this run left behind.

10. **Report.** The integration branch, tickets done and remaining, the mode
    used per frontier, checks and their results, the reviews run and the
    tickets no review read, open findings, and every
    existing test expectation changed with its cause. If the spec links a
    roadmap, record the same evidence under that slice in the roadmap,
    following the roadmap's own instructions for status.

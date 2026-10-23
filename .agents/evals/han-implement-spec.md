# han-implement-spec and related updates — evaluation notes

Agreed 2026-10-23 through a `/han-grill` session. Maintainer material, outside
installed skills. The owner asked for light testing: Haiku subagents only, and
none for small updates the author is confident in.

## Source problem

A Kleeve session ran Matt Pocock's `implement-spec` on a strict ticket chain
(07 → 08 → 09). Its retro, supplied by the owner, reported five subagents all on
the strongest model (including a merger for a conflict-free merge), whole-document
reads of about 20k tokens, dozens of wake-and-report cycles during an 8-minute
browser suite, and three existing tests asserting old behavior found only by the
full run. Model names in that retro are harness-specific; the skill names tiers.

## Case I1

Scenario file kept outside the repo during the run; condensed here. Local
Markdown tracker. Strict chain 07 (High, Shared, changes Layers row behavior
during a Pen draft, unblocks 08) → 08 (Low, Local) → 09 (Medium, Local).
`pnpm test` 40 s, `pnpm typecheck` 20 s, `pnpm test:e2e` 89 scenarios ~8 min.
Models `large` and `small` selectable per subagent. `/tdd`, `/code-review` and
worktrees available. Task: write the execution plan only, 400 words max.

Pass: no worktree or merger agent on a single-ticket frontier; impact sweep
before code; 07 reviewed before 08 starts; full e2e at most once per Shared
ticket plus once at the end; quiet waits; economy tier for mechanical work.

## Results

RED, upstream `implement-spec`, two Haiku samples: both gave every ticket of
the strict chain its own worktree and a merger subagent, both spawned an
exploration subagent, neither looked for existing tests the 07 change would
break, and neither reviewed 07 before dependents started; one started 08 while
07's e2e run was still pending. Full e2e ran two and three times. Both routed
merges to `small` unprompted, so model routing alone is not an improvement
claim on this case. Sample reason: "The chain is strict, so the only overlap is
ticket 08 during the 07 e2e run."

GREEN, first draft, two Haiku samples: both used sequential mode with no
worktrees or merger, ran the impact sweep, gated 07 with one review, waited on
completion signals without status messages. Both still spawned a `large`
implementer for 07 although the draft said the main session builds sequential
tickets: the tier table and the mode rule conflicted. Both reran the full suite
after every fix commit (three to four e2e runs). Revised: a sequential ticket
goes to an agent only when the main session's model is below its tier; a
Shared ticket's full suite runs once after its gate fixes; final review now
precedes a single batch gate; full suite defined as the CI checks.

GREEN rerun after revision, one Haiku sample (the task ended on a connection
error after its full report arrived): sequential throughout, no worktree or
merge, 07 assigned to a strong agent only "if the main session cannot meet
strong", impact sweep, 07 gated before 08, full e2e twice (07 after its gate
fixes, then the batch gate), quiet waits. Final review preceded the batch gate.
One sample is thin evidence; it shows the revised rules are followable, not
that they hold across runs.

## Not tested

`han-grill-with-docs` (thin wrapper) and the `han-to-tickets` briefing sections
were not run through subagents, by the owner's waiver for confident small
changes. The briefing sections were modelled on Kleeve's ticket 07, whose
Read first, Agent notes and Unblocks sections carried the implementer through
a High-complexity change. Line-number pointers in that ticket are replaced by
file plus symbol.

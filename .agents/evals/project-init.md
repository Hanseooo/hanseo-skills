# project-init — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's phases, generation rules or completion criteria (see the operating
contract's skill verification rules). Add a scenario whenever a real session
exposes a new failure.

## Import into this repo — 2026-10-24

RED ran the owner's existing dotfiles version (`agent-config/shared/project-init.md`)
in place of "no skill", since the change is to that procedure. One Haiku sample
per arm.

- **A — shared AGENTS.md, Claude Code, no CLAUDE.md**: an `AGENTS.md` written by
  groundwork (Documentation Map, a `charter:begin` block, a Keep docs current
  table, a placeholder Commands line); pnpm lockfile, scripts and CI; a
  `.worktrees/` checkout; a Stripe wrapper with recorded fixtures. "fill in the
  commands and testing stuff".
  - RED: no CLAUDE.md ("The user did not ask for them, and no other agent
    tool's file exists"), so Claude Code would never load the file. The Keep
    docs current table was left unchanged, with no row for the Commands or
    Testing Contract it wrote. Wrote "(inferred from vitest; unverified)"
    inline despite the rule against annotating. Already held: the charter
    block and Documentation Map kept, worktree noted, pnpm lock.
  - GREEN: CLAUDE.md created as `@AGENTS.md`. Rows added for Commands, Tooling
    Lock and Last reviewed, existing rows kept, the Known Gaps row dropped
    with its section. No inline evidence tags. Soft spot, not fixed: the
    single-test form came from vitest's CLI rather than a script, and went
    in untagged rather than into the report.

## Interview, honest stamp and no shim — 2026-11-04

RED ran the skill as of `2b64e78`. One Haiku sample per arm, per round. A small
sample shows the rules are followed, not that the resulting files help agents
more. Fixtures are throwaway repos; a user reply is simulated by putting answers
in the opening message, since subagents have no question tool.

- **A — messy existing file, "just replace it. I'm in a hurry, ship it."**: a
  charter block, an `## Agent skills` section, a stale `pnpm lint`, a pointer to a
  missing doc, a line of rename history, generic guidelines, and a
  `CLAUDE.local.md` with no import.
  - RED: kept the owned sections and caught the stale lines, but never saw that
    `CLAUDE.local.md` stops `AGENTS.md` loading, proposed a shim, and asked six
    questions.
  - GREEN: replaced without re-asking. Both owned sections hash-identical to the
    original. Load blocker reported with the import proposed. Stale lint and
    broken pointer cut, existing date left alone, no shim.
- **B — fresh app with a dev script, a seed script and a deprecated client,
  no answers**.
  - RED: wrote at once with no question about hidden facts or scope, no Run line,
    stamped today with nothing run, kept "outranks" boilerplate, an architecture
    line, inline evidence, a shim. Asked about fixing the repo.
  - GREEN: Run line and migration line present, no stamp, no shim, repo problems
    in the report. Soft spot: single-test form from vitest's general usage.
- **B2 — same app, answers and check approval up front.**
  - GREEN: scope and invariants from the user's words, no interview. Soft spots:
    ran `pnpm install` unapproved and `pnpm test:e2e`, then stamped although every
    check stopped on a missing tool.
  - REFACTOR: checks named exactly, install needs its own approval, single test
    needs a run, no stamp unless every check reached the code. Re-run without
    install approval: ran the four checks only, asked about install, no stamp.
    Re-run with it: installed, ran the four, no e2e, Known Gaps carry each failure,
    no stamp, single test in the report.
- **C — uv API with `.cursor/rules/style.mdc`, no mention of a team.**
  - RED: added the baseline block because `.cursor` exists, plus a fallback line
    and a shim.
  - GREEN: asked the team question and added no block.
- **C2 — same repo, "our team of five", plus the cents rule in the message.**
  - RED: stamped unrun, shim, demanded repo confirmation of the user's stated rule.
  - GREEN: team block, cents rule from the user's words, no stamp, no shim. Soft
    spots: new `.cursor/rules/project.mdc` beside the team's existing file, and a
    single-test form from pytest's general usage.
  - REFACTOR: a new tool file needs a request by name. Re-run: only `AGENTS.md`
    added, pointer proposed in the report, single test in the report.

Not fixed: agents still add questions beyond the open-item table (critical paths,
merge gating in B2). Harmless, skippable, and each would change a line.

## Skills CLI skipped the skill — 2026-11-04

The description's unquoted `Usage: /project-init` is invalid YAML. Claude Code
accepted it, so every test passed, while `npx skills add` warned, skipped the skill
and still printed "Done" for the rest. Check after any description edit:
`npx skills add <repo> --list` shows `Found 11 skills` (the current count) with no
`Skipped` line.

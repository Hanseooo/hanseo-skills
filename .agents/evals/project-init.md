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

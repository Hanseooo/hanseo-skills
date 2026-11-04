<!--
Template notes (do not copy into output):
- Tier scaling: prototype → half a page, guardrails + conflict clause only, no
  architecture.md references; product → the full template, lean; platform → full
  template plus any project-specific sections the interview surfaced.
- If a CLAUDE.md already exists: add/update ONLY a clearly delimited charter section
  (between the markers below); leave everything else untouched.
- Wording must stay tool-agnostic (say "agent", not the name of any product).
- An AGENTS.md with no CLAUDE.md, or with a CLAUDE.md that only imports it: this block,
  markers included, goes in AGENTS.md and no CLAUDE.md is written.
- No AGENTS.md: offer to mirror the file as AGENTS.md; same content, both maintained
  together.
- Guardrail IDs are stable. Number from the highest G- ID already in the file. A G- rule
  already in the file stays where it is; cite its ID rather than restating it.
- Trace: (ADR-NNN), or (user rule) for a rule the user stated with no tradeoff to record.
- A Documentation Map's `Always read` line stays as it is. A new doc gets a `Read when` line.
- Check: a command or test that fails when the rule breaks, `none yet` when a tool could
  check it but nothing does, or `advisory` for a judgment no tool can make. No code yet
  means `none yet`.
-->
<!-- charter:begin -->
# <Project> — Operating Contract

Guardrails only. The system's shape lives in <architecture.md path>; the reasoning
lives in the ADRs at <ADR path>. Read those before large changes — do not restate
them here.

## Locked decisions
Every decision in <ADR path> is frozen. To change one: write a new ADR and flag it
to a human — do not implement the change first.

## Conflict clause
If a task seems to require violating an ADR, stop and surface the conflict.
Do not guess, do not work around it.

## Guardrails
- G-01: <guardrail>. Check: <command or test | none yet | advisory>. (<trace>)
- G-02: <guardrail>. Check: <command or test | none yet | advisory>. (<trace>)
<!-- charter:end -->

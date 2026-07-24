<!--
Template notes (do not copy into output):
- Tier scaling: prototype → half a page, guardrails + conflict clause only, no
  architecture.md references; product → the full template, lean; platform → full
  template plus any project-specific sections the interview surfaced.
- If a CLAUDE.md already exists: add/update ONLY a clearly delimited charter section
  (between the markers below); leave everything else untouched.
- Wording must stay tool-agnostic (say "agent", not the name of any product).
- Offer to mirror the file as AGENTS.md; same content, both maintained together.
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
- <guardrail — traces to ADR-NNN>
- <guardrail — traces to ADR-NNN>
<!-- charter:end -->

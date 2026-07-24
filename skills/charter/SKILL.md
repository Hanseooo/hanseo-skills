---
name: charter
description: Use when starting a new project or joining an early codebase to set up its decision layer — an architecture doc, ADRs / architecture decision records, or an agent operating contract (CLAUDE.md / AGENTS.md). Triggers include "charter this project", "set up ADRs", "write guardrails for agents", and single-seam requests like "make X swappable" or "I need an abstraction layer for Y". Not for auditing code health, prioritizing work, feature-level design, or scaffolding.
---

# charter

Produce a project's decision layer: `architecture.md`, ADRs (each with an escape
hatch), and an operating contract (CLAUDE.md). Create **no folders and no code** —
the folder map is a section inside architecture.md; structure materializes when
the first module is built.

## Not this skill
- Codebase health audit → improve-codebase-architecture / improve
- "What should I work on next?" → improve
- Feature-level design → brainstorming → feature specs
- Scaffolding (directory trees, boilerplate, stubs) → nothing; never do this

## Route check (always first)
0. **Amend check.** If charter artifacts already exist (architecture.md, ADRs, or
   a `charter:begin` section in CLAUDE.md) → **amend mode**: new ADRs, targeted
   revisions, tier upgrades. Never a duplicate charter.
1. User wants ONE seam designed ("make X swappable") → **Route C**: load
   `references/heuristics.md` only. Design the seam — what the interface hides
   and exposes, wrapper vs. adapter, what NOT to abstract. Output: one ADR (plus
   an architecture.md update if that file exists). No interview. If no charter
   exists, nudge once ("a full charter would give this a home — run charter when
   ready") and proceed anyway.
2. No code yet (docs may exist) → **Route A**. Some code → **Route B**.

## Process (Routes A and B)
1. **Doc inventory.** Glob PRD*, design*, architecture*, MASTER_SPEC*, ADR*,
   CLAUDE.md, AGENTS.md, docs/**. Read what exists. Never re-ask what a doc
   answers — confirm the inference instead. Close with: "Any docs I missed — or
   external ones (Notion, wiki)?"
2. **(Route B only) Code inventory** per `references/existing-code.md`.
3. **Priority dial + interview** per `references/interview.md` — one question at
   a time, only for gaps. Budget: prototype ≤3, product ≤6, platform ≤10.
4. **Propose 2–3 architecture shapes** with tradeoffs and a recommendation, using
   `references/heuristics.md`. Discuss one question at a time until approved.
5. **Write artifacts** from `references/templates/`. Merge, never overwrite:
   existing CLAUDE.md gets only a delimited charter section; existing ADRs keep
   their own format and numbering; existing architecture.md is amended.
   Monorepo: charter the root by default; if clearly multi-package, ask which.
   Offer an AGENTS.md mirror; keep contract wording tool-agnostic either way.
6. **Self-review gate.** Every guardrail traces to an ADR; every ADR has an
   escape hatch; no abstraction without a stated volatility; no contradiction
   with existing docs; no placeholders.
7. **Handoff.** Name optional companions only if installed: feature design →
   brainstorming; plan stress-tests → grill-with-docs; later health checks →
   audit skills. The charter stands alone without them. Close by pointing the
   user at a fresh session that reads these artifacts before building — the
   decision layer is the cheap context reload, not this conversation's history.

## Rules
- Defaults recommend, users decide. One pushback = record the choice and move on.
  A recorded disagreement is a successful outcome.

# Route B: early-code inventory

Goal: surface the decisions already embedded in the code as **draft retroactive
ADRs** the user confirms or corrects. The charter documents reality plus agreed
deltas — it never silently prescribes a rewrite.

## Steps
1. **Skim structure only.** Folder tree, entry points, manifests (package.json,
   pyproject.toml, go.mod…), config files, and a handful of central source files.
   Not the whole codebase. This is not an audit: do not assess health, list
   problems, or rank fixes.
2. **Infer implicit decisions** — choices with charter weight:
   - vendors called directly vs. wrapped (e.g., "OpenAI is called from three
     handlers directly")
   - storage/framework choices and how tightly code couples to them
   - where module boundaries actually fall (who imports whom)
   - implicit contracts (shared dict shapes, duplicated types)
3. **Present each as a draft retroactive ADR**, one at a time: "The code
   currently decides X. Confirm as locked, revise it, or mark it as a decision
   you intend to change?" Use the ADR template with Context stating this
   documents an existing decision.
4. **Record deltas as decisions, not criticism.** If the user wants something
   changed, that becomes an ADR describing the target state — not a to-do list
   of fixes against the current code.
5. Continue the shared flow at the priority dial.

## Boundaries
- Health scanning, dead-code hunting, prioritized fix lists → out of scope; the
  handoff step names the tools that own them.
- If the repo is too large to skim (clear monorepo, many packages), ask whether
  to charter the whole repo or one package before inventorying.

## Large repo: delegate the skim, keep context clean
Charter's default is a hands-on skim — for an early/small codebase, just do it.
Only when the chosen scope is still too large to skim without flooding context:
dispatch one read-only exploration subagent (Explore) to return the folder tree,
entry points, manifests, and a shortlist of candidate implicit decisions with the
central files behind each. Then read only that handful of files yourself before
drafting retroactive ADRs — the subagent locates, you judge. Never let the
subagent write ADRs or assess health; that stays in the main session.

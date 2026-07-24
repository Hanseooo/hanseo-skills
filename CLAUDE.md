# Agent Operating Contract

This file governs how agents must behave when working on the `hanseo-skills` repo.

## Structure rules

- Skills live in `skills/<name>/` — one folder per skill
- Each skill must have `SKILL.md` (machine-readable instructions) and `README.md` (human/install docs)
- No category subfolders until ≥4 skills share a domain; then introduce `skills/<category>/<name>/`

## Anti-drift rules

- Adding a skill → update root README skill table in the same commit
- Renaming or removing a skill → update root README; fix any CONTEXT.md terms introduced for it
- Adding a domain term → update CONTEXT.md in the same commit
- `AGENTS.md` mirrors `CLAUDE.md` exactly — edit one, edit the other in the same commit

## Install guide rules

- Root README shows the general install pattern and links to each skill
- Each skill's `README.md` must have an `## Install` section with its specific command
- Install command format: `npx skills add Hanseooo/hanseo-skills` (whole collection) — individual skill installs are documented in each skill's own README

## Invocation classification

- Every skill must be classifiable as user-invoked or model-invoked — see `.agents/invocation.md`

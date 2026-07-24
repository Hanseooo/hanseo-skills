# ADR 0001: Flat Skills Layout

**Status:** Accepted

## Context

One skill exists (`charter`). Category subfolders would be premature — there is nothing to categorize yet.

## Decision

Skills live flat in `skills/<name>/` with no category subfolders until the escape hatch triggers.

## Consequences

- Simple, predictable paths: `skills/charter/SKILL.md`
- Easy installs: `npx skills add Hanseooo/hanseo-skills`
- Path gets noisier beyond ~6 skills if they span multiple domains

## Escape hatch

When ≥4 skills share a domain, introduce `skills/<category>/` folders. Existing skills need no migration — move their folders and update README links. No other trigger justifies early categorization.

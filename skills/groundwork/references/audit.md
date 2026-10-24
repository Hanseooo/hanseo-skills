# Audit an existing repo

Run during intake when the repo already has docs. The output is a table the
user picks from; nothing is written until they do.

## Classify every doc

| Status | Meaning | Evidence to cite |
|---|---|---|
| Present | Owns its row and matches the code | — |
| Stale | Contradicts the code | The doc line and the code path |
| Competing | Claims a row another doc also claims | Both docs and the overlapping facts |
| Unrouted | Live work nothing points to | The folder and its newest date |
| Missing | An owner row whose trigger is met, with no doc | The trigger evidence |

Ownership follows the owners table and the code, never a doc's description of
itself. A file calling itself "canonical" next to two others covering the same
scope is Competing, whatever it says.

## What to look for

- **Commands.** Compare documented commands with the manifest scripts, task
  runner and CI. A documented command with no source is Stale. A script with a
  test, lint, typecheck or e2e role and no documentation is Missing, usually
  the most valuable gap. Either one puts the `/project-init` session first in M0.
- **Status.** Todo files, handoff logs and "current state" sections all hold
  status, whose owner is the project roadmap. Propose moving live status there
  and reducing the old doc to a pointer.
- **Cached config.** Stack lists, versions and folder trees restated in prose.
  Propose replacing them with a pointer to the file that holds them.
- **Identical or near-identical files.** Propose one owner. The other becomes a
  pointer, or the user removes it.
- **Repeated rules.** The same rule in `AGENTS.md`, `CLAUDE.md` and a spec. It
  stays in its owner; the rest link.

## Repo conventions

Keep what the repo already does: its doc paths, numbering, ADR form (files or
one table) and work-tracking layout. Moving to a different layout is an M0
session the user can choose, never part of the audit.

## Competing docs

Consolidation is an M0 session, because it needs reading each doc in full and
the user's rulings on conflicts. It sends each fact to its owner: scope to the
brief, order and status to the roadmap, behavior to specs, decisions to ADRs.
Merging everything into the largest doc recreates the problem. When the
competing docs hold the scope, this session also writes the brief from them,
and intake skips its destination step. Superseded
docs get a header naming their successor; the user decides whether to remove
them later.

# Docket roadmap handoff scenarios

Neutral transcript fixtures for paired Luna checks. Each case is independent.
Use the supplied facts as evidence. Show concrete artifact excerpts or edits and
next actions, at most 140 words per case. Do not write project files, publish,
implement, inspect other skill files, or invent unavailable evidence. The
harness supplies instructions separately; no scenario implies another case's
state. Return actions and reasons, not a self-assessed pass/fail.

## A: Initial roadmap

User: /docket Map CSV report exports for our existing report screen. Agreed outcome: people can request exports up to one million rows, see progress, cancel, and download the result. PDF and scheduled email are excluded. Existing verified facts: the screen and row query exist; no export worker exists; request timeouts may matter but job duration is not measured. Users only access their own reports, preserving existing permissions. Choose outcome-based slices; shared constraints are CSV, existing permissions, no new paid service. I need a useful handoff today, not a full product design. All other choices can wait for the slice discussion. Show the planning artifact(s) you would produce and the next step.

## B: Unfinished previous slice

User: Refer to planning/report-exports-roadmap.md and grill me about slice S2, progress/cancel. S1, request/download, has spec https://tracker.example/specs/41 and status In progress. S1 acceptance AC1 valid CSV and AC2 cancelled exports release the generated file. Tests https://ci.example/run/17 verify AC1; review https://tracker.example/review/18 reports AC2 fails (file persists). User explicitly accepted a separate minor button-alignment finding, not the failing AC2. Roadmap goal is request/progress/cancel/download up to one million rows, excluding PDF/email. D1 existing permissions remains confirmed, source meeting/7. We have no time for another full audit. Let's discuss S2 now; the previous build is basically done. Show your response and any roadmap edit(s).

## C: Completed slice, nonzero findings

User: Refer to planning/report-exports-roadmap.md while we brainstorm S2 progress/cancel. S1 request/download remains In progress in the roadmap; spec https://tracker.example/specs/41, criteria AC1 valid CSV and AC2 generated files are cleaned up. Supplied CI https://ci.example/run/22 checks both and passes. Review https://tracker.example/review/23 confirms both criteria, finds only a minor button alignment issue, and is complete. User explicitly accepts that residual finding in comment https://tracker.example/comment/24. S2 depends on S1; S3 scheduled email is only an optional suggestion outside the agreed CSV request/progress/cancel/download outcome. D1 existing permissions, source meeting/7, stays binding. We already supplied enough evidence; don't redo the audit. Show your response and any roadmap edit(s).

## D: Adaptive roadmap without scope creep

User: We are brainstorming with planning/report-exports-roadmap.md before I invoke the spec skill myself. Goal CSV exports up to one million rows: request, progress, cancel, download. Existing slices S1 request/download and S2 progress/cancel. D1 existing permissions, source meeting/7; no spec exists yet. New verified benchmark https://ci.example/benchmark/9: one-million-row exports take 90 seconds; requests time out after 30 seconds. We need background processing to meet the agreed goal. PDF seems useful too, and email could be interesting. Keep the roadmap useful, don't make me approve every adjustment. Explain your roadmap edits and next discussion step; do not implement anything.

## E: User-invoked tracker spec and handoff

User: /to-spec Publish the agreed S1 CSV request/download spec to our configured issue tracker. Source roadmap planning/report-exports-roadmap.md. D1 existing permissions, source meeting/7. Agreed S1 behavior: request CSV from own report, download when ready; failed requests show a retryable error, and cancellation leaves no generated file. Acceptance checks are given request succeeds -> CSV downloads; given request fails -> retry available; given cancellation -> no file remains. These choices are confirmed; no further blockers. S2 progress/cancel has an outline only. No issue has been published yet. Preserve the source context needed by the later build/review session. Simulation: show the tracker publication content/actions and any linked roadmap update, but make no tool calls or real external changes.

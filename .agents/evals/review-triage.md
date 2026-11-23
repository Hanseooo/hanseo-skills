# Review triage and composition wording — evaluation

Run 2026-11-23. Maintainer material, outside installed skills. Subject model:
Haiku, three fresh samples per arm, to save tokens. A pass on Haiku is likely
to carry to stronger models (*inferred*); a Haiku-only failure may not.

## R1 — Does stint need an explicit verify-findings step?

Proposed change: after `/code-review`, verify each finding against the code,
mark it fix, leave or wrong with a reason, then rerun targeted checks on the
fixes. It encodes the owner's usual follow-up prompt.

Case: `review-triage-cases.md` R1. Pass: F1 rejected with the line-2 guard
cited; F2 fixed test-first with targeted checks rerun; F3 fixed or left with a
reason; F4 reported as a spec decision, not silently fixed.

**RED (current `skills/stint/SKILL.md`): 3/3 pass.** All three triaged before
editing, rejected F1 citing line 2, wrote a hand-worked red test for F2,
reran the targeted file, and reported F4 with clamp/reject options. Step 6's
"Findings are not orders … a stated reason" already produces the behavior.

**Decision:** no change to stint or han-implement-spec. With no failure there
is nothing for the wording to fix. Not covered: long real `/code-review`
output, findings whose falsity needs another file opened, and `/code-review`
used outside a build skill. A real transcript where an agent fixed a finding
without checking it would be the next fixture.

## C1 — Bare `/code-review` versus "Call the Skill tool"

Prompt: stint's step 5 text with a bare `` `/code-review` ``; the agent writes
its next tool call.

**Control (bare slash): 3/3 called `Skill` with `code-review` and the SHA.**
No failure, so the Skill-tool form was not tested and stint and
han-implement-spec keep their wording. `.agents/invocation.md` records this.
Not covered: a real harness invocation rather than a written tool call.

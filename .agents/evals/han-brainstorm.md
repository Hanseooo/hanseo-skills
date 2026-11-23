# han-brainstorm — evaluation

Run 2026-11-23. Maintainer material, outside installed skills. Subject model:
Haiku, three fresh samples per arm. One agent plays both assistant and
simulated user from `han-brainstorm-cases.md` B1, so it knows the hidden facts:
one GREEN run quoted "60%" before the user had said it. Treat leaks of that
kind as harness noise, not skill behavior.

## Pass criteria (B1)

1. Opens with one specific question; every message ends on one question.
2. Stays on what and why: proposes no direction, fix or experiment.
3. Actor, outcome, success and boundaries are each stated or marked as an
   assumption.
4. An integration check combines two user statements into an unstated
   consequence, raises it with the user and records it.
5. The streak freeze is parked, and the session's focus is asked.
6. The full brief is played back and confirmed.
7. The close suggests one next step by situation; here `/han-grill`, since
   how-choices remain. Nothing is invoked.

## RED (no skill): 0/3 pass

- **Pass in all three:** 5 and 6. Parking and playback come by default, so the
  skill gives them a line each, not rules.
- **2 failed 3/3.** Each proposed a direction mid-discovery: "Then there are
  two directions…", "one direction is to move the permission ask later",
  "Would it work to ask for permission later?"
- **7 failed 3/3.** Each closed on building: "make the change in the client",
  "ship the later-permission variant behind a flag", "I can write that up as
  a short plan". None routed the open choices (when and how to ask) to
  `/han-grill`.
- **4 failed 3/3**, and no run marked an assumption.
- **1 failed 2/3:** two questions per message, or compound questions.

## GREEN: 3/3 pass

All three held what and why, marked assumptions, parked the streak freeze,
confirmed the brief and closed on `/han-grill` without invoking it. Two of
three ran the integration check before all four items held and found only the
shallow consequence (declining means no reminders).

## REFACTOR: 3/3 pass

Change: "Once all four hold, run one integration check across everything the
user has said." All three ran it after the four items held. One then asked a
how question in the same message ("When could the permission be asked
instead?") and recorded the answer as the user's proposal; the other two kept
timing for `/han-grill`.

The case's deeper collision (a later permission ask removes reminders from the
days the day-7 metric counts) needs the user's answer on timing, which is a
how question the skill rightly withholds. Criterion 4 grades the check the
skill asks for, not that particular consequence.

## Not tested

- Project lookup: runs were barred from repo reads, and three runs named that
  as the only unclear wording.
- A user who stops early, a "not worth building" close, and the
  `/product-discovery` route.
- A real two-party conversation rather than one agent playing both sides.

## Provenance

Superpowers 7.0.0 `brainstorming` (installed copy): get the user describing,
one question per message, playback in parts. Every's `ce-brainstorm` on
`main`, read through a fetch subagent on 2026-11-23: the exit clauses, the
integration check and its muted-rule example, the "straining to fill option
slots" test and written assumptions.

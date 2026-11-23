---
name: han-brainstorm
description: Turn a vague itch into a confirmed brief of who has the problem, what should change and how success shows, ready for /han-grill.
disable-model-invocation: true
---

# han-brainstorm

Find out what the user wants and why, before anyone decides how. The result is
a **brief**: a short playback the user has confirmed. It stays in the
conversation, where `/han-grill` takes it as its intake.

## Stay on the problem

The brief holds the **what** and the **why**. The **how** (approaches, designs,
fixes, experiments) belongs to `/han-grill`, whose first round compares whole
approaches. When a solution occurs to you, turn it into a question about the
problem it would solve. When the user offers one, record it as their idea and
ask what it is for.

## Ask

1. Open by getting the user describing: one specific question anchored in a
   real moment or a concrete number. "When did you last notice this?" beats
   "Tell me more."
2. Every message ends on exactly one question. If you would strain to fill
   option slots, ask it open; offer two or three options only when the user is
   stuck or asks.
3. Look up what the project can tell you (code, docs, data the user points to)
   instead of asking for it.
4. Play back each part as soon as you can describe it, in a few sentences, with
   your guesses marked as guesses, and ask what is wrong or missing.

A second outcome that surfaces mid-conversation is **parked** in the brief, not
merged; ask which one this session is for.

## Done when

Each of these is stated by the user or written into the brief as an
**assumption**:

- **Actor**: who has the problem.
- **Outcome**: what changes for them when it is solved.
- **Success**: how the user will know, ideally something observable.
- **Boundaries**: constraints and exclusions.

Once all four hold, run one **integration check** across everything the user
has said: combine two of their statements and name the consequence neither
states. "Rules can be muted" plus "deleting a rule
needs no confirmation" means deleting a muted rule silently loses its paused
state. Ask about it; the answer joins the brief.

Play back the whole brief (actor, outcome, success, boundaries, assumptions,
the integration finding, parked ideas) and wait for the user to confirm it.
If the user wants to stop earlier, play back what exists with the gaps marked.

## Close

End with one suggested next step:

- Choices remain about how → `/han-grill`, with this brief as its intake.
- The change is small and its how is obvious → build it from the brief.
- Not worth building → say so and stop.
- The real question is whether a product should exist at all →
  `/product-discovery`.

The user starts it; nothing is invoked automatically. Specs, plans, tickets and
code are separate requests.

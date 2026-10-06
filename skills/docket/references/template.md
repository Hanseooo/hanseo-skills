# Docket template

Loaded when charting. Write the result to `docs/dockets/<feature>-docket.md` in
the user's project.

## The carried instruction

Every docket opens with this block, unchanged. It is what a session reads when
the docket is opened without the skill loaded, so it carries the few rules that
keep the file honest.

```markdown
> **Agent: read this first.** This docket is worked with `/docket`, one
> question per sitting. Without it: take the lowest-numbered open question whose
> blockers are all under Decisions. Look up facts yourself; put decisions to the
> user one at a time, each with your recommendation. Append confirmed decisions
> under Decisions and delete the question. Never edit a recorded decision —
> reopening one is a new question. Stop at decisions: no spec until no
> questions remain, and never plans or code.
```

## The body

Keep the worked example's shape when adapting: these lines are the only
illustration of what a usable question, decision and fog entry read like.

```markdown
# Messaging — docket

**Destination:** one spec at `docs/specs/messaging.md` covering 1:1 messages,
crew channels and a thread on every task, settled far enough to plan and build.

## Decisions
Settled with the user. Later questions treat these as given. Never edited.

- **Q1 · What a conversation is.** One model with a kind: direct, channel or
  task. A task's conversation is created on its first message. Messages are
  immutable; an edit is a new row pointing at the original.
  *Binds:* every later question works with one conversation model, not three.
- **Q2 · Can the host hold a streaming connection? (look up)** Yes, up to the
  plan's function time limit, so a long-lived stream must reconnect.
  *Source:* the host's function docs, checked 2026-10-06.

## Questions
Open only, numbered in the order they will be worked.

### Q3 · Delivery on patchy signal — decide
What "sent" means while offline, whether retries are bounded, what the sender
sees mid-flight.
**Blocked by:** Q1, Q2

### Q4 · Who can read and post where — decide
Each role against each conversation kind, and what happens on leaving a crew.
**Blocked by:** Q1

## Not yet specified
In scope, but not yet sharp enough to ask. Coarse on purpose.

- Notifications: push or in-app, and quiet hours on site. Waits on what
  "delivered" means (Q3).
- The inbox and the thread on a task card. Waits on the states Q3 and Q4 settle.

## Out of scope
- Voice notes and calls: a later effort.

## Found & parked
Real, but belongs to no question here. One line each, not acted on.

- 2026-10-06 (from Q1): `tasks.updated_at` is never bumped on edit. Unrelated
  to messaging; raise separately.
```

**A question names the decisions inside it.** "Delivery on patchy signal" alone
is a topic; the line under it is what makes it answerable in one sitting.

**A decision says what it binds.** The conclusion alone loses the reason later
questions must respect it.

**Keep every heading even when its list is empty.** An absent section reads as
no such rule.

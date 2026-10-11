# Docket template

Loaded when charting. Write the result to `docs/dockets/<feature>-docket.md` in
the user's project.

## The carried instruction

Every docket opens with this block, unchanged. It is what a session reads when
the docket is opened without the skill loaded, so it carries the few rules that
keep the file honest.

```markdown
> **Agent: read this first.** This docket is worked with `/docket`, one
> question per sitting. Without it: the current phase is the first on the
> Roadmap not marked built. Take its lowest-numbered open question whose
> blockers are all under Decisions. Look up facts yourself; put decisions to
> the user one at a time, each with your recommendation. Append confirmed
> decisions under Decisions and delete the question. Never edit a recorded
> decision — reopening one is a new question. When the current phase has no
> questions left, write its spec from the Decisions and stop until it is
> built. Later phases wait their turn. Never plans or code.
```

## The body

Keep the worked example's shape when adapting: these lines are the only
illustration of what a usable phase, question, decision and fog entry read like.

```markdown
# Messaging — docket

**Destination:** crews message each other 1:1, in crew channels and in a thread
on every task, and it keeps working on patchy signal.

## Roadmap
Phases in build order. Each works end to end once built, and is built before
the next is charted.

1. **Direct messages** — two members of a crew exchange text messages that
   survive patchy signal. Spec: `docs/specs/messaging-1-direct.md`
2. **Crew channels** — a shared channel for a crew, everyone in it reads and
   posts. Spec: `docs/specs/messaging-2-channels.md`
3. **Task threads** — a thread on every task card, with notifications for
   mentions and replies. Spec: `docs/specs/messaging-3-task-threads.md`

## Decisions
Settled with the user. Later questions treat these as given. Never edited.

- **Q1 · What a conversation is (phase 1).** One model with a kind: direct,
  channel or task. Phase 1 creates only direct conversations, on the first
  message. Message ids are made on the sending device.
  *Binds:* phases 2 and 3 add kinds to this model, not new tables.
- **Q2 · Can the host hold a streaming connection? (look up, phase 1)** Yes, up
  to the plan's function time limit, so a long-lived stream must reconnect.
  *Source:* the host's function docs, checked 2026-10-06.

## Questions
Open only. Worked by phase, then by number.

### Q3 · Delivery on patchy signal — decide · phase 1
What "sent" means while offline, whether retries are bounded, what the sender
sees mid-flight.
**Blocked by:** Q1, Q2

### Q4 · Who can post and moderate in a crew channel — decide · phase 2
Whether members or only leads post, and who can remove someone else's message.
**Blocked by:** Q1

## Not yet specified
In scope, but not yet sharp enough to ask. Coarse on purpose.

- Phase 1: the direct message screens. Waits on the states Q3 settles.
- Phase 3: notifications, push or in-app, and quiet hours on site.

## Out of scope
- Voice notes and calls: a later effort.

## Found & parked
Real, but belongs to no question here. One line each, not acted on.

- 2026-10-06 (from Q1): `tasks.updated_at` is never bumped on edit. Unrelated
  to messaging; raise separately.
```

**A phase says what a user can do once it is built.** "Direct messages" alone
is a label; the line after it is what makes the phase checkable. Its line gains
`· specified` when its spec is approved, which becomes `· built` when the build
matches it. Between the two, a `Build gaps:` list under the line holds what the
last check of the build found unfinished.

**A question names the decisions inside it.** "Delivery on patchy signal" alone
is a topic; the line under it is what makes it answerable in one sitting.

**A decision says what it binds.** The conclusion alone loses the reason later
questions and later phases must respect it.

**Keep every heading even when its list is empty.** An absent section reads as
no such rule.

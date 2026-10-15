# Docket template

Loaded when charting. Write the result to `docs/dockets/<feature>-docket.md` in
the user's project.

## The carried instruction

Every docket opens with this block. It supplies phase context when the file is
opened directly by a design, planning or build workflow.

```markdown
> **Agent: read this first.** This docket carries scope and decisions between
> phases. Use the phase the user names, otherwise the first Roadmap entry not
> marked `built`. Follow the workflow the user invoked. During design, explore
> the complete phase: Questions are starting points, not an exhaustive list
> or a limit on discovery. Respect confirmed Decisions and bring conflicts
> to the user for explicit revision. Record newly confirmed decisions and
> unresolved concerns here. A phase is `specified` after the user approves
> its written spec, not when its question list is empty. Plan, implement,
> review and fix that phase before returning to `/docket` for the next.
```

## The body

Keep the worked example's shape when adapting: these lines are the only
illustration of what a usable phase, question, decision and fog entry read like.

```markdown
# Messaging — docket

**Destination:** crews message each other 1:1, in crew channels and in a thread
on every task, and it keeps working on patchy signal.

## Roadmap
Phases in build order. Each works end to end once built. Design one phase,
approve its spec, plan, implement, review and fix before designing the next.

1. **Direct messages** — two members of a crew exchange text messages that
   survive patchy signal. Includes text and delivery feedback; crew channels
   and task threads wait for later phases. Depends on existing crew membership.
   Spec: `docs/specs/messaging-1-direct.md`
2. **Crew channels** — a shared channel for a crew, everyone in it reads and
   posts. Spec: `docs/specs/messaging-2-channels.md`
3. **Task threads** — a thread on every task card, with notifications for
   mentions and replies. Spec: `docs/specs/messaging-3-task-threads.md`

## Decisions
Confirmed with the user. Carry these forward; record revisions explicitly.

- **Q1 · What a conversation is (phase 1).** One model with a kind: direct,
  channel or task. Phase 1 creates only direct conversations, on the first
  message. Message ids are made on the sending device.
  *Binds:* phases 2 and 3 add kinds to this model, not new tables.
- **Q2 · Can the host hold a streaming connection? (look up, phase 1)** Yes, up
  to the plan's function time limit, so a long-lived stream must reconnect.
  *Source:* the host's function docs, checked 2026-10-06.

## Questions
Known open questions, tagged by phase. Starting points for design, not a
complete inventory. Numbers are stable references, not a required interview order.

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
Outside the destination. One line each, not acted on.

- 2026-10-06 (from Q1): `tasks.updated_at` is never bumped on edit. Unrelated
  to messaging; raise separately.
```

**A phase says what a user can do once it is built.** "Direct messages" alone
is a label; the line after it is what makes the phase checkable. Its line gains
`· specified` when its written spec is approved, which becomes `· built` when
implementation and review are complete and its spec holds. Between the two,
`Build gaps:` under the line holds unfinished behavior and blocking review
findings.

**A question explains a known uncertainty.** "Delivery on patchy signal" alone
is a topic; the line under it gives the design process a useful starting point.

**A decision says what it binds.** The conclusion alone loses the reason later
questions and later phases must respect it.

**Keep every heading even when its list is empty.** An absent section reads as
no such rule.

# Docket template

Loaded in plan mode only. Fill this in and write the result to
`docs/dockets/<feature>-docket.md` in the user's project. If the project already
has a spec convention, follow that instead and say so.

## The carried instruction

Every docket opens with this block, unchanged. Copy it verbatim.

```markdown
> **Agent: read this before anything else.**
> This docket governs a multi-session design. You are working ONE session.
> - Do not widen scope past the current session's cluster, and do not
>   re-decompose it. If it should split, append an amendment — don't split it
>   in-session.
> - "Binding constraints" are decided. To challenge one, append a NEW session.
>   Never edit a DONE session.
> - Stop at an approved spec. Do NOT continue to writing-plans or implementation.
> - To end the session, in this order: record the spec path, propose the
>   constraints it establishes, wait for the user to confirm them, write them in.
>   Only then set the session to DONE, and flip every session blocked on it to
>   READY. A session is not DONE until its constraints are confirmed.
```

Do not trim it. Each line defends against something specific:

- Line 1 stops brainstorming's scope-assessment step from re-decomposing a
  session that was already scoped here, and producing specs the docket never
  learns about.
- Line 3 overrides brainstorming's terminal handoff to writing-plans.
- Line 4 is the write-back. It is the only thing keeping the docket accurate,
  because control never returns to this skill after a handoff — the docket is in
  the session's context for its whole run, and docket-the-skill is not. Its
  ordering is load-bearing: agents perform these steps in the order they are
  listed, so status must come last. Listed first, sessions get marked `DONE` on
  spec approval alone and the constraint extract never happens.

## The body

```markdown
# Messaging — session docket

**Goal:** direct + group messaging inside the app
**Cut rationale:** ordering and identity constrain everything downstream, so they
go first; presence turned out independent of delivery, so it splits off.
**Spec path convention:** docs/specs/YYYY-MM-DD-<topic>-design.md

## Binding constraints
Decided in earlier sessions. Later sessions treat these as given, not open.
- [S1] Messages are immutable once sent; an edit is a new row referencing the original.
- [S1] Ordering is server-assigned. Client clocks are never authoritative.

## Sessions
Statuses: DONE (spec linked **and** constraints confirmed) · PARTIAL (stopped early,
resumable) · READY · BLOCKED (needs Sn)

### S1 · Message identity & persistence — DONE
**Cluster:** what a message is, how it is stored, how order is established
**Explicitly out:** delivery, presence, anything visual
**Stance:** persistence session — done means a schema plus the invariants it must never violate
**Spec:** docs/specs/2026-07-24-messaging-persistence-design.md

### S2 · Delivery & sync — READY (needs S1)
**Cluster:** send path, retry, offline queue, what "sent" means to the sender
**Explicitly out:** how any of it looks
**Stance:** failure-semantics session — done means every failure mode has a named, chosen behaviour
**Open questions:** does a send fail loudly or queue silently; is retry bounded; what does the sender see mid-flight

### S3 · Conversation UI — BLOCKED (needs S2)
**Cluster:** what the sender sees while a message is in flight, how failures read, how history loads
**Explicitly out:** the sync mechanism itself, which S2 settles
**Stance:** interaction session — done means every state a user can observe has a chosen rendering
**Open questions:** …

## Amendments
- 2026-07-26 (from S2): split S3; unread state turned out to be a sync decision,
  not a UI one. Now S3 (UI) + S4 (unread).
```

Keep the worked example when adapting — a blank template invites template-filling,
and these lines are the only illustration of what a usable cluster and stance read
like.

**`Explicitly out` carries as much weight as `Cluster`.** Naming what a session
must refuse is what stops scope creeping back to full-feature size.

## Statuses

| Status | Meaning |
|---|---|
| `DONE` | Spec approved and linked; constraints extracted and confirmed. |
| `PARTIAL` | Session started and stopped early. Decisions banked so far recorded inline; the session stays resumable. |
| `READY` | Dependencies satisfied, not started. |
| `BLOCKED` | Waiting on a named upstream session. |

`PARTIAL` exists so an interrupted session does not evaporate.

# han-brainstorm — case

Read by the eval agent. Pass criteria live in `han-brainstorm.md`.

## B1 — A vague itch

Write the full conversation, playing the assistant against a simulated user.
The user's opening message:

> I keep feeling like my app is losing people early on. Maybe I should build
> something about it? Not sure what.

The simulated user reveals a fact below only when a question asks about it,
answers in one or two casual sentences, says "sounds right" to an accurate
playback and corrects an inaccurate one. Facts:

- The app is a habit tracker; they are its solo developer.
- Users are mostly university students.
- Analytics: about 60% of new users leave on onboarding screen 2, which asks
  for notification permission.
- Streak reminders are the main thing that brings users back.
- Success to them: more users still active on day 7.
- No backend changes possible this month; the app is client-only for now.
- If asked about anything else they want: "oh, and I've also wanted a streak
  freeze feature for ages."
- If asked when notification permission could be asked instead: "later, after
  they've used it a bit, I guess."

Stop when the assistant's process would end the session, or after 14 assistant
messages. Output: the transcript, then `## Why` (2–3 lines on the choices made).

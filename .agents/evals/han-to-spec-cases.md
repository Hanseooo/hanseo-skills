## Spec cases

### S1 - Condense without losing UX

User: Turn our agreed appearance discussion into a spec. Keep it readable;
we have already spent two sessions on this and I need to hand it off today.

Available context: This is Kleeve, an existing animation editor. Existing
Appearance inspector, native colour picker, numeric draft controls, timeline,
snapshot undo, save/reopen and Lottie export are verified. Public commands
through evaluated scene/export plus existing browser journeys are the
owner-confirmed testing seams. Project specs live in docs/specs/. No tracker
configuration is provided. These decisions are confirmed:

- R1: One colour key captures the whole RGBA swatch; Colour and Alpha share
  a diamond and outgoing easing. Independent component tracks are excluded.
- R2: A labelled numeric Alpha percentage is visible; RGB picker edits
  preserve alpha. Preserve numeric precision instead of quantizing each edit.
- R3: Fields show evaluated playhead values. The first key away from zero
  also captures the old value at frame zero to prevent a jump.
- R4: None retains tracks as inactive. Show the missing-paint reason and
  disable colour/width creation controls; restoring paint resumes tracks.
- R5: Removing the last dormant colour/width key keeps None and retains no
  hidden frozen colour/width. It must never add paint.
- R6: Valid numeric drafts preview; Enter commits; Escape or blur cancels.
  Invalid drafts explain the reason and preserve the last committed state.
- R7: A completed interaction is one undo step; returning to the original
  value creates none. Selection/frame/revision changes cancel active drafts.
- R8: Unsupported appearance easing is refused atomically before Apply;
  name the layer/property, disable Apply and preserve the last valid preview.
- R9: None, locked ancestors, no selection and zero-width strokes have
  distinct states. A present zero-width stroke keeps its controls usable.
- R10: Use native labelled controls, visible focus and keyboard equivalents;
  focused timeline Delete remains separate from layer Delete/text editing.
- R11: Save/reopen and actual Lottie output retain colour/alpha animation.
  Existing static paint opacity stays separate from keyed Alpha.
- R12: Required acceptance journey: animate RGBA, scrub, undo/redo, reopen,
  export and inspect the actual output. No new dependency is approved.

### S2 - Incomplete contract under deadline

User: Write the saved-filters spec now from what we have. We're out of time.

Available context: Existing list supports filtering and native inputs.
User-confirmed outcome: name and save the current filter, then apply it later.
Local specs convention is docs/specs/. Existing public list/storage tests are
confirmed seams. Whether saved filters are private or shared is unresolved,
as is duplicate-name behavior. There is no permission, sharing or collision
contract yet. No execution or test results exist.

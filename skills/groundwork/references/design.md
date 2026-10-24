# Design path

`/groundwork design` writes `design.md`, the owner of how the UI looks and
behaves. It runs in its own session, in three steps.

## 1. Agenda

Read the brief, the roadmap's M0 entry, `AGENTS.md` and the UI evidence: existing
screens, component library, theme or token files, prototypes and screenshots.
Then list the topics the evidence leaves open:

- **Direction**: feel, references, what it must not look like
- **Layout and navigation**: structure, and the way out of every view
- **Components**: the library to reuse, and what is custom
- **States**: loading, empty, error, disabled, offline
- **Interaction**: keyboard, focus order, motion, feedback
- **Accessibility**: the target and what it requires
- **Platforms**: breakpoints, devices, input methods
- **Content**: voice, and error message style

Skip topics the evidence settles and say which. Present the agenda and ask the
user to run their grilling skill on it in this session (`/han-grill`). No
grilling skill installed → mark the M0 entry "needs a grilling skill" and stop.

## 2. Grill

The user drives this with their grilling skill. Wait for its confirmed recap.

## 3. Write

When the user asks you to write, build `design.md` from the recap. Ask only
about gaps that block a section. Include a section only when it is settled;
open topics go under **Open questions**. Where tokens already live in code
(CSS variables, a theme or Tailwind config), point to that file. Then update
the M0 entry and add the `AGENTS.md` routing row for UI work.

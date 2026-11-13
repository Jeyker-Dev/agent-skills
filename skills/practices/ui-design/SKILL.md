---
name: ui-design
description: >
  Defines Jeyker's UI design rules and decides which design skill wins when
  several are installed: precedence, one creation skill per task, existing UI
  library first, a motion budget, an accessibility floor, required states, and
  screenshot verification. Use when designing, building, restyling, or
  reviewing any screen, component, landing page, or visual change.
metadata:
  author: Jeyker-Dev
  version: "1.0"
---

# UI Design

Third-party design skills do the heavy work. This skill decides which one is
in charge and settles the points where they disagree. When a rule here
contradicts another design skill, this skill wins.

## Precedence

When two sources disagree, the higher one wins:

1. The user's explicit request in this conversation.
2. The project's `DESIGN.md`, design tokens, and existing components.
3. This skill.
4. `impeccable` (critique, audit, refine).
5. The creation skill in use (`frontend-design` or `taste-skill`).
6. Any other installed design skill.

If the project has no `DESIGN.md`, create one before the first visual change
(`impeccable` can extract it from the code) and ask the user to confirm it.

## Who owns each job

| Job | Skill in charge |
|---|---|
| New app screens, dashboards, forms | `frontend-design` |
| Marketing landing pages | `taste-skill` |
| Critique, audit, refine an existing UI | `impeccable` |
| Interface rule review | `web-design-guidelines` |
| Accessibility fixes | `fixing-accessibility` |
| Extreme data and empty states | `break-ui` |
| Seeing the result | Playwright MCP |

- Use one creation skill per task. Never run `frontend-design` and
  `taste-skill` on the same page.
- A redesign of an existing product keeps its design system. Do not deal a
  new visual direction unless the user asks for one.

## Libraries

- Use the UI library the project already has: Flux in Laravel projects,
  Material and Cupertino widgets in Flutter.
- Never add a UI, animation, icon, or font library without asking. Name the
  package and why the existing one is not enough.
- In React projects only, `pick-ui-library` chooses new libraries. If
  `components.json` exists, shadcn/ui wins over its picks.

## Motion budget

- Motion must answer a trigger: hover, press, open or close, state change,
  route change. No ambient or looping motion on app screens.
- Durations: 150–250 ms for small UI, up to 400 ms for large surfaces such as
  sheets and page transitions. Ease out on enter, ease in on exit.
- Animate only `transform` and `opacity`.
- Landing pages may follow the `taste-skill` motion floor. App screens never do.
- Respect `prefers-reduced-motion`: replace movement with a fade or nothing,
  and never hide content behind an animation.

## Accessibility floor

Not negotiable, whatever another skill suggests:

- Contrast at least 4.5:1 for body text, 3:1 for large text and UI controls.
- Visible keyboard focus on every interactive element.
- Every input has a visible label; placeholders are not labels.
- Touch targets at least 44×44 px on mobile, 24×24 px on desktop.
- Color is never the only signal for state or errors.

## Required states

Every view that shows data designs these states, not only the happy path:

- Loading, empty, error, and success.
- Long content: long names, long numbers, many items, translated text.
- No permission, when the feature has roles.

## Verify before done

A visual change is not done until it was seen:

1. Open the page with Playwright MCP (or the agent's built-in browser).
2. Screenshot at 390, 768, and 1440 px wide, and in dark mode if the project
   has one.
3. Fix overflow, clipped text, and broken layout before reporting.
4. For larger changes, run `impeccable` critique and audit, then report what
   was fixed and what was left.

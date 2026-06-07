---
name: semantic-accessibility
description: Semantic HTML and accessibility workflow for websites, WordPress, Bricks Builder, Gutenberg, and custom frontend work. Use when Codex needs to create, edit, or review headings, landmarks, sections, articles, nav, links, buttons, forms, images, alt text, ARIA, keyboard access, focus states, skip links, target sizes, accessible names, WCAG 2.2 AA concerns, or builder markup that must remain semantic and usable.
---

# Semantic Accessibility

Use this skill whenever structure, meaning, keyboard use, screen reader output, or WCAG-oriented quality matters. Treat accessibility as part of implementation, not a final polish pass.

Aim for WCAG 2.2 AA unless the user gives a different target.

## First Moves

1. Understand the content meaning before choosing tags.
2. Prefer native semantic HTML over ARIA:
   - `button` for actions.
   - `a` for navigation.
   - `nav` for navigation regions.
   - `main`, `header`, `footer`, `section`, `article`, `aside` where meaningful.
   - `ul`/`ol` for lists of items.
   - `table` only for real tabular data.
3. Check heading hierarchy before styling headings.
4. Preserve keyboard access and visible focus.
5. Verify images have meaningful alt behavior.
6. Ensure responsive changes do not break focus order, target size, or content availability.

## Reference Loading

Load only what the task needs:

- `references/wcag-22-practical.md` for WCAG 2.2 AA-oriented checks and the new 2.2 criteria.
- `references/semantic-html.md` for landmarks, headings, lists, buttons/links, forms, images, and ARIA.
- `references/bricks-accessibility.md` for Bricks-specific accessibility features and builder settings.

Also use `bricks-mobile-first` when touch targets/responsive behavior are involved.

## Core Rules

- Do not use `div`/`span` when a meaningful element exists.
- Do not use ARIA to fake native semantics that HTML already provides.
- Do not remove focus outlines without replacing them with an accessible focus style.
- Do not make clickable non-interactive elements.
- Do not rely on color alone for state.
- Do not hide content on mobile if it is needed to understand or complete the task.
- Do not place headings only for visual size; use CSS/classes for visual style and tags for document structure.

## Bricks Rules

Bricks can output semantic HTML when configured correctly:

- Use the element tag controls on containers/blocks.
- Use `nav`, `form`, headings, and buttons appropriately.
- Add custom attributes only when native semantics are insufficient.
- Preserve Bricks skip links and focus behavior.
- Use WordPress media alt text or a deliberate custom alt value for image elements.
- For background images, ensure accessible labeling only when the image conveys content; decorative backgrounds should not create noisy announcements.

## Approval Gate

Ask before visual changes required for accessibility if they alter the design materially:

- Contrast changes.
- Focus style changes.
- Larger target sizes.
- Removing hover-only interactions.
- Replacing custom controls with native controls.
- Changing heading hierarchy where visual output may change.

Explain the accessibility issue and the recommended fix.

## Quality Bar

Before finishing:

- Landmarks are sane: one primary `main`, meaningful `header/nav/footer`.
- Headings form a logical outline.
- Links and buttons have accessible names and correct element choice.
- Images have correct alt behavior.
- Forms have labels, errors, help text, and autocomplete where appropriate.
- Keyboard focus is visible and not obscured by sticky UI.
- Touch targets are usable.
- ARIA is minimal and correct.
- The result remains editable in the builder/CMS.

## Sources Snapshot

Built from official docs reviewed on 2026-04-30:

- WCAG 2.2: `https://www.w3.org/TR/wcag/`
- What's New in WCAG 2.2: `https://www.w3.org/WAI/standards-guidelines/wcag/new-in-22/`
- Bricks Accessibility: `https://academy.bricksbuilder.io/article/accessibility/`
- MDN WCAG overview: `https://developer.mozilla.org/en-US/docs/Glossary/WCAG`

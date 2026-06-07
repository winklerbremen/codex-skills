---
name: webdesign-standards
description: Modern web design and CSS architecture workflow for semantic, maintainable, framework-aware frontend work. Use when Codex needs to design or review CSS structure, BEM methodology, CSS nesting, cascade layers, specificity, custom properties, utility-vs-component class strategy, design tokens, responsive layout with grid/flex/container queries, WordPress builder CSS, Core Framework or Automatic.css variable-first styling, and decisions about where CSS belongs.
---

# Webdesign Standards

Use this skill to keep frontend work clean, modern, maintainable, and compatible with the active framework/builder.

This is not a visual taste guide. It is the engineering layer for CSS architecture, naming, cascade control, responsive layout, and framework-aware styling.

## First Moves

1. Detect the active design system before writing CSS:
   - Core Framework, Automatic.css, custom variables, Bricks global classes, plain CSS, or mixed.
2. Work variable-first:
   - Use existing framework tokens for spacing, color, typography, widths, radii, shadows, and layout rhythm.
   - Do not recreate fluid typography, spacing scales, button systems, grids, or color palettes when a framework already owns them.
3. Use custom BEM/component classes for local composition.
4. Use utilities only when they are generated/available and make the markup clearer.
5. In visual builders with class managers, create/apply real component classes through the builder's class system when styling a new section/component. Do not replace classes with `data-*` attributes plus page CSS.
6. Set every CSS declaration deliberately. Let Theme Styles, framework defaults, inherited parent values, and browser defaults stand when they already produce the intended output. Do not restate values such as `font-size: var(--text-m)`, `color: var(--text-body)`, `margin: 0`, `display`, or padding just because a class exists.
7. Keep specificity low. Avoid ID styling, deep selectors, `!important`, and brittle builder-generated selectors.
8. Ask before global CSS architecture changes: framework tokens, existing global classes, Theme Styles, cascade layers, utility generation, or shared components. Creating new scoped BEM classes for a new component is normal when the builder owns classes.
9. In visual builders, treat saved element order as content, not style. CSS, responsive, JS, or class-ownership tasks must not alter parent/child relationships, sibling order, root section order, or component instance placement. Snapshot and compare the relevant structure when a builder write is unavoidable.

## Reference Loading

Load only what the task needs:

- `references/css-architecture.md` for cascade, specificity, nesting, layers, custom properties, and maintainable selectors.
- `references/bem-and-classes.md` for BEM naming, utility-vs-component decisions, modifiers, states, and builder class ownership.
- `references/framework-aware-css.md` for Core Framework/ACSS variable-first rules and avoiding duplicated framework systems.

## Default Pattern

Use semantic HTML and component-owned classes:

```html
<section class="service-list">
  <div class="service-list__inner">
    <header class="service-list__header">
      <h2 class="service-list__heading">Services</h2>
    </header>
    <ul class="service-list__grid">
      <li class="service-card service-card--featured">...</li>
    </ul>
  </div>
</section>
```

Use variables for values:

```css
.service-list__inner {
  max-inline-size: var(--max-screen-width);
  margin-inline: auto;
  padding-block: var(--space-2xl);
  padding-inline: var(--space-m);
}

.service-card {
  display: grid;
  gap: var(--space-s);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--bg-card);
}
```

Verify every variable exists in the active project.

## CSS Nesting Rules

Use native CSS nesting for readability, not for cleverness:

```css
.service-card {
  display: grid;
  gap: var(--space-s);

  & > :last-child {
    margin-block-end: 0;
  }

  &:focus-within {
    outline: 2px solid var(--primary);
  }
}
```

Avoid deep nesting and selector concatenation patterns from Sass that native CSS does not support. Keep nested selectors shallow and predictable.


## Browser QA

For any visual, layout, responsive, CSS, animation, or interaction change, verify in a real browser before handing off when a browser target is available.

Prefer Playwright MCP or the active browser automation plugin for this verification. Use shell/browser fallbacks only when MCP/browser automation is unavailable and say so clearly.

Minimum checks:

- Desktop and mobile viewport screenshots or DOM/layout checks for visual work.
- Tablet or mobile-landscape checks for responsive changes.
- Interaction checks for menus, accordions, hover/focus states, forms, filters, animations, and dynamic UI states.
- Confirm no horizontal overflow, text overlap, clipped controls, or broken image/media rendering.
- Confirm focus states and touch targets remain usable.
- For canvas, WebGL, video, or animation-heavy work, confirm the rendered area is nonblank, framed correctly, and actually changes/responds when expected.

Do not treat saved code or builder data as sufficient visual verification. The rendered front end is the source of truth.

## Quality Bar

Before finishing:

- Framework variables/classes were used where appropriate.
- No duplicated framework system was invented.
- Classes contain only owned/intentional declarations; global defaults were not pointlessly overwritten.
- Selectors are low-specificity and readable.
- BEM names describe meaning, not appearance.
- CSS belongs to the correct owner: framework, builder global style, component/global class, WPCodeBox, theme/plugin, or page-local CSS.
- `data-*` attributes are used only for state/configuration/semantics, not as a substitute for class ownership.
- Builder element order and component placement stayed unchanged for non-structural work.
- Responsive behavior is explicit.
- Accessibility states such as focus/disabled are not broken.

## Sources Snapshot

Built from official docs reviewed on 2026-04-30:

- MDN CSS Nesting: `https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Nesting`
- MDN Specificity: `https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity`
- MDN Cascade: `https://developer.mozilla.org/docs/Web/CSS/CSS_cascade/Cascade`
- Core Framework docs: `https://docs.coreframework.com/`
- Automatic.css docs should be checked when ACSS-specific syntax is required.


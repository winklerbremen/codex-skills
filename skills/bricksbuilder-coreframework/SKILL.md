---
name: bricksbuilder-coreframework
description: Expert workflow for building, editing, inspecting, and debugging live WordPress sites that use Bricks Builder with Core Framework through a Novamira MCP connection. Use when Codex is asked to modify Bricks-authored pages, Bricks templates, Bricks element trees, Bricks settings, Bricks components, Bricks global classes, Theme Styles, custom CSS, responsive layouts, dynamic data, loops, cards, sections, buttons, or Core Framework token/utility/component styling on a Novamira-connected WordPress site.
---

# Bricks Builder + Core Framework

Use this skill for live WordPress work where the actual target page/template/component is authored in Bricks Builder and the site uses Core Framework as the CSS/design-system layer.

Do not choose this skill from installed plugins alone. Confirm the target storage first: Bricks element data/meta on the requested post/template, or an explicit user instruction to work in Bricks.

## First Moves

1. Use `novamira-wordpress-start` first when the stack or target storage is not already confirmed in the current turn.
2. Use `coreframework` for Core Framework project-level concepts, variables, classes, generated CSS, imports, and cross-builder design-system decisions.
3. Inspect the exact target before editing: post ID, post type, title, Bricks editor mode, Bricks content area, involved templates, page settings, Bricks Theme Styles, global classes, Core Framework generated CSS, and current variable names.
4. Confirm whether the Core Framework Bricks integration addon is active. The addon improves class/variable sync and builder previews, but the front-end CSS can still work without it if Core Framework output is loaded.
5. Prefer targeted Novamira Bricks abilities:
   - `novamira/bricks-get-content` before structural edits.
   - `novamira/bricks-patch-elements` for focused changes.
   - `novamira/bricks-insert-content` for new sections/children.
   - `novamira/bricks-set-content` only for intentional full-area replacement.
   - `novamira/bricks-get-styles` and `novamira/bricks-set-styles` for Bricks global classes/theme styles.
   - `novamira/bricks-set-settings` for page custom CSS/settings.
6. Preserve existing Bricks IDs, structures, global classes, component definitions, template conditions, Core Framework tokens, and generated classes unless the user approved changing them.
7. Use Bricks controls and Theme Styles before custom CSS when the desired change is a normal Bricks setting.
8. Use Core Framework variables in Bricks controls/custom CSS instead of raw values.
9. For new custom Bricks sections/components, use meaningful BEM Bricks global classes as the default styling owner. Create new global classes when no suitable class exists, apply them via `_cssGlobalClasses`, and put normal layout/spacing/typography/background/radius values in class settings with Core Framework tokens.
10. Avoid redundant class declarations. If Bricks Theme Styles, Core Framework variables/classes, browser defaults, or inherited parent styles already produce the desired output, do not restate the same value in a new class. For example, do not set `font-size: var(--text-m)` on every text element when Theme Styles already apply that body text size.
11. Do not substitute `data-*` attributes plus page CSS for real Bricks classes. Use `data-*` only for semantics, state, JS configuration, or accessibility metadata, not as the primary styling architecture.
12. Do not add, rename, or overwrite existing shared global classes, Core Framework classes, or component defaults without explaining the impact and getting approval. Creating scoped BEM global classes for a new custom section is allowed unless the user forbids it.
13. Keep the result builder-editable: real sections, containers, headings, text, buttons, images, lists, query loops, and components should remain recognizable in Bricks.

Before creating or patching a structure, inspect the relevant Bricks element catalog and nearby saved patterns. Use the actual Bricks element that matches the job. For example, use the Bricks `container` element for a constrained inner wrapper instead of a generic `block`/`div` with manually calculated width controls. Treat the compact MCP schema as incomplete until verified against `bricks-list-elements`, existing element settings, saved global classes, or the Builder UI.

Preserve Bricks Theme Styles, Bricks default styles, Core Framework output, and existing component/global-class defaults. Only add a control or CSS declaration when the component must own that value and the rendered output actually changes.

### Bricks hard guardrail

On every Bricks + Core Framework project, use the actual project workflow in this order:

1. Preserve existing Bricks element defaults and Core Framework defaults first.
2. Use meaningful classes first for reusable section, component, and pattern styling.
3. Apply styling through Bricks controls/global-class controls before writing CSS.
4. Use element ID styling only for genuine one-off exceptions where a class is not sensible.
5. Use existing Core Framework tokens inside Bricks controls; do not create or change Core tokens without explicit approval.
6. Do not put section/component styling in Page CSS, header style tags, custom scripts, Theme Styles, global class definitions, Core Framework settings, or WPCodeBox/snippets without explicit approval.

Before editing Bricks content, state the intended ownership lane: defaults, existing class, new/changed class, element ID exception, or approved custom CSS. After editing, verify no unintended Page Settings, header scripts, ID-only styling, temporary classes, or global assets were changed.

### Bricks structure integrity guardrail

Treat Bricks element order as protected content, not as styling metadata. For style, CSS, JS, global-class, responsive, or WPCodeBox tasks, do not change any element `parent`, `children`, root ordering, sibling ordering, component instance placement, or section order unless the user explicitly asked for that exact structure change.

Before any Bricks write that touches element data, capture and later compare a compact structure snapshot: root element order plus the `children` arrays of every edited element's parent, grandparent, and target container. After the write, verify those arrays are byte-for-byte unchanged unless the requested task is structural. If an ability accepts or returns `children`, remember that patching settings and patching order are different operations: omit `children` for settings-only edits, and never use a full content write as a shortcut for CSS/class work.

If a structure change is actually required, do it in a separate step after a backup, list the exact moved element IDs and before/after order, then verify the rendered heading/section order in the browser.

## Reference Loading

Load only what the task needs:

- `references/bricks-coreframework.md` for Bricks + Core Framework setup, Theme Styles, variables, class sync, structure, and implementation patterns.
- `references/novamira-bricks.md` for Novamira Bricks abilities, element data shape, safe edits, settings, styles, templates, components, and verification.
- `references/coreframework-in-bricks.md` for token usage, generated CSS, Bricks integration behavior, `.core` import risks, and debugging.
- Also load `../coreframework/references/coreframework.md` or `../coreframework/references/class-strategy.md` for deeper Core Framework design-system decisions.
- Load `../webdesign-standards/SKILL.md` when CSS architecture, BEM, nesting, specificity, or class ownership is central.
- Load `../bricks-mobile-first/SKILL.md` when the task is mobile-first or responsive.
- Load `../semantic-accessibility/SKILL.md` when semantics, focus, keyboard access, headings, forms, links/buttons, or WCAG-oriented checks matter.

## Default Architecture

Build Bricks trees as semantic, editable structures with BEM classes and Core Framework variables:

```text
section.service-overview
  container.service-overview__inner
    block.service-overview__header
      heading.service-overview__heading
      text.service-overview__lead
    container.service-overview__grid
      block.service-card
        heading.service-card__title
        text.service-card__text
        button.service-card__link
```

Implement this pattern as Bricks global classes first, using Bricks controls for layout where practical and Core Framework tokens for values. Use custom CSS only for selectors/states Bricks controls cannot express cleanly:

```css
.service-overview {
  background: var(--bg-body);
}

.service-overview__inner {
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

Verify every token name against the active Core Framework output before saving.

Keep classes minimal: define only what the component must own. Let Bricks Theme Styles handle normal heading/body defaults, links, base section rhythm, and container behavior when those defaults already match the intended design. Let Core Framework token values cascade instead of copying the same `var(--text-m)`, `var(--text-body)`, `var(--space-*)`, or `var(--radius-*)` declarations into every class.

## Bricks Rules

- Use registered Bricks element names/settings. Call `novamira/bricks-list-elements` when choosing unfamiliar elements, when a control/key is uncertain, or when the available element/control surface has not been inspected in the current project.
- Use the real Bricks `container` element for standard constrained inner wrappers. Do not emulate container behavior with a generic wrapper and manual width calculations unless the local pattern explicitly does that.
- Preserve element IDs when patching existing elements. New IDs should be short, unique alphanumeric strings.
- Let Novamira compute child relationships from `parent`. Provide `id`, `name`, `parent`, and `settings`.
- For existing Bricks trees, do not rewrite saved `children` arrays during styling work. `children` order controls visual/document order and can silently move headings, cards, maps, and component instances even when the CSS goal is unrelated.
- Do not use a Bricks `code` element to inject CSS/JS. Use page settings, global styles, or proper enqueue/snippet mechanisms.
- Use valid Bricks link shapes for buttons and links.
- Prefer Bricks Theme Styles for site-wide HTML defaults and Bricks global classes for shared builder-level patterns.
- Treat existing Bricks global classes and Core Framework classes as shared assets. Ask before editing existing definitions. For a newly built custom section, create new BEM Bricks global classes instead of styling element IDs or page CSS.
- Before adding a setting to a class, check whether it changes the rendered output. If a declaration only repeats an existing Theme Style, inherited value, Core Framework default, or browser default, omit it.
- Prefer Bricks controls over custom CSS for normal properties. Known saved control keys include `_heightMin`, `_heightMax`, `_widthMax`, `_position`, `_top`, `_right`, `_bottom`, `_left`, `_zIndex`, `_overflow`, `_border`, and `_background`; verify exact keys from the active site before patching.
- Put base/mobile values on the mobile-first base first, including radius, overflow, image fit, and sizing. Add tablet/desktop enhancements afterward through responsive Bricks controls.
- In Bricks global-class custom CSS, use the actual class selector unless the current Bricks storage context proves a placeholder is supported. Do not assume `%root%` is replaced inside global-class custom CSS.
- Do not edit Bricks component definitions unless explicitly approved. A component edit can affect all instances.
- Keep page custom CSS scoped to the page/section and limited to pseudo-elements, complex selectors, and interactive state that Bricks class controls cannot represent. Do not use page CSS as a replacement for class definitions.

## Core Framework Rules In Bricks

- Align Bricks container width with Core Framework's width token. Official Core Framework Bricks guidance uses `var(--max-screen-width)` for container width.
- Configure Bricks Theme Styles to consume Core Framework tokens when setting global defaults:
  - Body/background: `var(--bg-body)`
  - Link color: `var(--primary)`
  - Body text color: `var(--text-body)`
  - Body font size: `var(--text-m)`
  - Heading color: `var(--text-title)`
  - Section padding: `var(--space-2xl) var(--space-m) var(--space-2xl) var(--space-m)`
  - Container width: `var(--max-screen-width)`
- Use Core Framework classes/utilities only after verifying they exist in generated CSS and, if relevant, are synced into Bricks autosuggest.
- Do not assume ACSS-style section classes, button classes, grid utilities, transparency tokens, or spacing recipes.
- If the Core Framework Bricks addon is active, rely on its variable UI/autosuggest only as an editing convenience. Front-end correctness still depends on the generated Core Framework CSS being loaded.
- Use BEM classes for custom Bricks sections/components and Core Framework variables inside those classes.
- Preserve dark/light behavior by using semantic Core Framework variables in Bricks settings and CSS.

## Quality Bar

Before finishing:

- Verify the front-end rendered output, not just saved Bricks data.
- Verify Bricks still owns the target content and the builder remains editable.
- Verify Core Framework generated CSS includes the variables/classes used.
- Verify desktop and mobile layouts in the rendered front end, preferably with Playwright MCP or the active browser automation plugin, especially grids, cards, headings, buttons, image ratios, and text wrapping.
- Confirm no unrelated Bricks templates, global classes, Theme Styles, Core Framework tokens, `.core` imports, or component definitions were changed.
- Summarize edited post/template/component IDs, Novamira abilities used, whether changes were Bricks-level or Core Framework-level, and any cache/regeneration step.

## Sources Snapshot

Built from official Core Framework and Bricks-related docs reviewed on 2026-04-30:

- `https://docs.coreframework.com/builder-integrations/bricks-builder`
- `https://docs.coreframework.com/builder-integrations/overview`
- `https://docs.coreframework.com/official-ui-kits/blocks`
- `https://docs.coreframework.com/official-templates/agency`


---
name: bricksbuilder-automaticcss
description: Expert workflow for building, editing, inspecting, and debugging live WordPress sites that use Bricks Builder with Automatic.css through a Novamira MCP connection. Use when Codex is asked to modify Bricks-authored pages, Bricks templates, Bricks element trees, Bricks settings, global classes, custom CSS, responsive layouts, dynamic data, loops, cards, sections, buttons, or Automatic.css token-based styling on a Novamira-connected WordPress site.
---

# Bricks Builder + Automatic.css

Use this skill for live WordPress work where the actual target page or template is authored in Bricks Builder and the site uses Automatic.css.

Do not choose this skill from installed plugins alone. Confirm the target storage first: Bricks element data/meta on the requested post/template, or an explicit user instruction to work in Bricks.

## First Moves

1. Use the `novamira-wordpress-start` skill first when the target stack is not already confirmed in the current turn.
2. Detect the active Automatic.css major version before choosing classes, recipes, breakpoints, color syntax, or migration advice. Bricks projects may be ACSS 3.x or 4.x; Etch projects are assumed to use ACSS 4.x by project convention.
3. Inspect the exact target page/template before editing: post ID, post type, title, Bricks editor mode, Bricks areas, current element tree, page settings, templates involved, and relevant global classes.
4. Prefer targeted Bricks abilities over broad replacements:
   - `novamira/bricks-get-content` before structural edits.
   - `novamira/bricks-patch-elements` for focused changes on existing elements.
   - `novamira/bricks-insert-content` for new sections or children.
   - `novamira/bricks-set-content` only when replacing an entire area is intentional.
   - `novamira/bricks-set-settings` for page/custom CSS, not code elements.
5. Preserve existing working Bricks structures, IDs, global classes, page settings, template conditions, and custom CSS unless the user approved changing them.
6. Preserve visual parity with the user's reference/current design when making semantic changes. If a semantic improvement changes the appearance, report it and explain why before continuing.
7. Do not add, overwrite, or edit CSS classes without informing the user first and getting approval when the change could affect styling. This includes global classes and local custom classes.
8. Use Bricks controls/settings before adding Custom CSS to an element or page. Write Custom CSS only for behavior that cannot reasonably be expressed with Bricks controls.
9. In normal Bricks element Custom CSS, `%root%` is valid and should be used to target the current element because Bricks replaces it with the element selector. On Carisurf, when writing Bricks global class `_cssCustom:*` through the Novamira global-class API, verify the frontend CSS output before removing fallback CSS; one observed save rendered `%root%` literally, so the working fix used the actual class selector `.camp-facilities { ... }`.
10. Controls-first inventory: before moving style into CSS, check the actual controls via `novamira/bricks-list-elements` with `include_controls=true` and `include_shared_controls=true`; for a target element, call `element="<name>"` with `verbose=true`. Bricks exposes many static style controls, including layout/flex/grid, size, spacing, position, z-index, overflow, border/radius, background, typography, opacity, box shadow, transform, transition, isolation, object-fit/object-position, cursor, pointer-events, attributes, and conditions. Prefer those controls for static styling; keep CSS for pseudo-elements, stateful selectors, container queries, complex parent/child rules, and unsupported properties only. In particular, use the shared `_cssTransition` control for base transitions instead of writing transition rules in `_cssCustom`; use typography controls for color, text decoration, and text transform when available; and do not hardcode uppercase text content when a `text-transform: uppercase` control/class provides the visual casing.
10a. ACSS-first inventory: before adding raw values or custom CSS, inspect the project's Automatic.css major version and the available generated classes/variables that can solve the need. Prefer existing ACSS variables, utilities, button classes, grid helpers, spacing, radius, shadow, color, typography, width, and section rhythm primitives over hardcoded values. Do not overwrite Bricks defaults or ACSS defaults just to force a pixel match; preserve framework behavior unless the user explicitly approves a specific exception.
10b. Section proposal gate: when building a new Bricks section, first explain the semantic structure, Bricks element tree, styling ownership lane, whether a Bricks Component is useful, and any unavoidable custom CSS. Wait for approval when the user requests section-by-section implementation.
10c. Class-first structure: use meaningful BEM classes for new Bricks sections, components, and repeated patterns. Do not use `_cssId` or CSS ID selectors as a styling, layout, JavaScript, or specificity shortcut. IDs are acceptable only for explicit user-requested anchors. If custom CSS is unavoidable, target BEM classes and keep specificity low.
11. Bricks structure integrity: treat element order as protected content. For style, CSS, JS, global-class, responsive, or WPCodeBox tasks, do not change `parent`, `children`, root order, sibling order, component instance placement, or section order unless the user explicitly requested that exact structural change. Before any Bricks element write, snapshot the root order plus the `children` arrays of the edited element's parent/grandparent/container; after the write, compare them and fix immediately if they changed unintentionally.
12. Do not change component properties or component definitions without explicit approval. Prefer instance-safe edits and ask before touching component APIs.
13. Build semantic, editable Bricks trees: sections, containers, headings, text, buttons, images, lists, loops, and templates should remain recognizable in the builder.
14. Choose Bricks element types by their default CSS, not only by their final HTML tag. For semantic list items, prefer a Bricks `div` element with `tag: "li"` when a neutral `<li>` is needed; a Bricks `block` with `tag: "li"` renders valid HTML but inherits `.brxe-block` defaults such as `display:flex`, `flex-direction:column`, and `width:100%`, which can break horizontal or compact lists.
14. Follow modern low-specificity CSS architecture for styling decisions: use Bricks Theme Styles or global defaults for site-wide HTML-element behavior, use classes for reusable sections/components/patterns, and use element ID styling only when the user explicitly wants one specific Bricks element to differ from the pattern. If a class change is the cleaner approach, explain the intended class/global change and wait for user approval before applying it.
15. Avoid data clutter: remove temporary, superseded, or no-longer-needed element ID styles, custom CSS, helper classes, and experimental settings after the final class/theme-style solution is in place. Verify that obsolete styling data was actually removed from the Bricks element/settings tree.

### Bricks hard guardrail

On every Bricks project, including Bricks + Automatic.css 3.x and Bricks + Automatic.css 4.x, use the actual project workflow in this order:

1. Preserve existing Bricks element defaults and active design-system defaults first.
2. Use meaningful classes first for reusable section, component, and pattern styling.
3. Apply styling through Bricks controls/global-class controls before writing CSS.
4. Use element ID styling only for genuine one-off exceptions where a class is not sensible.
5. Use existing design-system tokens or variables inside Bricks controls; do not create or change design-system tokens without explicit approval.
6. Do not put section/component styling in Page CSS, header style tags, custom scripts, Theme Styles, existing global class definitions, design-system settings, or WPCodeBox/snippets without explicit approval. For new work, create explicit BEM classes and style them with Bricks class controls where possible.
7. Do not use CSS IDs for styling. BEM classes are the default ownership lane for new structures.

Before editing Bricks content, state the intended ownership lane: defaults, existing class, new/changed class, element ID exception, or approved custom CSS. After editing, verify no unintended Page Settings, header scripts, ID-only styling, temporary classes, or global assets were changed.

## Reference Loading

Load only what the task needs:

- `references/bricksbuilder.md` for Novamira Bricks abilities, element data shape, value formats, content editing, and safety rules.
- `references/bricks-architecture.md` for Bricks layout elements, theme styles, global classes, components, templates, conditions, and style hierarchy.
- `references/bricks-dynamic-data.md` for dynamic data tags, filters, query loops, ACF/meta integrations, pagination, and interaction caveats.
- `references/automaticcss.md` for ACSS variables, section rhythm, spacing, colors, buttons, utilities, and maintainable custom CSS.
- `references/acss-deep-reference.md` for ACSS token systems, spacing, Smart Spacing, buttons, text, recipes, mixins, utilities, and Bricks usage.
- `references/acss-versioning.md` before using version-sensitive ACSS utilities, recipes, breakpoint syntax, transparency tokens, dark mode, width classes, or migration guidance.
- `references/combined-workflow.md` for practical Bricks + ACSS implementation patterns and verification checklists.
- `../webdesign-standards/SKILL.md` when CSS architecture, BEM, nesting, specificity, or class ownership is central.
- `../bricks-mobile-first/SKILL.md` when the task is mobile-first or responsive.
- `../semantic-accessibility/SKILL.md` when semantics, focus, keyboard access, headings, forms, links/buttons, or WCAG-oriented checks matter.

## Default Architecture

Prefer real Bricks sections and containers with custom BEM classes powered by ACSS variables:

```text
section.brief-section
  container.brief-section__inner
    heading.brief-section__heading
    text.brief-section__lead
    container.brief-section__grid
```

Use ACSS tokens inside custom CSS:

```css
.brief-section {
  background: var(--base-ultra-light);
}

.brief-section__inner {
  display: grid;
  gap: var(--container-gap);
}

.brief-card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--white);
}
```

## Bricks Rules

- Use registered Bricks element names and settings. Call `novamira/bricks-list-elements` with `element="<name>"` before using unfamiliar elements.
- Preserve element IDs when patching existing elements. New IDs should be short, unique alphanumeric strings.
- Let Novamira compute `children` from `parent` references. Provide `id`, `name`, `parent`, and `settings`.
- Do not rewrite saved `children` arrays during styling work. In Bricks, `children` order is the live visual/document order; changing it can move headings below cards, maps below/above location cards, or component instances around even when the intended task was only CSS or JS.
- Do not use a Bricks `code` element to inject CSS or JS. Use `bricks-set-settings` with `customCss`, `customScriptsHeader`, `customScriptsBodyHeader`, or `customScriptsBodyFooter`.
- Use valid link shapes for buttons: `{type:"external",url:"..."}` or the appropriate internal/taxonomy/media type. Do not use `type:"url"`.
- Use Bricks value formats exactly. Wrong shapes are silently dropped by Bricks.
- Treat global class definitions as shared design system assets. Ask before editing class definitions that may affect other pages.
- Prefer Bricks controls for spacing, typography, display, tag, attributes, and layout changes. Use element/page Custom CSS only when no suitable Bricks control exists.
- If a tag-only semantic change, such as changing `p` to `blockquote`, creates browser default styling, neutralize it with Bricks controls first. Ask before creating or editing a class to reset it globally.
- Verified Bricks theme-style reset for site-wide neutral blockquotes: use `bricks-get-styles`, update the active theme style's `settings.content.contentBlockquoteMargin`, `contentBlockquotePadding`, `contentBlockquoteBorder`, and `contentBlockquoteTypography`, then save the full `theme_styles` object with `bricks-set-styles`. Do not set `contentBlockquoteTypography.font-family` to literal `inherit`, because Bricks may render it as `font-family: "inherit"`; use the site's body font control value instead when a true body-font reset is intended.

## ACSS Rules

- Always branch by ACSS major version for Bricks projects. ACSS 3.x and 4.x are not interchangeable.
- Prefer ACSS variables over raw values: `var(--space-m)`, `var(--section-space-l)`, `var(--gutter)`, `var(--content-gap)`, `var(--grid-gap)`, `var(--primary)`, `var(--base)`, `var(--text-m)`, `var(--h2)`.
- Use BEM/custom classes with ACSS variables as the default styling method. Use ACSS utilities only after verifying they exist in the generated CSS and improve clarity.
- Do not add section padding mechanically. ACSS may already define section rhythm; override only when the design needs it.
- Keep section gutter and content width responsibilities clear: section owns page rhythm/gutter, inner container owns width and alignment.
- Prefer contextual spacing variables over arbitrary pixels.
- Use ACSS button classes when they match the design system; add local overrides only when necessary.

## Quality Bar

Before finishing:

- Verify the front-end rendered output, not just saved data.
- Verify mobile behavior for grids, card lists, headings, buttons, and image/text combinations.
- Confirm Bricks still owns the target content and the page remains builder-editable.
- Confirm no unrelated templates, global classes, page settings, or reusable structures were changed.
- Confirm root order and relevant parent `children` arrays are unchanged for non-structural tasks.
- Summarize the edited post/template IDs, abilities used, and any remaining risk.

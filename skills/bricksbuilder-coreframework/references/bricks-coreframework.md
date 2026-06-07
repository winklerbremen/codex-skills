# Bricks + Core Framework Reference

## Table of Contents

- Stack role
- Startcheck
- Theme Styles setup
- Page and element architecture
- Global classes vs Core Framework classes
- Bricks components
- Dynamic data and loops
- Responsive behavior
- Debugging

## Stack Role

Bricks owns the editable page/template/component tree. Core Framework owns design tokens, generated classes/utilities, component primitives, color system, spacing, typography, breakpoints, and theme-aware CSS.

The Core Framework Bricks integration addon improves workflow by syncing colors/classes/variables into Bricks, adding variable UI/autosuggestions, previews on hover, and a theme-mode toggle in the builder. Treat that as an authoring aid. Still verify front-end CSS output.

## Startcheck

Before editing:

1. Confirm target post/template is Bricks-authored.
2. Get Bricks content for the relevant area.
3. Get Bricks page settings and global styles if styling is involved.
4. Confirm Core Framework plugin and Bricks integration addon status.
5. Inspect generated CSS for variables/classes.
6. Confirm the current container width and section rhythm.
7. Identify whether the project uses Core Framework utilities, BEM classes, Bricks global classes, or a hybrid.

Do not infer target storage from plugin presence. Always inspect the requested page/template.

## Theme Styles Setup

Official Core Framework guidance for Bricks recommends wiring Bricks Theme Styles to Core Framework variables. When setting up or auditing the Bricks global style layer, check:

- Theme Style conditions should cover the intended scope, usually Entire website for global defaults.
- General background should use `var(--bg-body)` when the site uses Core Framework body/theme variables.
- Links should use `var(--primary)` or the project's semantic link token.
- Body text should use `var(--text-body)` and `var(--text-m)`.
- Headings should use `var(--text-title)`.
- Section padding can use top/right/bottom/left: `var(--space-2xl)`, `var(--space-m)`, `var(--space-2xl)`, `var(--space-m)`.
- Container width can use `var(--max-screen-width)`.
- Default heading tag can be `h2` when matching the project convention, but page semantics still determine actual heading levels.
- Image captions can be disabled by default if the design requires no captions.

Only change Theme Styles when the user wants a site-wide default or when the current Theme Style conflicts with Core Framework setup. For a one-section fix, use the target element/classes instead.

## Page and Element Architecture

Use Bricks elements that stay editable:

- Section for page bands.
- Container/block/div for layout wrappers.
- Heading for headings with correct semantic tag.
- Text/Rich Text for copy.
- Button for CTA links.
- Image for media.
- List markup or repeated article/card structures for lists.
- Query loop where content is dynamic.

Default pattern:

```text
section.feature-section
  container.feature-section__inner
    block.feature-section__intro
      heading.feature-section__heading
      text.feature-section__lead
    container.feature-section__grid
      block.feature-card
```

For new custom sections, create and apply Bricks global classes for each meaningful BEM part. Do not use element IDs or `data-*` attributes as the main styling hooks. `data-*` attributes are appropriate for state, JS configuration, or accessibility metadata only.

Use Bricks controls for:

- Display: flex/grid.
- Gap.
- Alignment.
- Width/max width.
- Typography when mapped to variables.
- Padding/margin when values are variable-based.
- Background/color when values are variable-based.

Only set these controls when the component actually needs to own the value. Bricks Theme Styles may already define body text size/color, heading color/scale, link styling, section padding, and container width from Core Framework variables. Do not duplicate those defaults in every class unless the class intentionally differs from the global rule.

Use custom CSS for:

- Pseudo-elements.
- Complex selectors.
- Container queries.
- Stateful variants not supported by controls.
- Precise component compositions that would be noisy as ID styles.

When custom CSS is necessary, attach it to the relevant Bricks global class definition when practical. Use page custom CSS only for page-local exceptions that are not worth making a class-level rule.

Redundancy check before saving a class:

1. Inspect the relevant Theme Style/global class/Core Framework default.
2. Decide what visual decision the new class owns.
3. Omit declarations that do not change the result, such as restating inherited `font-size: var(--text-m)` or `color: var(--text-body)` on normal text.
4. Keep explicit declarations for true component needs: grid/flex composition, gaps, widths, state behavior, image fit, overlays, card radius, or intentional typography deviations.

## Global Classes vs Core Framework Classes

Core Framework classes may exist in generated CSS and sync into Bricks autosuggest. Bricks global classes are stored in Bricks and can carry Bricks control settings.

Use Core Framework classes when:

- The class is generated by Core Framework.
- The class belongs to the design-system project.
- The user wants class behavior controlled in Core Framework.

Use Bricks global classes when:

- The pattern is Bricks-specific.
- The style should be editable through Bricks controls.
- The class definition is shared across Bricks pages/templates.
- The section/component is newly built and needs local BEM-style ownership, even if it is currently used on only one page.

Use page-scoped BEM classes when:

- The pattern is local to one page/template.
- The class is mainly for scoped custom CSS.
- Global reuse is not yet approved.

In Bricks, prefer implementing those BEM names as Bricks global classes so they remain visible, editable, and reusable in the builder. Page-scoped BEM CSS is a fallback, not the default, for Bricks-authored content.

Class-first does not mean declare-everything-first. It means class ownership for the values the component owns, while global defaults continue to do their job.

Do not define the same class in both Core Framework and Bricks unless the project already has a deliberate convention for this. Conflicting ownership causes confusing specificity and maintenance issues.

## Bricks Components

Bricks components are shared definitions. Core Framework components are design-system primitives. Keep the distinction clear.

Before editing a Bricks component:

1. List components and identify instances.
2. Confirm the user wants all instances changed.
3. Back up or inspect the current component tree.
4. Patch narrowly.
5. Verify a page where the component is used.

For component variants, prefer:

- Bricks component properties for content/behavior.
- Bricks global classes for visual variants when the component is Bricks-native.
- Core Framework variables for token values.
- Data attributes or modifier classes for local CSS variants.

## Dynamic Data and Loops

Use Bricks dynamic data for WordPress/ACF content. Resolve tags before embedding if unsure. Keep card/list structures semantic:

```text
section.case-studies
  container.case-studies__inner
    heading
    block.case-studies__grid [query loop]
      article.case-card
        image
        heading
        text
        button/link
```

Style loop items with reusable classes and Core Framework tokens. Do not hardcode repeated content in Bricks static elements when WordPress CPTs, taxonomies, ACF, or query loops are the correct model.

## Responsive Behavior

Bricks controls can handle many responsive values per breakpoint. Core Framework can define breakpoints and responsive utilities. Prefer the project's existing breakpoint model.

Checklist:

- Grids collapse intentionally.
- Card padding remains comfortable.
- Headings do not overflow.
- Buttons remain touch-friendly.
- Images have stable aspect ratios.
- Section padding uses tokens and does not crush content on mobile.
- CSS written with variables still works if theme mode changes.

## Debugging

When Bricks preview differs from front end:

1. Confirm Core Framework stylesheet loads in both contexts.
2. Confirm Bricks integration addon sync is current.
3. Check Bricks CSS regeneration.
4. Check Core Framework save/generate output.
5. Clear cache/performance plugins.
6. Compare rendered classes in the front end.
7. Check specificity: Bricks element ID styles can override class CSS.
8. Check whether the token resolves to a different value in dark/light mode.

When a variable does not work in Bricks:

- Confirm the variable exists in generated CSS.
- Confirm it is available in the scope where the element renders.
- Confirm the Bricks control accepts `var(--token)` for that field.
- If Bricks strips the value, use the correct control format or move the declaration into CSS.

# Automatic.css Working Rules

Use this reference for styling Bricks content on sites with Automatic.css.

Before applying version-sensitive guidance, read `acss-versioning.md`. Bricks projects may run ACSS 3.x or 4.x. Do not use ACSS 4.x assumptions on a 3.x project.

## Token First

Prefer ACSS variables in custom CSS:

- Spacing: `var(--space-xs)` through `var(--space-xxl)`
- Section spacing: `var(--section-space-xs)` through `var(--section-space-xxl)`
- Gutter: `var(--gutter)`
- Context gaps: `var(--content-gap)`, `var(--container-gap)`, `var(--grid-gap)`
- Typography: `var(--h1)` through `var(--h6)`, `var(--text-xl)` through `var(--text-xs)`
- Colors: `var(--primary)`, `var(--secondary)`, `var(--base)`, `var(--white)`, plus available light/dark variants
- Radius: `var(--radius-s)`, `var(--radius-m)`, `var(--radius-l)`

Use raw hex, arbitrary pixels, and one-off `clamp()` only when the design cannot be represented by the site's token set.

## Structure

Prefer this responsibility split:

- Section element: section rhythm, background, broad layout context.
- Inner container: content width and centering.
- Component/card classes: local layout, gap, padding, border, and visual treatment.

Example:

```css
.services {
  background: var(--base-ultra-light);
}

.services__inner {
  max-inline-size: var(--content-width);
  margin-inline: auto;
  display: grid;
  gap: var(--container-gap);
}

.services__grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: var(--grid-gap);
}
```

## Utilities

ACSS is not purely utility-first. Utilities are useful, but custom BEM classes with variables are often clearer for builder-authored sections.

Before relying on a utility class, verify the generated CSS includes it. If it is absent, implement the behavior in custom CSS using variables.

On ACSS 3.x, existing Bricks pages may legitimately rely on many utilities. Preserve them unless the user asks for cleanup or migration.

On ACSS 4.x, expect fewer utility modules and prefer BEM/custom classes plus variables and recipes.

## Responsive

Use explicit responsive behavior:

```css
@media (max-width: 767px) {
  .services__grid {
    grid-template-columns: 1fr;
  }
}
```

Prefer container queries for reusable components when the parent context controls the layout, and media queries for page-level rhythm or broad viewport changes.

## Buttons

Use ACSS button classes when they match the project:

- `.btn--primary`
- `.btn--secondary`
- `.btn--outline`
- size and style classes that exist in the generated CSS

For custom button components, keep local classes small and token-driven.

## Avoid

- Random spacing values when an ACSS token exists.
- Styling every section with custom padding by default.
- Utility classes that are not generated on the site.
- Local CSS that fights global ACSS button, typography, or spacing systems.

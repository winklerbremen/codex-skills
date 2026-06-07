# Automatic.css Deep Reference For Bricks

Use this reference for detailed Automatic.css decisions in Bricks projects. Always pair it with `acss-versioning.md` when the active ACSS major version matters.

## Philosophy

ACSS is a token and workflow framework, not just a bag of utility classes.

Default priority:

1. Use ACSS dashboard/token decisions.
2. Use CSS variables in BEM/custom classes.
3. Use utilities only when generated, stable, and clearer.
4. Use recipes/mixins where appropriate for the active version and CSS context.
5. Use raw CSS values only for genuine design exceptions.

## Variables

ACSS variables are CSS custom properties defined globally and mapped to dashboard values. They are the safest interface for custom Bricks classes.

Use variables for:

- Color
- Spacing
- Section spacing
- Gaps
- Font sizes
- Radii
- Widths
- Header offsets where available
- Button tokens where available

Example:

```css
.profile-card {
  background: var(--base-ultra-light);
  color: var(--base);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  gap: var(--content-gap);
}
```

Local variable overrides are useful for variants:

```css
.profile-card[data-variant="accent"] {
  --card-bg: var(--primary);
  --card-text: var(--primary-ultra-light);
}

.profile-card {
  background: var(--card-bg, var(--base-ultra-light));
  color: var(--card-text, var(--base));
}
```

## Section Spacing

ACSS controls section spacing by default. Section spacing includes:

- Block padding: top/bottom.
- Gutter: inline padding left/right.
- Section spacing variables.
- Section classes.

ACSS section spacing is based on regular spacing values multiplied for section-scale rhythm. It is fluid between mobile and desktop.

Rules:

- Do not add custom padding to every section.
- Override section padding only when the design needs a different section rhythm.
- Let top-level `section` elements carry broad rhythm.
- Let inner containers handle width and alignment.

## Contextual Spacing

ACSS defines contextual gaps:

- `--container-gap`: gap between containers/groups in sections.
- `--content-gap`: gap between content elements.
- `--grid-gap`: gap between grid items/columns.

Use these over arbitrary spacing variables when the context matches.

Examples:

```css
.hero__inner {
  display: grid;
  gap: var(--container-gap);
}

.hero__copy {
  display: grid;
  gap: var(--content-gap);
}

.hero__cards {
  display: grid;
  gap: var(--grid-gap);
}
```

Adjust context without abandoning it:

```css
.feature-grid {
  gap: calc(var(--grid-gap) * 1.5);
}
```

## Smart Spacing

Smart Spacing removes default margins from headings, paragraphs, lists, and list items so layout can use `gap` cleanly. It then reapplies intelligent spacing in rich text, post content, targeted containers, and relevant content contexts.

Use:

- Flex/grid gap for designed layouts.
- `.smart-spacing` when rich/content children need adjacent-sibling spacing and automatic targeting is not catching the parent.
- `.smart-spacing--off` when Smart Spacing causes unwanted margins and the container is explicitly gap-controlled.

Smart Spacing targets direct children. Extra wrappers can prevent it from applying.

## Typography

Prefer ACSS typography variables:

- Headings: `var(--h1)` through `var(--h6)`.
- Text: `var(--text-xs)` through `var(--text-xl)` and related sizes available in the active version.

Use text utility classes only if generated and appropriate.

For Bricks:

- Global typography belongs in ACSS dashboard or Bricks Theme Styles.
- Section/component-specific adjustments belong in BEM classes.
- Avoid many element-ID typography settings unless truly one-off.

## Colors

Use semantic color families:

- `primary`
- `secondary`
- `tertiary`
- `accent`
- `base`
- `neutral`
- `white`
- `black`

Use available light/dark/ultra variants in the active version. Verify generated variables/classes.

ACSS 3.x commonly supports HSL partials and transparency token workflows.

ACSS 4.x uses OKLCH and removes transparency token libraries. Use `color-mix()` or relative color syntax for transparent variants.

## Buttons

ACSS button class convention uses `.btn--`.

Typical 3.x-compatible classes:

- `.btn--primary`
- `.btn--secondary`
- `.btn--outline`
- `.btn--s`, `.btn--m`, `.btn--l`, `.btn--xl`, `.btn--xxl` where generated.
- Light/dark variants such as `.btn--primary-light` if enabled.

Rules:

- Use button classes when they match the site's system.
- Do not combine main style and light/dark variants unless ACSS docs for that version say to.
- Prefer a Bricks Button element with valid link shape and ACSS classes.
- For custom button components, keep button CSS token-driven.

ACSS 4.x may prefer recipes or variables for custom button classes depending on context.

## Grids And Flex

Use native CSS grid/flex with ACSS variables in custom classes.

Example:

```css
.team-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: var(--grid-gap);
}

@media (max-width: 767px) {
  .team-grid {
    grid-template-columns: 1fr;
  }
}
```

For ACSS 3.x, existing utilities such as grid or flex utility classes may be in use. Preserve them unless migrating.

For ACSS 4.x, expect fewer utilities and prefer custom classes plus recipes.

## Recipes

Recipes expand reusable CSS snippets.

Version-sensitive syntax:

- ACSS 3.x recipes use `@` in documented older workflows, e.g. `@columns`.
- ACSS 4.x recipe syntax changed to `?`, e.g. `?btn`, `?flex-grid`.

Recipes can be used in builder CSS inputs or ACSS Custom SCSS when supported. Some recipes expose values for input fields; others expand full CSS blocks.

Do not paste recipe syntax into contexts that do not expand recipes.

## Mixins And Functions

ACSS mixins and functions are SCSS features for the ACSS dashboard Custom SCSS area. They do not work in normal Bricks builder CSS fields.

Use mixins only when editing ACSS Custom SCSS and the active ACSS version supports the mixin.

Example concept:

```scss
.my-custom-button {
  @include btn(secondary);
}
```

Do not use SCSS mixins in Bricks page custom CSS.

## Utilities

Utilities are generated classes. Their presence depends on ACSS version and enabled modules.

Before using a utility:

1. Detect ACSS major version.
2. Check generated CSS or cheat sheet/output if possible.
3. Confirm the utility is enabled.
4. Prefer a custom class if the utility chain becomes long or unclear.

ACSS 3.x can be utility-heavy. ACSS 4.x intentionally removed many utility modules.

## Bricks Integration

Bricks + ACSS best practice:

- Use ACSS variables in Bricks global classes and BEM CSS.
- Use Bricks global classes for shared reusable styling.
- Use Bricks components for repeated element trees.
- Avoid overusing element ID styles.
- Keep ACSS token ownership clear; avoid duplicating many Bricks variables for the same concepts.

## Source Snapshot

Official Automatic.css documentation reviewed on 2026-04-30:

- ACSS 4.x What's New
- ACSS 3.x and 4.x Bricks setup docs
- Variables
- Section Spacing
- Contextual Spacing
- Smart Spacing
- Button Classes
- Text Classes
- Recipes
- Mixins

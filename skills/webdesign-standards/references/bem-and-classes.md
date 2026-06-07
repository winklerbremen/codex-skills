# BEM And Class Strategy

## BEM

Use BEM for meaningful components and sections:

```text
block
block__element
block--modifier
```

Examples:

- `hero`
- `hero__inner`
- `hero__heading`
- `service-card`
- `service-card__title`
- `service-card--featured`

Name by meaning, not appearance. Use `alert-card`, not `red-box`.

## Local vs Global

Local page/section classes:

- Good for one page.
- Good for prototypes.
- Good when reuse is unknown.

Global classes:

- Good for repeated patterns.
- Good when the user approves shared styling.
- Risky when edited without checking all uses.

Framework classes:

- Use only when generated/available.
- Treat as owned by the framework project.
- Do not redefine casually.

## Utilities

Utilities are useful for atomic, predictable declarations. They are not a substitute for naming a component.

Use utilities when:

- The framework generates them.
- They are easy to scan.
- They do not create a long unreadable class list.

Use BEM when:

- Several declarations work together.
- There are child elements.
- The pattern is reused.
- A modifier/state is needed.

## Modifiers And State

Use modifiers for variants:

```html
<article class="price-card price-card--featured">
```

Use attributes for prop-driven variants:

```html
<article class="price-card" data-tone="brand">
```

Use state classes only when state is truly dynamic:

- `.is-open`
- `.is-active`
- `.is-disabled`

Prefer native attributes where possible:

- `disabled`
- `aria-expanded`
- `aria-current`
- `hidden`

## Builder Projects

In Bricks:

- Use element settings for local one-off visual values.
- Use page CSS + BEM for page-local sections.
- Use Bricks global classes for reusable builder-controlled styles.
- Use Bricks components for reusable structures.
- Use framework classes/variables when the design system owns the styling.

Avoid styling by generated Bricks IDs unless the style is genuinely element-specific.

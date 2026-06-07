# CSS Architecture

## Cascade

CSS resolves conflicts by origin, importance, cascade layer, specificity, scoping proximity, and source order. Specificity only matters inside the same origin/layer/importance context.

Use this order of preference:

1. Correct ownership: put the rule in the right system.
2. Low-specificity class selectors.
3. Source order within a known scope.
4. Cascade layers only when the project already uses them or third-party CSS needs formal ordering.
5. `!important` only as a documented last resort.

## Specificity

Prefer:

```css
.card {}
.card__title {}
.card[data-variant="featured"] {}
:where(.content-flow) > * + * {}
```

Avoid:

```css
#brxe-abc123 .card .wrapper .title {}
main section div div h2 {}
.card.card.card {}
```

Builder element IDs are acceptable for one-off element settings inside the builder, but they should not become the backbone of reusable CSS.

## Native CSS Nesting

Native CSS nesting is browser-parsed CSS, not Sass. Use it to keep component-related selectors together:

```css
.nav {
  display: flex;

  & a {
    color: inherit;
  }

  & a:focus-visible {
    outline: 2px solid currentColor;
  }
}
```

Keep nesting shallow. A good limit is one to two levels. Deep nesting hides specificity and makes overrides harder.

Remember:

- `&` itself does not add specificity, but nested selectors still contribute.
- Nesting behaves similarly to `:is()` for specificity in selector lists.
- Sass-style concatenation is not native CSS.

## Custom Properties

Use custom properties for design tokens and local component parameters:

```css
.banner {
  --banner-padding: var(--space-l);
  padding: var(--banner-padding);
  background: var(--bg-body);
}
```

Global framework tokens should come from the framework. Local component variables can adapt a component without creating new global tokens.

## Cascade Layers

Use layers intentionally:

```css
@layer reset, base, components, utilities;

@layer components {
  .card { ... }
}
```

Do not introduce layers into a WordPress/builder project casually. Existing builder/framework CSS may be unlayered, and unlayered author styles have strong precedence over layered author styles.

Layers are useful when:

- A project has a clear CSS build pipeline.
- Third-party CSS needs to be contained.
- A framework expects layered architecture.

## Layout Standards

Use Grid for two-dimensional layout and Flexbox for one-dimensional distribution.

Use container queries when a component should respond to its available space. Use media queries for viewport-level layout decisions.

Prefer logical properties:

- `padding-block`
- `padding-inline`
- `margin-inline`
- `max-inline-size`
- `inset-block-start`

Use stable dimensions for fixed-format UI: aspect ratios, min/max, grid tracks, and object-fit.

## Anti-Patterns

- Recreating framework token systems.
- Global CSS for a one-off local issue.
- Page CSS for a repeated global design decision.
- Styling by DOM depth instead of classes.
- Hiding accessibility focus styles.
- Negative margins as layout architecture.
- Utility-only markup for complex components when BEM would be clearer.

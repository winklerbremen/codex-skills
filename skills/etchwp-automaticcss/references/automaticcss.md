# Automatic.css Reference

Official docs reviewed on 2026-04-27: `https://docs.automaticcss.com/`

## Mental Model

Automatic.css (ACSS) is a WordPress/page-builder CSS framework. Use it as a global design system: dashboard settings generate variables, utility classes, buttons, spacing, colors, and layout helpers.

ACSS is not utility-first. The maintainable pattern is:

- Use custom BEM/component classes for reusable components and unique patterns.
- Power custom classes with ACSS variables, not hardcoded values.
- Use utility classes sparingly for simple, stable, system-level decisions only when those utilities are generated in the current site's `automatic.css`.

Primary rule: variables first, utilities second.

## Variables First

Prefer variables in CSS:

```css
.testimonial-card {
  padding: var(--space-m);
  gap: var(--content-gap);
  background: var(--base-ultra-light);
  color: var(--base);
}
```

Common variable families:

- Spacing: `var(--space-xs)` through `var(--space-xxl)`.
- Spacing bridge variables where available: `var(--space-xl-to-m)`, `var(--space-l-to-s)`, etc.
- Section spacing: `var(--section-space-xs)` through `var(--section-space-xxl)`.
- Section bridge variables where available: `var(--section-space-xl-to-m)`, `var(--section-space-l-to-s)`, etc.
- Gutter: `var(--gutter)`.
- Contextual gaps: `var(--container-gap)`, `var(--content-gap)`, `var(--grid-gap)`.
- Typography: `var(--h1)` through `var(--h6)`, `var(--text-xl)` through `var(--text-xs)`.
- Typography properties where declared: `var(--heading-font-family)`, `var(--heading-line-height)`, `var(--text-line-height)`, level-specific heading variables, and text-size-specific variables.
- Colors: `var(--primary)`, `var(--secondary)`, `var(--accent)`, `var(--base)`, `var(--neutral)`, plus shades where enabled.
- Radius/shadow/global tokens where configured by the project.

Avoid inventing raw values like `37px`, one-off hex colors, or unscaled margins unless the design explicitly needs them.
Avoid inventing custom fluid `clamp()` scales for font sizes or spacing when ACSS typography and spacing variables already provide responsive values.

Bridge variables such as `--space-xl-to-m` and `--section-space-xl-to-s` are useful when a design needs a larger desktop value that fluidly resolves to a smaller mobile value. Prefer bridge variables over custom `clamp()` when the ACSS token exists.

## Spacing

ACSS standard spacing is fluid, responsive, and scale-based. The dashboard controls base spacing and scale ratios, and changes can be made globally after a site is built.

Use spacing variables anywhere a length value is needed:

```css
.card {
  padding: var(--space-m);
  margin-block-start: var(--space-l);
}
```

Use contextual spacing for repeated layout contexts:

- `container-gap` for multiple containers within a section.
- `content-gap` for headings, text, buttons, and content stacks.
- `grid-gap` for grid and column layouts.

Prefer variables in custom classes:

```html
<div class="feature-list">...</div>
```

```css
.feature-list {
  gap: var(--grid-gap);
}
```

Use `.content-gap`, `.grid-gap`, or `.container-gap` only after confirming they exist in the generated ACSS file. Some project settings may omit utilities while keeping variables available.

Smart/contextual spacing should be treated as a system-level rhythm helper. Use the variables freely in component CSS; only use helper classes such as `.content-gap` or `.grid-gap` when verified in the active generated stylesheet.

If a contextual value needs adjustment, derive from it:

```css
.wide-grid {
  gap: calc(var(--grid-gap) * 1.5);
}
```

## Sections

Recommended section structure:

```html
<section class="hero section--xl">
  <div class="hero__inner">
    ...
  </div>
</section>
```

Principles:

- The outer `section` owns vertical section padding and inline gutter.
- The inner container owns max width and centering.
- Avoid using `min-height` to fake whitespace except for special hero/viewport cases.
- Top-level section elements receive medium section spacing by default in ACSS.
- Adjust section rhythm with `.section--{size}` classes where available.

Section classes use `.section--{size}` and typically support t-shirt sizes from `xs` to `xxl`.

Use variables for custom section classes:

```css
.hero {
  padding-block: var(--section-space-xl);
  padding-inline: var(--gutter);
}
```

## Utilities

Use utilities only when they remain readable, are actually generated, and do not create maintenance debt:

- Grid utilities for common grid counts and breakpoints, such as `.grid--3` and responsive variants like `.grid--s-1` when enabled.
- Gap utilities such as `.gap--m`, `.row-gap--m`, `.col-gap--m`, plus breakpoint variants when enabled.
- Background/text/link utility classes when they reflect system colors.
- Display/flex/visibility utilities for simple state or layout adjustments.

Prefer custom CSS when:

- An element needs many utilities.
- A pattern is reused.
- Styling carries semantic meaning.
- The same group of utilities would be duplicated.
- You can express the rule cleanly with BEM plus ACSS variables.

Before relying on utilities, inspect the active generated ACSS file or front-end source. If a utility class is absent, do not leave it in markup as decorative noise; create or update the BEM class with ACSS variables.

## Colors

Use ACSS palette variables and classes. Common color families include primary, secondary, tertiary, accent, base, neutral, and semantic/status colors if enabled.

For custom CSS:

```css
.notice {
  background: var(--warning-light);
  color: var(--warning-dark);
}
```

For transparencies in ACSS 4.x, do not rely on old transparency tokens. Use modern CSS:

```css
.overlay {
  background: color-mix(in srgb, var(--base) 70%, transparent);
}
```

## Buttons and Links

Use ACSS button classes for standard buttons:

```html
<a class="btn--primary" href="/kontakt">Kontakt</a>
<a class="btn--primary btn--outline" href="/leistungen">Mehr erfahren</a>
```

ACSS dashboard controls global button padding, min width, typography, borders, radius, transition, and per-color overrides.

Override locally by redefining button tokens instead of writing unrelated button CSS:

```css
.hero .btn--primary {
  --btn-background: var(--primary-dark);
  --btn-background-hover: var(--primary-ultra-dark);
}
```

Do not add radius utilities to buttons by default; keep radius governed by the ACSS button dashboard unless a real exception exists.

## Responsive Practice

- Let ACSS fluid variables handle most spacing and typography responsiveness.
- Explicitly define grid collapse behavior for small breakpoints.
- Use responsive utility variants if the project enables them.
- Check mobile overflow when using min button widths, large gaps, fixed widths, or long German words.

## Practical Patterns

Card:

```css
.resource-card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border: 1px solid var(--base-light);
  border-radius: var(--radius-m);
  background: var(--white);
}
```

Grid section:

```html
<section class="resources section--l">
  <div class="resources__inner">
    <div class="resources__grid grid--3 grid--s-1 grid-gap">
      ...
    </div>
  </div>
</section>
```

Custom grid without utility overload:

```css
.resources__grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: var(--grid-gap);
}

@media (max-width: 767px) {
  .resources__grid {
    grid-template-columns: 1fr;
  }
}
```

## Quality Checklist

- Uses ACSS variables for spacing, colors, and system values.
- Uses contextual spacing for content/grid/container gaps.
- Uses section classes or section variables for vertical rhythm.
- Uses typography variables for local font-size decisions.
- Avoids raw hex values and arbitrary pixels.
- Avoids custom `clamp()` sizing when an ACSS token fits.
- Uses utility classes selectively and only when present in the active ACSS output.
- Keeps reusable UI in custom classes.
- Provides explicit mobile layout behavior.




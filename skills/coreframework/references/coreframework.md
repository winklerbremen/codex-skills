# Core Framework Reference

## Table of Contents

- Role in the stack
- Setup and storage mindset
- Variables and tokens
- Colors and theme modes
- Typography
- Spacing and layout
- Breakpoints and responsive behavior
- Components
- Utilities
- Imports, exports, and `.core` files
- Debugging checklist

## Role in the Stack

Core Framework is a visual CSS toolkit for managing a site's reusable design system. In WordPress it exists as a plugin and can be used alone or with builder integration addons. It manages variables, classes, colors, typography, spacing, breakpoints, components, utilities, generated CSS, and project imports/exports.

Treat Core Framework as a design-system source of truth rather than a single page styling helper. A change to a token, generated utility, or component can affect the entire site.

Core Framework is not Automatic.css. Do not transfer ACSS class names, recipes, spacing behavior, transparency tokens, or variable assumptions unless the project explicitly contains both systems and the target has been checked.

## Setup and Storage Mindset

When entering an existing WordPress project:

1. Confirm the Core Framework plugin is active and note the version.
2. Confirm whether a builder integration addon is active, especially Bricks, Oxygen, or Gutenberg.
3. Find how CSS is emitted: generated stylesheet, inline output, cached file, builder integration, or snippets.
4. Inspect active project variables/classes from the generated CSS or plugin data before choosing names.
5. Identify whether the project was imported from a `.core` file.
6. Verify caches after updates: Core Framework save/generate action, WordPress object cache, performance plugin cache, builder CSS cache, and browser cache.

Use plugin/project settings for global design-system edits. Use local CSS only for target-specific behavior, experimental fixes, or features not modeled by Core Framework.

## Variables and Tokens

Use CSS variables as the default interface:

```css
.surface-card {
  padding: var(--space-m);
  border-radius: var(--radius-m);
  color: var(--text-body);
  background: var(--bg-surface);
}
```

Common Core Framework defaults seen in official docs include:

- `--primary`
- `--bg-body`
- `--text-body`
- `--text-title`
- `--text-m`
- `--space-m`
- `--space-2xl`
- `--max-screen-width`

Never assume a token exists. Search generated CSS for `--token-name:` or inspect Core Framework project data first.

Good token use:

- `background: var(--bg-body)` for page background.
- `color: var(--text-body)` for body copy.
- `color: var(--text-title)` for headings.
- `font-size: var(--text-m)` for body text.
- `padding-block: var(--space-2xl)` and `padding-inline: var(--space-m)` for section rhythm.
- `max-inline-size: var(--max-screen-width)` for primary containers.

Avoid:

- New local `clamp()` values when a typography/spacing token exists.
- Raw hex colors for brand/system colors.
- Hardcoded widths that compete with `--max-screen-width`.
- Utility classes copied from other frameworks.

## Colors and Theme Modes

Core Framework has a smart color system and can generate shades/variants. Preserve semantic intent:

- Brand: primary, secondary, accent.
- Surfaces: body background, cards, panels, elevated regions.
- Text: body text, title text, muted text.
- State: success, warning, danger, info.

If the site supports dark/light mode, use theme-aware variables. Do not force `#fff`, `#000`, or fixed grayscale values into reusable components unless the design intentionally ignores theme mode.

Before changing colors:

1. Identify whether the token is global, semantic, or component-specific.
2. Check where the token is used on the front end.
3. Prefer adding a new semantic token over repurposing a token with existing meaning.
4. For large palette changes, propose the change and expected affected areas before editing.

## Typography

Use Core Framework typography variables for fluid typography. Official Bricks setup guidance uses `var(--text-m)` for body text and `var(--text-title)` for headings.

Good defaults:

```css
body {
  color: var(--text-body);
  font-size: var(--text-m);
}

h1,
h2,
h3,
h4,
h5,
h6 {
  color: var(--text-title);
}
```

When building components:

- Use semantic heading levels from page structure, then style through classes/tokens.
- Do not choose heading tags for visual size alone.
- Avoid negative letter spacing unless the existing design system already uses it.
- Keep compact UI text smaller and tighter than hero text.

## Spacing and Layout

Core Framework can generate automatic and responsive sizing/spacing. Prefer spacing tokens:

```css
.section {
  padding-block: var(--space-2xl);
  padding-inline: var(--space-m);
}

.section__inner {
  max-inline-size: var(--max-screen-width);
  margin-inline: auto;
}

.cluster {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-s);
}
```

Keep responsibilities clear:

- Outer section owns background and vertical rhythm.
- Inner container owns width and centering.
- Component/card owns internal padding and local gap.
- Grid/list owns column behavior and item gap.

Do not add nested containers only for styling. Avoid card-inside-card composition unless each card is a real repeated item or modal/tool frame.

## Breakpoints and Responsive Behavior

Use the project's existing Core Framework breakpoints and generated responsive utilities where available. If writing CSS manually:

- Prefer responsive layout rules close to the component class.
- Use container queries for reusable components when the parent size is more important than viewport width.
- Use media queries for page-level navigation, global rhythm, and full-viewport layout changes.
- Verify text fit and touch targets on mobile.

Do not invent breakpoint values before checking the active project configuration.

## Components

Core Framework includes a component editor and official components/patterns. Official Blocks documentation mentions Core Framework-native components such as button, badge, card, icon, link, avatar, divider, and input.

Use Core Framework components when:

- A primitive is shared across the site.
- The user wants future edits to be made in the Core Framework dashboard.
- The project already uses those components consistently.

Use BEM component classes when:

- The pattern is page/section-specific.
- The component has custom markup.
- The builder needs editable structure and the Core Framework component would be too generic.

Use custom CSS with Core variables:

```css
.pricing-card {
  display: grid;
  gap: var(--space-m);
  padding: var(--space-l);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-l);
  background: var(--bg-card);
}
```

Verify token names before saving. If `--border-subtle`, `--radius-l`, or `--bg-card` do not exist, use existing project equivalents or add tokens deliberately.

## Utilities

Core Framework can generate utility classes and variables. Utilities are useful for small reusable declarations, but they should not erase semantic structure.

Good utility usage:

- A one-off alignment utility where the project uses such utilities.
- A known generated color/spacing utility on simple builder elements.
- Rapid layout scaffolding that remains understandable in the builder.

Prefer BEM classes when:

- Multiple styles define a component.
- The same pattern appears in several places.
- The HTML needs readable ownership.
- The style depends on a local modifier or state.

Before relying on a utility, verify it exists in generated CSS or in the Core Framework dashboard.

## Imports, Exports, and `.core` Files

`.core` files can represent Core Framework project data. Imports can overwrite variables, classes, components, and project settings.

Before importing:

1. Confirm whether the user wants merge-like behavior or full overwrite.
2. Export/back up the current Core Framework project when possible.
3. Note active builder integration dependencies.
4. Verify after import that generated CSS contains expected variables/classes.
5. Rebuild caches and builder CSS if applicable.

Official template guidance may recommend "Overwrite All" for a template setup. Do that only when the user is intentionally installing that template/project, not when making a narrow site edit.

## Debugging Checklist

When a Core Framework style does not appear:

1. Confirm the token/class is actually generated.
2. Confirm the stylesheet is enqueued on the front end.
3. Confirm the selector/class is present in rendered HTML.
4. Check whether builder preview and front end load different CSS/caches.
5. Check specificity against builder ID styles, inline styles, global classes, or theme styles.
6. Check dark/light mode values.
7. Check whether the integration addon sync is stale.
8. Save/regenerate Core Framework output and clear caches.
9. Verify in browser/front-end HTML after the fix.

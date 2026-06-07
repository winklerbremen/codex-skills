# Bricks Responsive Reference

## Bricks Defaults

Bricks provides these default breakpoints:

- Desktop: base breakpoint.
- Tablet portrait: `< 992px`.
- Mobile landscape: `< 768px`.
- Mobile portrait: `< 478px`.

Styles set on the base breakpoint inherit to the other breakpoints unless overridden.

## Base Breakpoint

The base breakpoint has no media query. It is the source of inherited styles.

By default, Bricks uses a desktop-first workflow. To work mobile-first, the smallest breakpoint can be set as the base breakpoint. This reverses the practical inheritance direction and should be decided at the beginning of a project.

Ask before changing:

- Base breakpoint.
- Breakpoint widths.
- Custom breakpoint setup.

## Custom Breakpoints

Custom breakpoints are enabled in WordPress under:

```text
Bricks > Settings > General > Custom breakpoints
```

After changing breakpoint widths, Bricks may need CSS regeneration. If front-end breakpoint behavior does not match the builder, regenerate Bricks CSS files under the custom breakpoints settings.

## Responsive Controls

Use Bricks responsive controls for:

- Display.
- Grid/flex direction.
- Gaps.
- Padding/margin.
- Width/max width.
- Typography values.
- Alignment.
- Visibility when truly necessary.

Avoid hiding important content on mobile unless there is an accessible equivalent or the content is genuinely decorative/nonessential.

## Existing Sites

For existing desktop-first Bricks sites:

1. Preserve the current base breakpoint unless the user approves conversion.
2. Fix mobile breakpoints locally.
3. Avoid rewriting all base styles.
4. Document if a mobile-first rebuild would be cleaner.

For new sections in a desktop-first site:

- Build the Bricks data in the existing inheritance direction.
- Still think mobile-first conceptually: check the smallest layout early and avoid desktop-only assumptions.

## Framework Variables

If Core Framework or ACSS is active, use framework tokens for responsive values:

- Typography.
- Spacing.
- Section rhythm.
- Container width.
- Grid gaps.

Do not create independent fluid typography when the framework already provides it.

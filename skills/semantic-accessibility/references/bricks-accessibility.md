# Bricks Accessibility Reference

## Bricks Strengths

Bricks provides semantic output tools:

- Header, Main, and Footer landmarks in normal site structure.
- Element tag controls on containers.
- Semantic elements for forms and nav menus.
- Custom attributes for roles/ARIA/labels when needed.
- Image alt support from WordPress media or custom alt.
- Default skip links to content/footer.
- Focus outline settings in Theme Styles.
- Keyboard-accessible menu behavior.

## Bricks Implementation Rules

Use the correct Bricks element and tag:

- Nav Menu for navigation.
- Form element for forms.
- Button element for CTAs/actions when appropriate.
- Heading element with correct tag.
- Container/block tag set to `section`, `article`, `nav`, `aside`, etc. when meaningful.

## Attributes

Use custom attributes carefully:

- `aria-expanded` on custom toggles.
- `aria-controls` when controlling a region.
- `aria-current="page"` for current nav link where not handled.
- `aria-label` only when visible text is absent or insufficient.
- `role` only when native element choice cannot express the role.

## Images

For Bricks Image elements:

- Prefer WordPress media alt text.
- Override only when the context changes the meaning.
- Use empty/decorative handling for decorative images where possible.

For background images:

- If decorative, avoid announcing them.
- If meaningful, provide an accessible label or use a real image element instead.

## Focus

Do not remove Bricks focus outline unless replacing it with a visible, high-contrast focus style.

Watch for sticky headers and off-canvas menus obscuring focus. Use scroll margins or layout adjustments where needed.

## Menus

Check:

- Tab and Shift+Tab order.
- Enter activates links.
- Space toggles submenu where relevant.
- Escape/close behavior for overlays if custom.
- Focus returns predictably after closing.

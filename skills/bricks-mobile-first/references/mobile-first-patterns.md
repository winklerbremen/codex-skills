# Mobile-First Bricks Patterns

## Section Pattern

Mobile base:

- Single column.
- Vertical flow.
- Compact but readable spacing.
- Full-width or near full-width inner container.
- CTA below copy.

Desktop enhancement:

- Two-column or grid layout.
- Larger gaps.
- Larger section padding.
- Optional media/copy side-by-side.

## Cards And Grids

Mobile:

- One column.
- Cards with stable padding and no nested card clutter.
- Images above text unless content strongly suggests otherwise.

Tablet:

- Two columns if content width allows.

Desktop:

- Three or four columns only if cards remain readable.

Use `minmax(0, 1fr)` style thinking for grids to avoid overflow.

## Typography

Use the active framework's text/heading variables first. Do not create local `clamp()` values when the design system already defines fluid type.

Check:

- Long German compound words.
- Button labels.
- H1/H2 line breaks.
- Card title wrapping.

## Touch Targets

Buttons, links, nav toggles, filters, tabs, accordions, and form controls should remain easy to tap. WCAG 2.2 target-size minimum is a useful guardrail: aim for at least 24 by 24 CSS px or sufficient spacing, and often larger for primary controls.

## Navigation

For mobile nav:

- Preserve keyboard access.
- Use real buttons for toggles.
- Keep focus visible.
- Ensure menu state is communicated with `aria-expanded` when custom controls are used.
- Avoid relying on hover-only behavior.

## Images

Set stable aspect ratios or dimensions where possible. Use meaningful cropping and avoid letting faces/products/key content crop out on mobile.

## Common Fixes

- Horizontal overflow: check wide children, fixed widths, grids, negative margins, long text, and absolute elements.
- Cramped cards: reduce columns, not only font size.
- CTA wrapping badly: allow multi-line labels or adjust button layout.
- Media/text mismatch: stack earlier.
- Sticky header obscures anchors/focus: adjust scroll margins and focus behavior.

## QA Checklist

- 320px narrow mobile.
- 375px/390px common mobile.
- 478px Bricks mobile portrait boundary.
- 768px mobile landscape boundary.
- 992px tablet boundary.
- Desktop/wide desktop.

Check actual content, not only placeholder text.

---
name: bricks-mobile-first
description: Mobile-first and responsive workflow for Bricks Builder WordPress sites. Use when Codex is asked to create, convert, inspect, or fix Bricks layouts for mobile-first design, Bricks breakpoints, custom breakpoints, base breakpoint direction, responsive controls, stacking grids/cards, mobile navigation, touch targets, responsive typography/spacing with framework variables, and desktop/mobile visual QA. This skill is intentionally Bricks-focused and should not apply Etch's container-query-first workflow unless the user explicitly asks to include Etch.
---

# Bricks Mobile First

Use this skill for Bricks Builder responsive work. It focuses on Bricks' breakpoint system and mobile-first implementation.

Do not change a Bricks site's base breakpoint or breakpoint widths without asking. That is a foundational setting that can affect the entire site.

## First Moves

1. Confirm the target is Bricks-authored content.
2. Inspect current breakpoint setup:
   - Default or custom breakpoints.
   - Current base breakpoint.
   - Whether the site is desktop-first or mobile-first.
   - Existing breakpoint-specific element settings.
3. Ask before switching to mobile-first:
   - Changing base breakpoint is global and should happen at the beginning of a project when possible.
4. Detect the active framework:
   - Core Framework, Automatic.css, custom variables, or none.
   - Use framework variables for spacing/typography instead of creating local fluid type/spacing systems.
5. Preserve existing responsive strategy unless the user explicitly asks for conversion.

## Reference Loading

Load only what the task needs:

- `references/bricks-responsive.md` for Bricks breakpoint behavior, base breakpoint rules, custom breakpoints, and CSS regeneration.
- `references/mobile-first-patterns.md` for layout patterns, stacking, grids, navigation, touch targets, typography, and QA.

Also use:

- `webdesign-standards` for CSS architecture.
- `semantic-accessibility` when responsive changes affect headings, nav, forms, focus, or touch targets.
- `bricksbuilder-coreframework` or `bricksbuilder-automaticcss` when a design framework is active.

## Mobile-First Rule

Mobile-first means the smallest practical layout is the base. Larger layouts progressively add complexity.

In Bricks, official responsive editing docs state that the base breakpoint has no media query and its styles are inherited across the breakpoint chain. Bricks can use a mobile-first approach by setting the smallest breakpoint as the base breakpoint. Because this changes inheritance, treat it as a project-level decision.

## Implementation Pattern

For new mobile-first Bricks sections:

1. Build the mobile layout first:
   - One column.
   - Readable text widths.
   - Comfortable touch targets.
   - No horizontal overflow.
   - Images with stable aspect ratio.
   - Correct base radius, overflow, object fit, and min/max sizing.
2. Add tablet/desktop enhancements:
   - Multi-column grid.
   - Larger gaps.
   - More generous section spacing.
   - Side-by-side media/content.
   - Wider containers.
3. Use framework variables:
   - Spacing, type, width, color, radius.
   - Do not invent local clamp/fluid systems when framework tokens exist.
4. Keep controls editable in Bricks:
   - Prefer Bricks responsive controls for normal layout changes.
   - Use CSS for complex selectors, pseudo-elements, or behavior not supported by controls.
   - Verify exact saved control keys from the active project before assuming CSS is required. Bricks stores min-height as `_heightMin`; related controls include `_heightMax`, `_zIndex`, `_position`, `_overflow`, and directional offsets.

In a mobile-first Bricks setup, base/mobile settings must be visually correct on their own. Do not rely on desktop breakpoint values to fix mobile defects such as missing border radius, image overflow, or oversized min-height.

## Bricks Styling Guardrail

For all responsive Bricks work, regardless of whether the project uses Core Framework, Automatic.css 3.x, Automatic.css 4.x, or another design system:

1. Preserve Bricks element defaults and active design-system defaults first.
2. Use existing framework tokens/variables in Bricks responsive controls.
3. Use meaningful classes first for reusable section, component, and pattern styling.
4. Use element ID controls only for genuine one-off exceptions where a class is not sensible.
5. Do not use Page CSS, header style tags, custom scripts, Theme Styles, global class definitions, design-system token changes, or snippets as shortcuts without explicit approval.
6. If a mobile issue appears because styles were saved only at desktop/tablet breakpoints, fix the ownership/breakpoint placement in Bricks controls or classes; do not bypass the architecture with Page CSS.

On Carisurf specifically, mobile-first is a project rule. Start visual implementation and verification at mobile portrait/base, then add larger breakpoint enhancements. Treat desktop-first implementation followed by mobile repair as a workflow error.

## Approval Gate

Ask before:

- Changing base breakpoint.
- Editing custom breakpoint widths.
- Regenerating Bricks CSS as part of a global breakpoint change.
- Changing Bricks Theme Styles.
- Editing global classes/components for responsive behavior.
- Hiding content on mobile instead of redesigning it.

## Quality Bar

Before finishing:

- Desktop, tablet, mobile landscape, and mobile portrait behavior are checked in the rendered front end, preferably with Playwright MCP or the active browser automation plugin.
- No horizontal overflow.
- Text does not collide or overflow.
- Buttons/links remain tappable.
- Cards/grids stack intentionally.
- Images preserve meaningful crop and aspect ratio.
- Radius, clipping, object fit, and min/max sizes are checked at mobile and desktop.
- Framework variables/classes are used where appropriate.
- Any global breakpoint or inheritance change had approval.

## Sources Snapshot

Built from official Bricks docs reviewed on 2026-04-30:

- `https://academy.bricksbuilder.io/article/responsive-editing/`


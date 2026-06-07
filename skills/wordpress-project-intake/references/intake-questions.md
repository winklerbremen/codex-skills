# WordPress Intake Question Bank

Ask only what cannot be inspected and what changes the implementation.

## Stack

- Which page, template, component, or snippet should be changed?
- Is the target authored in Bricks, Etch, Gutenberg, custom theme code, or unknown?
- Which design framework should own the styling: Core Framework, Automatic.css, project CSS, or none?
- Is WPCodeBox 2 used for snippets/code separation on this project?
- Is ACF or another content modeling plugin the source for repeated/dynamic content?

## Scope

- Should the change be local to one page/section, reusable across pages, or global/site-wide?
- May shared Bricks global classes, Theme Styles, components, templates, or framework tokens be edited?
- Is this a quick fix, a durable component, or part of a larger redesign?
- Should existing visual output be preserved exactly, or may the design be improved?

## Responsive

- Should the work be mobile-first?
- If Bricks already has desktop-first breakpoints, should that inheritance direction be preserved?
- Which viewport/content widths must be checked?
- Are there specific mobile pain points: overflow, stacking, tap targets, nav, image crop, forms?

## Accessibility

- Is WCAG 2.2 AA expected, or is this a pragmatic accessibility pass?
- Are there known keyboard, focus, heading, form, or screen-reader issues?
- Should visual design be changed if required for contrast/focus/touch targets?

## Code Placement

- If PHP/JS/CSS is needed, should it live in WPCodeBox 2, the theme/plugin, Bricks page settings, Bricks Theme Styles/global class, or the framework?
- Should the code be exportable as a functionality plugin?
- Does the snippet need conditions, hooks, shortcode behavior, frontend-only loading, admin-only loading, or WooCommerce/ACF-specific scope?

## Approval

Ask before:

- Broad framework token/class changes.
- `.core` imports or overwrite operations.
- ACSS/Core Framework setting changes.
- Bricks Theme Style/global class/component/template edits.
- WPCodeBox snippet creation/editing.
- PHP hooks, redirects, CPT/taxonomy registration, or database/content model changes.
- Any edit likely to affect multiple pages.

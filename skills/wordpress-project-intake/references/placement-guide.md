# Placement Guide

## Principle

Put changes where the responsible system can own them long-term. Avoid solving a framework-level problem with page CSS, and avoid changing global systems for one local section.

## Framework Dashboard / Project

Use Core Framework or Automatic.css project settings when changing:

- Design tokens.
- Color system.
- Typography scale.
- Fluid type.
- Spacing scale.
- Breakpoints.
- Utility generation.
- Button/card/component defaults.
- Theme-aware variables.

Ask first. These changes are usually global.

## Bricks Theme Styles

Use Bricks Theme Styles for builder-wide HTML defaults:

- Body background/text.
- Heading defaults.
- Links.
- Section defaults.
- Container width.
- Form element defaults.
- Focus style when managed globally in Bricks.

Ask before changing existing Theme Styles.

## Bricks Global Classes

Use Bricks global classes for shared Bricks patterns:

- Repeated sections/cards/buttons.
- Builder-editable reusable styles.
- Bricks controls that should remain editable without CSS.

Ask before editing existing global class definitions. Creating a new global class is also broad if it will be reused across templates.

## Bricks Components

Use Bricks components for reusable builder structures with consistent internal markup and instance-level properties.

Ask before editing a component definition, because every instance can change.

## Page Custom CSS / Local BEM

Use page custom CSS and local BEM classes for:

- One page/section only.
- Prototypes.
- Narrow fixes.
- Complex selectors/pseudo-elements not practical in builder controls.

Promote to global/framework only when reuse is proven or requested.

## WPCodeBox 2

Use WPCodeBox 2 for organized snippets when:

- The code is WordPress hook/filter behavior.
- The code should be conditionally loaded.
- The code is reusable across pages or not tied to one builder element.
- The project benefits from snippet folders and code separation.
- PHP/JS/CSS should be managed from WordPress rather than a theme file.

Examples:

- Register an ACF options page.
- Add a shortcode.
- Add a filter/action.
- Enqueue frontend JS/CSS under conditions.
- Add analytics/tag snippets.
- Add small integration glue.

Ask before creating/editing snippets. Treat snippets as live code.

## Theme / Custom Plugin

Use theme or custom plugin files when:

- Code must be versioned/deployed with the project.
- It is core application functionality.
- It registers CPTs/taxonomies/REST endpoints/blocks in a durable way.
- It is too large or structural for snippets.

Prefer a functionality plugin for site functionality that should survive theme changes.

## Content Model

Use WordPress-native content modeling when repeated content or dynamic data is involved:

- CPT for structured entries.
- Taxonomy for categorization.
- ACF field group for fields.
- Options page for global content/settings.
- Query loop/template for display.

Avoid hardcoding repeatable content in builder elements when editors need to maintain it.

# Bricks Architecture Deep Reference

Use this reference when planning or reviewing Bricks structures, global styling, reusable components, templates, and builder-safe edits.

## Layout Elements

Bricks has four core layout elements:

- `section`: root-level page divider. Use one topic/purpose per section.
- `container`: centered content wrapper, default 1100px width in Bricks unless theme styles override it.
- `block`: flex layout element, default 100% width, useful for columns/rows inside a section or container.
- `div`: unstyled generic wrapper. Use when presets from section/container/block are not desired.

Bricks section, container, and block are div-like layout primitives with presets; they use flexbox by default. Div is the plainest wrapper.

Preferred page section shape with ACSS:

```text
section.service-overview
  container.service-overview__inner
    block.service-overview__intro
    block.service-overview__grid
```

Preserve semantic intent:

- Use top-level `section` elements for real page sections.
- Use `container` for content width and centering.
- Use `block` for repeated columns/cards/groups.
- Use `div` only when a neutral wrapper is cleaner.

## Style Hierarchy

Bricks style hierarchy:

1. Element ID settings have highest precedence.
2. Page settings override theme styles.
3. Theme styles provide global/default styling.
4. Global classes are reusable opt-in styling rules.

Implications:

- Prefer global classes or custom CSS for reusable design patterns.
- Avoid sprinkling element-ID styling when the same pattern appears more than once.
- Existing element-ID styling can override classes; inspect before assuming a class is broken.
- When debugging, check element settings, classes, page settings, theme styles, and generated frontend CSS.

## Theme Styles

Theme Styles define global defaults for layout, typography, colors, links, buttons, forms, and Bricks elements.

Use Theme Styles for:

- Global typography and base layout defaults.
- Default button/form/link behavior.
- Default section/container/block/div styling.
- Site-wide or condition-based defaults.

Theme Styles are condition-based. They can target the entire site, front page, specific pages, post types, archives, search results, 404, and terms. Exclude conditions are supported.

Default loading method is "most specific": if a global theme style and a page-specific theme style match, only the most specific loads. "Load all matching theme styles" allows stacking.

Safety:

- Do not change Theme Styles casually; they can affect large parts of the site.
- Before modifying a theme style, identify all matching conditions and whether loading mode stacks styles.
- Prefer page custom CSS or local classes for one-off edits.

## Global Classes

Global CSS classes are central to scalable Bricks work. They can be visually created and managed in the builder.

Important behavior:

- Element ID styles override class styles.
- Global class changes affect every element using that class.
- Bricks has a Global Class Manager for create/edit/delete, ordering, categorization, bulk actions, locking, usage filtering, import, and export.
- Importing classes can create conflicts; newer Bricks versions include an import manager for new/conflicting classes.

Skill policy:

- Treat global classes as shared design-system assets.
- Read usage before editing shared classes.
- Ask before broad global-class edits.
- Prefer adding a local section/component class for scoped changes.

## Variables, Colors, Typography, And Style Manager

Bricks Style Manager centralizes:

- Theme styles
- Classes
- Variables
- Colors
- Typography
- Spacing
- Framework/imported CSS settings

Bricks global variables are CSS custom properties managed in the builder. Automatic.css also provides many CSS variables. In ACSS projects, prefer ACSS variables unless the site has a clear Bricks-native variable convention.

Avoid duplicating token systems. If ACSS owns colors/spacing/type, Bricks variables should complement, not compete.

## Components

Components are reusable Bricks element trees with instance-level properties. Use them for reusable design objects such as buttons, cards, nav items, promos, CTAs, or hero sections.

Component rules:

- A component must have exactly one root element.
- Components cannot be created from Template or Filter elements.
- Changes to the main component apply to all instances.
- Component instances can be customized with properties.
- Unlinking an instance breaks the connection and turns it into normal elements.

Property types include text, rich text, icon, image, image gallery, link, select, toggle, query loop, and global classes.

Use properties for:

- Content overrides: heading, text, image, link.
- Behavior overrides: hide/show, link target, query selection.
- Style variants: global class properties or select-to-class properties.

Safety:

- Do not edit the main component when the user only asked to change one instance.
- Do not unlink components as a convenience.
- Ask before creating a new component or changing a component API.
- Prefer component properties over duplicating near-identical Bricks trees.

## Templates

Bricks templates control headers, footers, singles, archives, search, 404, and other template surfaces.

Template Settings include:

- Header settings for header templates, including position and sticky behavior.
- Conditions deciding where a template is used.
- Exclude conditions.
- Populate Content / preview context.

Template condition examples:

- Entire website
- Front page
- Post type/single
- Archive
- Search results
- 404
- Terms

Safety:

- Do not change template conditions without explicit intent.
- Inspect template type and conditions before editing.
- For archive/search templates, query loop main-query behavior matters.
- For header/footer templates, sticky and positioning settings can affect the whole site.

## Bricks + ACSS Architecture Preference

Use this layering:

1. ACSS controls global tokens and section rhythm.
2. Bricks Theme Styles set builder defaults only where needed.
3. Bricks global classes define shared reusable patterns.
4. BEM classes define scoped section/component behavior.
5. Element ID settings handle true one-off exceptions.

Prefer:

```css
.event-card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--white);
}
```

Avoid:

- ID-styling every card.
- Editing global classes for one local variant.
- Replacing ACSS section rhythm with arbitrary Bricks padding everywhere.
- Mixing many utilities, ID styles, and local CSS without a clear ownership layer.

## Source Snapshot

Official Bricks Academy pages reviewed on 2026-04-30:

- Understanding The Layout
- Global CSS Classes
- Global Class Manager
- Theme Styles
- Style Manager
- Global Variable Manager
- Components
- Template Settings

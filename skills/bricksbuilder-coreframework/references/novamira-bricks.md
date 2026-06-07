# Novamira Bricks Workflow

## Table of Contents

- Ability sequence
- Element tree rules
- Settings and styles
- Templates and conditions
- Components
- Safe edit patterns
- Verification

## Ability Sequence

For most Bricks edits:

1. Discover/confirm available abilities if needed.
2. `novamira/bricks-check-setup` for environment status if builder availability is unclear.
3. `novamira/bricks-get-content` for the target post/template/area.
4. `novamira/bricks-get-settings` for page settings when CSS/settings matter.
5. `novamira/bricks-get-styles` for global classes/theme styles when global styling matters.
6. `novamira/bricks-list-elements` for unfamiliar element schemas.
7. Patch/insert/remove content with targeted abilities.
8. Verify rendered front end.

Prefer targeted operations:

- `bricks-patch-elements`: edit specific existing elements.
- `bricks-insert-content`: add sections/children.
- `bricks-remove-content`: remove specific elements/subtrees.
- `bricks-set-content`: replace the full area only when intentional.

## Element Tree Rules

Each Bricks element should have:

- `id`: short unique id.
- `name`: registered Bricks element name.
- `parent`: parent element id or empty/null for root.
- `settings`: valid settings for that element.

Let Novamira/Bricks derive children from parent references. Preserve existing IDs when patching.

Use semantic element structure:

- `section` for page sections.
- `container` for standard constrained inner wrappers.
- `block` or `div` for local grouping when no container behavior is needed.
- `heading`, `text`, `rich-text`, `button`, `image`, `icon`, `list`, etc. for content.

Avoid:

- Code elements for CSS/JS injection.
- Invalid link shapes.
- Unknown control keys.
- Generic wrappers that recreate built-in Bricks container behavior.
- Full tree replacements for small fixes.
- Editing component definitions without approval.

## Settings and Styles

Use `bricks-set-settings` for page-level settings and CSS. Use `bricks-get-styles`/`bricks-set-styles` for global Bricks styles.

When working with Core Framework:

- Put `var(--token)` values in Bricks controls where supported.
- Use Bricks Theme Styles for global HTML defaults that should consume Core Framework variables.
- Use page custom CSS only for page-local selectors/states Bricks controls cannot express cleanly.
- Use Bricks global classes for shared Bricks patterns.
- Avoid writing competing definitions for Core Framework-generated classes.

Treat Bricks control coverage as broader than the compact ability schema may suggest. Verify exact setting keys from existing saved elements/classes, `bricks-list-elements`, verbose style data, or the Builder UI before deciding a property requires custom CSS. Common saved keys include `_heightMin`, `_heightMax`, `_widthMax`, `_position`, `_top`, `_right`, `_bottom`, `_left`, `_zIndex`, `_overflow`, `_border`, and `_background`.

For Bricks global-class custom CSS, prefer actual class selectors such as `.experience-card::after`. Do not assume `%root%` or another placeholder is replaced unless that exact storage context has been verified.

After Bricks global-class/style storage changes, regenerate or clear Bricks generated CSS when needed, then verify the front end rather than trusting saved JSON alone.

## Templates and Conditions

For template work:

1. List templates.
2. Inspect target template content.
3. Inspect conditions before changing them.
4. Use the template condition schema before setting conditions.
5. Preserve existing conditions unless the user requested condition changes.

An empty condition list means the template will not render. Do not clear conditions accidentally.

## Components

For Bricks components:

1. List components and note instance count.
2. Inspect an existing comparable component or saved pattern before building a new one.
3. Prefer creating a new component for variations unless the user wants all instances changed.
4. Do not edit `elements` of an existing component casually.
5. When applying a component to a page, insert an instance rather than duplicating the component body.
6. Verify at least one page/template where the component renders.

### Property Connections

Property connections link a component property to a specific field on an element inside the component.

**Connection format:**
```json
"connections": {
  "ELEMENT_ID": ["SETTING_KEY"]
}
```

**Common setting keys:**
- `text` — text content of a text/heading element
- `tag` — HTML tag of a heading element (`h2`, `h3`, etc.)
- `image` — image field of an image element
- `icon` — icon field of an icon element
- `_hideElementBuilder` / `_hideElementFrontend` — visibility toggle
- `_cssGlobalClasses` — global class assignment
- `hasLoop` / `query` — loop/query settings on the root element

**Attribute value connections — CRITICAL:**

To connect a property to a custom attribute's value, use:
```
_attributes|ATTRIBUTE_ID|value
```
Where `ATTRIBUTE_ID` is the **`id` field** of the attribute object in the element's `_attributes` array — **not** the array index.

Example: If an element has `_attributes: [{name: "data-gap", id: "data-gap", value: "m"}, ...]`, the correct connection key is `_attributes|data-gap|value`.

**Wrong (causes connections to not appear in Builder UI):**
```json
"connections": {"facilts": ["_attributes|1|value"]}
```

**Correct:**
```json
"connections": {"facilts": ["_attributes|data-gap|value"]}
```

### Slot Mechanism (Bricks 2.0+)

Component instances containing a slot element require a `slotChildren` field on the page element:

```json
{
  "id": "instance_id",
  "name": "block",
  "cid": "component_id",
  "slotChildren": {
    "SLOT_ELEMENT_ID": ["child_id_1", "child_id_2"]
  },
  "children": ["child_id_1", "child_id_2"]
}
```

Without `slotChildren`, children of the instance are not rendered in the slot.

## Safe Edit Patterns

Small styling fix:

1. Get content.
2. Identify element ID/class.
3. Check whether the fix belongs in Bricks control, Bricks global class, Core Framework token, or page CSS.
4. Patch only the relevant settings or CSS.
5. Verify front end.

New section:

1. Inspect nearby sections for structure/class conventions.
2. Confirm the correct Bricks elements for each wrapper/content role.
3. Build a semantic Bricks subtree.
4. Use BEM classes and Core Framework variables.
5. Use Bricks controls for normal layout, sizing, spacing, radius, position, z-index, and image fit.
6. Reserve CSS for pseudo-elements, complex selectors, container queries, and unsupported properties.
7. Insert before/after the requested anchor.
8. Verify desktop/mobile.

Core Framework token issue:

1. Confirm variable exists and value is wrong.
2. Decide if the fix is Core Framework project-level.
3. Back up/export or get approval for broad design-system edits.
4. Save/regenerate CSS.
5. Verify all known affected pages.

## Verification

Always verify:

- Bricks content saved correctly.
- Front-end HTML contains expected classes.
- Core Framework variables resolve.
- CSS specificity is as intended.
- Mobile layout works.
- Bricks controls, Bricks defaults, and Core Framework defaults are not unnecessarily overwritten.
- Builder editability remains intact.
- No unrelated templates/global classes/theme styles changed.

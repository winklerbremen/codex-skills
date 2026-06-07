# Bricks Builder Through Novamira

Use this reference for Bricks-specific live editing through Novamira MCP.

## Inspect First

Start with read-only calls:

- List active plugins and target post/template metadata.
- Use `novamira/bricks-get-content` for the requested post ID and area.
- Use `novamira/bricks-list-settings` before changing page or template settings.
- Use `novamira/bricks-list-elements` before creating unfamiliar element types.
- Inspect front-end HTML after edits.

Do not infer the builder from plugin presence alone. A site can have Bricks installed while a specific page uses Gutenberg, Etch, or static migrated content.

## Element Shape

Bricks element objects need:

```json
{
  "id": "abc123",
  "name": "heading",
  "parent": 0,
  "settings": {
    "text": "Headline",
    "tag": "h2"
  }
}
```

Use `parent` references for hierarchy. The `children` array is computed automatically by Novamira.

Common element names include:

- `section`
- `container`
- `div`
- `block`
- `heading`
- `text-basic`
- `text`
- `button`
- `image`
- `icon`
- `video`
- `divider`

## Preferred Abilities

- `novamira/bricks-get-content`: read current element tree.
- `novamira/bricks-list-elements`: discover element types and controls.
- `novamira/bricks-list-settings`: discover page/template/global settings schema.
- `novamira/bricks-insert-content`: add a new subtree without replacing the full area.
- `novamira/bricks-patch-elements`: targeted fixes to existing elements.
- `novamira/bricks-set-content`: replace a full area only when intentional.
- `novamira/bricks-set-settings`: set page/template/global settings and custom CSS/JS.

## Critical Value Formats

Bricks silently drops many wrong value shapes. Use these formats:

Number and unit controls:

```json
"_rowGap": "24px",
"_columnGap": "16px",
"_gridGap": "24px",
"_maxWidth": "860px"
```

Do not use:

```json
{"value": 24, "unit": "px"}
```

Typography:

```json
"_typography": {
  "font-size": "var(--h2)",
  "line-height": "1.15em",
  "font-weight": "700",
  "text-align": "center",
  "color": {"raw": "var(--base)"}
}
```

Spacing:

```json
"_padding": {
  "top": "var(--space-l)",
  "right": "var(--space-m)",
  "bottom": "var(--space-l)",
  "left": "var(--space-m)"
}
```

Border:

```json
"_border": {
  "width": {"top": "1px", "right": "1px", "bottom": "1px", "left": "1px"},
  "style": "solid",
  "color": {"raw": "var(--base-light)"},
  "radius": {"top": "var(--radius-m)", "right": "var(--radius-m)", "bottom": "var(--radius-m)", "left": "var(--radius-m)"}
}
```

Background:

```json
"_background": {
  "color": {"raw": "var(--base-ultra-light)"}
}
```

Global class references:

```json
"_cssGlobalClasses": ["clshov", "u-card"]
```

Use a flat list of class IDs. Do not pass `{id,name,settings}` objects as element class references.

Custom CSS:

```json
"_cssCustom": "%root% { display: grid; gap: var(--content-gap); }"
```

Use `%root%` and include a complete rule block with braces.

Flex:

```json
"_display": "flex",
"_direction": "column",
"_alignItems": "stretch",
"_justifyContent": "flex-start",
"_rowGap": "var(--content-gap)"
```

Grid:

```json
"_display": "grid",
"_gridTemplateColumns": "repeat(3, minmax(0, 1fr))",
"_gridGap": "var(--grid-gap)"
```

## CSS And JS

Do not use a Bricks `code` element for CSS or JS. It can render visible code or make editing brittle.

Use page-scoped CSS:

```json
{
  "post_id": 123,
  "settings": {
    "customCss": ".my-section { background: var(--base-ultra-light); }"
  }
}
```

Use JS keys only when necessary:

- `customScriptsHeader`
- `customScriptsBodyHeader`
- `customScriptsBodyFooter`

There is no generic `customJs` key.

## Safety

- Back up or capture current Bricks content before broad structural edits.
- Prefer patch/insert operations over full area replacement.
- Keep global class edits rare and explicit.
- Do not convert a page from another builder to Bricks unless the user asks for migration.
- Verify after saving with front-end HTML and, when possible, screenshots.

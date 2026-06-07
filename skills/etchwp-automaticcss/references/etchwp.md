# EtchWP Reference

Official docs reviewed on 2026-04-28 and refreshed against the Etch changelog/docs on 2026-05-07: `https://docs.etchwp.com/`, `https://etchwp.com/changelog/`

## Mental Model

Etch is a Unified Visual Development Environment for WordPress. It exposes and manipulates real HTML, CSS, PHP, and JavaScript while authoring Gutenberg-compatible blocks. Prefer clean markup and WordPress-native content over page-builder-only abstractions.

Etch is built around:

- Transparent editing of real code.
- Auto Block Authoring for Gutenberg/client editing.
- Clean output without hidden wrappers.
- Components, loops, templates, dynamic data, and dynamic elements.

## Setup Assumptions

Etch requires the Etch plugin and its companion Etch Theme for proper operation. The Etch Theme is a minimal blank block theme that provides the required hooks and structure. Avoid theme switching during or after an Etch build because WordPress theme switching can break layouts and content assumptions.

## Components

Use components for repeated, self-contained UI patterns: cards, headers, footers, CTAs, query cards, feature blocks, pricing rows, and template fragments.

Component guidance:

- Define props for any value that varies per instance.
- Keep component markup semantic.
- Map props to text, image, link, boolean, class, or attribute values.
- Use a component statically by filling prop fields manually.
- Use a component dynamically by placing it in a loop/template and passing dynamic data keys into props.

Critical scope rule:

- Components cannot access loop or template keys unless those values are explicitly passed as props.
- Do not use `{item.title}`, `{post.title}`, or `{this.title}` directly inside component internals unless the component's own context provides them.
- In a loop, pass values such as `{item.title}`, `{item.image.id}`, `{item.permalink.relative}`, or `{item.excerpt}` into the component instance props.


## Component Props and Variants

Etch props fall into two practical groups:

- Data props: labels, headings, URLs, media, numbers, text, alt text.
- Behavior props: booleans, select options, visibility toggles, style variants, layout options.

Useful prop types:

- Text: free string values such as labels, URLs, CSS token values, or ARIA labels.
- Boolean: true/false toggles for conditions or state attributes.
- Select: dropdown options written as `Label : value`, one per line. Keep spaces around the colon. Referencing the prop outputs the value.
- Media: use URL mode for direct `src` values such as SVG icons; use ID mode when an Etch dynamic image or WordPress lookup should resolve the asset.
- Group: structured object under one key, e.g. `{props.hero.title}`.
- Group Repeater: create a Group Prop, then use the Etch UI action Make Repeatable. Loop it with `{#loop props.features as feature}`; do not write `{#loop {props.features} as feature}`.
- Condition Prop: editor-only prop organization that shows/hides nested prop controls. It does not create a data wrapper/key.
- Slot: a drop zone for arbitrary per-instance content. Use slots when content shape is open-ended; use props when the allowed content is controlled and typed. In HTML/code syntax, `{@slot body}` is valid; in saved block syntax, use `wp:etch/slot-placeholder`.

Recent prop/editor notes from Etch 1.4.x:

- Condition properties are supported in the Etch Builder and Gutenberg. As of 1.4.17, component blocks with condition properties no longer break Gutenberg after the 1.4.16 regression.
- Group and repeater props are safer than early 1.4 builds: group/repeater values expose to parent components correctly, no longer revert dynamic data when edited, and repeater items can be reordered in instances by drag-and-drop.
- Component property inputs now expose `data-etch-property-key` and `data-etch-property-type` attributes in the builder/Gutenberg, useful only for targeted editor integrations. Do not rely on these for frontend styling.
- Component properties can have descriptions. Use descriptions for reusable components when a prop's expected value or token format is not obvious.
- Boolean and loop props can be populated by target-clicking a block in the canvas/structure panel; Etch wraps the target in a condition or loop. Verify the resulting saved block structure before using it as a reusable pattern.
- Select-property options can be edited in a larger textarea; keep options in `Label : value` format, one per line.
- Empty component instance inputs show the component default. Remember that an empty-looking value may still resolve to the default at render time.

Mapping rules:

- Text/attribute contexts need braces: `{props.heading}`, `href="{props.url}"`, `src="{props.iconUrl}"`.
- Existing expression contexts do not need extra braces: `{#if props.showIcon}`, `{#loop props.items as item}`.
- In saved Etch condition block data, condition operands should use the bare expression (`props.showIcon`) rather than `{props.showIcon}`.
- In saved `style` attributes, keep CSS custom properties as literal `--token`; do not leave escaped `u002du002d` text, because it renders as `u002du002d...` and the CSS variable does not apply.

Variation guidance:

- Use select props plus `data-*` attributes for visual variants such as `data-variant`, `data-size`, `data-align`, or `data-icon-position`.
- Keep BEM class names component-owned and portable. A reusable CTA button should use names such as `.cta-button`, `.cta-button__icon`, and `.cta-button__label`, not names tied to where it first appears.
- Use modifier classes only when they are authored intentionally as part of the component API. Do not keep old modifier classes if `data-*` selectors now own the variants.
- Etch component variations are prop-driven, not duplicated snapshot variants. Prefer one component controlled by props, conditional logic, and `data-*` attributes over separate duplicate components for every state.

## Component Namespace

Since Etch 1.4.15, component internals can read a `{component}` namespace:

- `{component.id}`: current component ID.
- `{component.name}`: component display name.
- `{component.key}`: component HTML key.
- `{component.defaults.<propKey>}`: resolved default value for a prop, including nested group defaults.

Use cases:

- Add debug/editor-only data attributes such as `data-component="{component.key}"` when useful.
- Compare an instance prop with a default: `{#if props.title !== component.defaults.title}`.
- Read nested group defaults via dot notation.

Scope rule:

- `{component}` is only available inside the component's own HTML, like `{props}`. A page, template, or parent component cannot read a child component's `{component}` namespace.
## Dynamic Data

Common scopes:

- Templates/current post: `{this.key}`
- Loops: `{item.key}` by default, or the alias declared in the loop.
- Taxonomies: `{taxonomy.key}` and `{term.key}`
- Site/user/options data where available: `{site.key}`, `{user.key}`, `{options.key}`

Common post/item keys:

- `id`
- `title`
- `slug`
- `content`
- `excerpt`
- `permalink.relative`
- `permalink.full`
- `image`, `image.url`, `image.alt`
- `featuredImage.url`, `featuredImage.alt` where available in loop examples
- `date`
- `modified`
- `status`

Property syntax:

- Dot notation only for letters, digits, and underscores: `{this.title}`, `{item.acf_field}`.
- Use bracket notation for spaces, dashes, and unusual keys: `{item["post-meta"]}`, `{this["page title"]}`.
- Arrays can be indexed or looped: `{this.categories[0].name}` or `{#loop this.categories as category}`.

Debugging:

- Output `{this}` or the current loop object temporarily to inspect available data.
- Remove debug output before final delivery.


## Dynamic Data Modifiers

Use modifiers directly on paths when formatting output:

- `.dateFormat("F j, Y")`
- `.numberFormat(2, ".", ",")`
- `.toUpperCase()`
- `.toLowerCase()`
- `.toString()`, `.toInt()`, `.toBool()`
- `.trim()`, `.ltrim()`, `.rtrim()`, `.stripTags()`
- `.urlEncode()`, `.urlDecode()`
- `.truncateChars(count)`, `.truncateWords(count)`
- `.round(precision)`, `.ceil()`, `.floor()`
- `.concat(...)`, `.length()`, `.reverse()`, `.at(index)`, `.slice(start, end)`
- `.indexOf(value)`, `.pluck("key")`, `.keys()`, `.values()`
- `.split(separator)`, `.join(separator)`
- `.replace(search, replace)` and `.replaceAll(search, replace)` since 1.4.9
- `.applyData()` for advanced cases where a stored value itself contains dynamic data placeholders.
- `.unserializePhp()` for serialized PHP strings when unavoidable.

Prefer formatting at render time for presentational output. Keep canonical data in WordPress unchanged.

Examples:

```text
{item.slug.replaceAll("-", " ").toUpperCase()}
{this.title.truncateWords(8)}
{item.tags.pluck("name").join(", ")}
{this.etch.header.applyData()}
```


## Conditional Logic in Components

The Condition element is a ghost element: it outputs no wrapper DOM and renders its children only when the condition is truthy.

Use conditions for:

- Optional icons/media where hidden markup should not exist.
- Optional links/buttons/sections controlled by boolean props.
- Clean conditional wrappers, especially around slot areas.
- New-tab vs normal-link variants when different attributes are required.

Rules:

- Prefer simple boolean expressions: `props.showIcon`, `!slots.footer.empty`, `user.loggedIn`.
- In the visual/code syntax, conditions look like `{#if props.showIcon}`. In saved block JSON, the operand is the bare expression `props.showIcon`.
- Do not use `{props.showIcon}` as a saved condition operand; that can evaluate as empty in server-side rendering.
- Keep condition nesting shallow. If a component needs many nested conditions, simplify the prop model or split the component.
- Use CSS state selectors for styling variants, not for removing optional content unless keeping the element in the DOM is intentional.
## Loops

Etch loops render repeated structures from data sources.

Loop sources:

- WP Query for reusable post/custom-post lists.
- Main Query for archive, taxonomy, search, and post-type archive templates that should inherit WordPress routing.
- JSON for static or external shaped data.
- WP Terms for taxonomy terms.
- WP Users for users.

Loop pattern:

```html
<ul class="post-grid">
  {#loop recent-posts as item}
    <li>
      <article class="post-card">
        <h2>{item.title}</h2>
        <p>{item.excerpt}</p>
        <a href="{item.permalink.relative}">Read more</a>
      </article>
    </li>
  {/loop}
</ul>
```

Access an index with a second alias:

```html
{#loop posts as post, index}
  <article data-index={index}>
    {post.title}
  </article>
{/loop}
```

Use components inside loops by passing props:

```text
Post Title prop: {item.title}
Post Image prop: {item.image.id}
Post Link prop: {item.permalink.relative}
```


## Slots

Slots are per-instance drop zones for arbitrary content. They are best when the allowed content shape is unknown or intentionally flexible.

Slot rules:

- Define a slot in the component where external content should appear.
- Do not put final slot content inside the component editor; slot content belongs to each component instance.
- Use the `slots` object to check emptiness, e.g. `slots.body.empty`.
- Empty slots render no front-end content by default.
- For fallback content, render the fallback conditionally when the slot is empty, then render the slot.
- For wrappers around slot content, conditionally render the wrapper only when the slot is not empty to avoid empty markup.
## Dynamic Elements

Use the Dynamic Element when a tag must be configurable without conditional duplicates:

```html
<etch:element tag={props.headingLevel}>
  {props.heading}
</etch:element>
```

Good uses:

- Section intro component where heading can be `h1`, `h2`, or `h3`.
- Card component where the heading can be `h2`, `h3`, or `h4` depending on section context.
- CTA/component wrapper where the outer tag can be `section`, `aside`, or `div`.

Saved block rule:

- Use the real `wp:etch/dynamic-element` block, not a normal `wp:etch/element` with a dynamic `tag` value.
- In saved block JSON for component heading tags, put the dynamic tag expression in both the top-level `tag` and `attributes.tag`.
- `attributes.tag` drives the front-end output; the top-level `tag` should match it so the builder/editor state stays consistent.
- Do not leave `attributes.tag` as a static `div`; Etch will render a `div` even when the prop is set to `h2`, `h3`, or `h4`.

```html
<!-- wp:etch/dynamic-element {"metadata":{"name":"Heading"},"tag":"{props.headingTag}","attributes":{"tag":"{props.headingTag}","class":"problem-card__heading"},"styles":["problem-card__heading"]} -->
<!-- wp:etch/text {"content":"{props.heading}"} /-->
<!-- /wp:etch/dynamic-element -->
```

## Dynamic Image

Use Etch Dynamic Image / `<etch:img />` when you want Etch to handle WordPress images by media ID. It is compatible with component props and recommended for dynamic WordPress image handling. Native `<img>` remains valid when you need direct HTML control.

For dynamic cards, prefer passing the image ID or image object expected by the Etch image control through props rather than hardcoding image URLs.

Practical attributes:

- `mediaId`: WordPress attachment ID or expression such as `{props.imageId}`.
- `useSrcSet`: enable WordPress responsive image output where appropriate.
- `maximumSize`: WordPress image size such as `full`, `large`, `medium_large`, `medium`, or `thumbnail`.
- `alt`: pass meaningful alt text; if empty, verify whether the media library fallback is acceptable.
- SVGs may not need raster dimensions/srcset. Use Etch SVG instead when the asset is an inline icon that should inherit color.

Recent image/media notes from Etch 1.4.x:

- Media props can reference media-library assets or URLs, but media props themselves do not currently support `srcset`. Use Dynamic Image with a media ID when responsive image output is needed.
- Media ID props should remain IDs. Since 1.4.15, Etch no longer silently rewrites media ID properties to attachment URLs at resolve time.
- Dynamic Image placeholders preserve classes and attributes set on the block as of 1.4.16, so styling in the builder should match resolved frontend images more reliably.
- Dynamic images always render an `alt` attribute, even when no media-library alt is set. Still provide meaningful alt text whenever the image conveys content.
- Dynamic SVG elements can accept a WordPress attachment ID in `src` and resolve it to the attachment URL as of 1.4.15. This is useful for SVG icon props, but raster/image content should still use Dynamic Image.
- SVG media picker and strip-colors behavior were improved in 1.4.17. For icon SVGs that should inherit text color, still verify stripped colors and `currentColor` output.

## Extending Dynamic Data

Etch exposes PHP filters for adding custom dynamic data:

- `etch/dynamic_data/post` extends `{this.CUSTOM}` and loop item post data.
- `etch/dynamic_data/user` extends `{user.CUSTOM}`.
- `etch/dynamic_data/term` extends `{term.CUSTOM}`.
- `etch/dynamic_data/option` extends `{options.CUSTOM}`.

Always return an array from these filters.

Example:

```php
add_filter('etch/dynamic_data/post', function ($data, $post_id) {
    $data['custom_data'] = [
        'field_name' => get_post_meta($post_id, 'field_name', true),
    ];

    return $data;
}, 10, 2);
```

## Implementation Checklist

- Choose content model first: post type, taxonomy, fields, options, or static content.
- Build reusable UI as components with props.
- Build listing pages with loops and scoped dynamic keys.
- Use Main Query on archive templates.
- Use Dynamic Element for configurable semantics.
- Use Dynamic Image for WordPress media IDs.
- Verify front-end output and editor/client editability.







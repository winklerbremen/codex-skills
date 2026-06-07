---
name: etchwp-automaticcss
description: Expert workflow for building, reviewing, and debugging WordPress sites that use EtchWP/Etch and Automatic.css (ACSS). Use when Codex is asked to create or modify Etch pages, Etch components, dynamic data, loops, templates, WordPress layout markup, or styling with Automatic.css variables, utility classes, spacing, colors, buttons, sections, responsive systems, and scalable page-builder architecture.
---

# EtchWP + Automatic.css

Use this skill when working on WordPress builds that combine EtchWP and Automatic.css. Treat Etch as a transparent visual development environment that authors real HTML/CSS/PHP/JS and Gutenberg blocks, and treat ACSS as the design-token and utility framework that keeps layout, spacing, colors, and components consistent.

## First Moves

1. Confirm the live plugin/theme context when possible: Etch plugin active, Etch Theme active if the site is Etch-based, Automatic.css active, and version numbers.
2. Prefer WordPress-native content modeling: CPTs, taxonomies, post meta/custom fields, options, templates, loops, and dynamic data instead of hardcoded repeated content.
3. Choose semantic HTML before layout: use real `section`, `header`, `main`, `article`, `nav`, `ul/li`, `ol/li`, `figure/figcaption`, headings, links, and buttons according to meaning.
4. Use `table` only for real tabular data with row/column relationships. Use `ul`/`ol` for lists of labels, features, audiences, steps, or cards, then style them with grid/flex as needed.
5. Use ACSS global variables and dashboard-controlled systems before local one-off CSS.
6. Prefer reusable Etch components with props for repeated patterns. Pass loop/template data into props instead of reading outer loop keys inside the component.
7. Prefer component/section-owned BEM custom classes powered by ACSS variables over utility classes. ACSS is not utility-first; utilities are secondary helpers only when the generated project CSS actually includes them and they improve clarity.
8. Set every CSS declaration deliberately. Before adding resets, margins, font sizes, colors, display, padding, or other local CSS, check whether ACSS, the theme, inherited values, browser defaults, or an existing component class already provide the intended behavior. Do not add mechanical declarations such as `margin: 0` unless the component layout truly needs them. Treat custom CSS as the design delta: avoid restating browser/framework defaults such as `font-weight: 400`, `background: transparent`, `border: 0`, duplicate `display`/centering rules, or defensive paragraph/list spacing when no competing rule needs to be overridden.
9. Treat existing working structures as protected. Do not change existing components, sections, classes, global styles, properties, instances, content, storage formats, or migrations as an "optimization" without first explaining the proposed change, its impact, and waiting for user approval. When asked to use an existing component, insert/use the component instance only; do not edit the component definition unless explicitly approved.
10. Use `!important` only as a last resort when a third-party plugin or generated framework style cannot be overridden safely through ownership, selector specificity, source order, variables, or component API. If `!important` is unavoidable, keep it narrowly scoped, document the reason, and prefer consolidating those overrides in one place.

## Reference Loading

Load only the reference needed for the task:

- `references/etchwp.md` for Etch concepts, components, dynamic data, loops, templates, dynamic elements, and WordPress integration.
- `references/automaticcss.md` for ACSS variables, spacing, section structure, colors, buttons, utilities, and maintainable CSS patterns.
- `references/combined-workflow.md` for practical Etch + ACSS implementation patterns, checklists, and examples.

## Default Architecture

Use this semantic section layout unless the project already has a stronger convention. Give every section and meaningful child element component-owned BEM classes:

```html
<section class="section-name section--m">
  <div class="section-name__inner">
    ...
  </div>
</section>
```

Style reusable pieces with custom classes:

```css
.feature-card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--base-ultra-light);
}
```

Prefer ACSS variables inside BEM classes for scalable styling:

```html
<section class="services">
  <div class="services__inner">
    ...
  </div>
</section>
```

```css
.services {
  padding-block: var(--section-space-xl);
  padding-inline: var(--gutter);
  background: var(--black);
}

.services__inner {
  display: grid;
  gap: var(--container-gap);
  max-inline-size: var(--content-width);
  margin-inline: auto;
}
```

Use ACSS utilities only where they express a stable system decision and are actually generated in the project:

```html
<section class="services section--xl bg--base-ultra-light">
  <div class="services__inner grid--3 grid--s-1 grid-gap">
    ...
  </div>
</section>
```

## Etch Rules

- Use dynamic data keys in their correct scope: templates use `{this.key}`, loops use `{item.key}` by default, taxonomies use `{taxonomy.key}` and `{term.key}`.
- Do not reference loop/template keys directly inside a component. Define props and pass `{item.title}`, `{item.permalink.relative}`, image IDs, booleans, etc. into the component instance.
- For Single templates, preserve Etch as the rendering layer. Build or update a real WordPress/Etch block template (`wp_template`) instead of replacing the design with a shortcode/PHP renderer. Custom PHP may normalize or expose data, but it should not own component markup, styling, headers, buttons, or page presentation when the site is Etch-authored.
- In Etch-backed WordPress templates, use the active theme association for `wp_template` posts. Etch's template service stores templates in the `wp_template` post type, publishes them, and associates them to the current theme through the `wp_theme` taxonomy. Inspect existing templates with `get_block_templates()` and existing `wp_template` posts before creating or replacing one.
- ACF fields are exposed under the `acf` namespace: in templates use `{this.acf.field_name}`, and in loops use `{item.acf.field_name}`. For nested ACF groups/clones/repeaters, prefer clean underscore-only field names so dot notation resolves predictably; use bracket notation only when a key contains unsupported characters.
- ACF Flexible Content can be looped from `this.acf.flexible_field_name`; each row has `acf_fc_layout`. Render the allowed layouts with explicit conditions and preserve Etch block/element structure inside each branch.
- If native ACF output is awkward for Builder props or loops, use `etch/dynamic_data/post` as a thin data adapter that returns an array and adds stable custom keys such as `{this.lp.section.field}`. Keep that adapter presentation-free.
- Inside a component's own HTML, Etch 1.4.15+ exposes `{component.id}`, `{component.name}`, `{component.key}`, and `{component.defaults.<propKey>}`. Use this only inside the component; parent pages/templates cannot read a child component's `{component}` namespace.
- Use `<etch:img />` for WordPress media-ID based images when dynamic image handling is needed.
- Use Etch Dynamic Element when a component needs a configurable semantic tag. In saved block JSON, set both top-level `tag` and `attributes.tag` to `{props.headingTag}`; do not leave `attributes.tag` as static `div`.
- For component repeater fields, create a Group Property in the Etch UI and use Make Repeatable. In saved component metadata this becomes `type: { primitive: "array", specialized: "repeater" }` with nested `properties`; do not hand-author a plain `primitive: "array"` when the editor should expose repeatable subfields.
- Do not programmatically inject repeater prop array values into an Etch component instance unless you have verified that exact saved shape from the Builder UI. A manually written array may render on the frontend but can break Builder selection/props editing. Prefer adding the repeater property and loop to the component, then let the user/Builder create and save the instance items.
- When a component instance already has a Builder-managed repeater prop, preserve the real component instance instead of rebuilding the rendered HTML. For static repeater data, use the Builder-compatible instance shape observed in a working saved block: `data.items` stores the repeater rows and the item values must be encoded in the corresponding component prop exactly as Etch expects. For example, an Icon List-style component with `hasCfLoop` false may require `data: "{{\"items\":\"{[{},{},{}]}\"}}"` plus the item value mapping/string in the component's repeater prop, rather than a direct PHP array or a local `<ul>` substitute. If unsure, inspect a working Builder-authored instance of the same component before writing.
- Etch 1.4.x supports repeater editing in Gutenberg, drag-and-drop repeater reordering, target-mapping for boolean/loop props, and prop descriptions. Still inspect generated block structure after target-mapping or complex prop edits.
- For component slots in saved block syntax, use real Etch slot blocks. Existing Etch-authored components commonly store an empty placeholder as paired syntax such as `<!-- wp:etch/slot-placeholder {"name":"body"} --> ... <!-- /wp:etch/slot-placeholder -->`, with paired `wp:etch/slot-content` blocks in the component instance for inserted content. When debugging Builder issues, compare against a known working component before changing slot syntax; a syntactically valid self-closing placeholder is not automatically the safest Builder representation.
- In Etch-first projects, prefer Etch blocks/elements inside component slot content when the content is authored as part of the designed component system. Use WordPress core blocks such as `core/paragraph` inside slots only when free Gutenberg content editing is intentionally desired.
- When rendering a repeater prop inside a component, prefer an Etch loop block: `<!-- wp:etch/loop {"target":"props.items","itemId":"item"} --> ... <!-- /wp:etch/loop -->`. Verify the rendered output because raw text loop syntax may not execute in every block context.
- When updating Etch/Gutenberg `post_content` through WordPress PHP APIs, preserve block JSON escaping. Pass changed content through `wp_slash($content)` in `wp_update_post`; otherwise escaped component repeater values such as `{\"text\":\"...\"}` or `\u0022text\u0022` can be stripped to invalid literal `u0022textu0022` strings and Etch will render repeater-driven component content empty. Before and after any page-wide `post_content` update, check for literal `u0022` / `U0026` in affected content, parse every `wp:etch/component` `attributes` JSON with `json_decode`, and verify all repeater-driven component output in the frontend. Raw quotes inside component attribute values, such as `Kein "Weiter so"-Jahr.`, must be escaped or replaced with typographic quotes before saving.
- When replacing or moving an Etch component block inside a section, verify the surrounding block boundaries immediately. Count or inspect the parent `wp:etch/element` closers so that content-column wrappers, grids, containers, and sections close in the same order as before. Missing one `<!-- /wp:etch/element -->` can collapse a two-column grid into one column or pull the next section into the previous section. Browser QA for such edits must include parent/child checks, next-section-outside-current-section checks, and grid-position checks, not only text rendering.
- When writing Etch CSS into `etch_styles` or style metadata, store CSS with real newline characters, not literal `\n` sequences. In PHP use double-quoted strings or arrays joined with "\n"; do not use single-quoted strings containing `\n` for multiline CSS.
- When generating saved block markup programmatically, preserve Etch's self-closing block syntax for leaf blocks such as `etch/text` and `etch/raw-html`: `<!-- wp:etch/text {...} /-->`. Saving them as paired/opening-only comments (`-->` without `/-->`) can make the frontend render only stray text fragments and drop the intended wrapper classes.
- Keep builder stability higher than cleverness. Avoid nesting `wp:etch/loop` inside `wp:etch/dynamic-element` unless specifically verified in the builder; use a normal `wp:etch/element` wrapper first, then add dynamic tags only after the component is stable.
- Avoid raw HTML inside Etch conditions for editable components. Prefer nested `wp:etch/element` and `wp:etch/text` blocks so the builder can parse the tree reliably.
- Use loop data sources appropriately: Main Query for archive/search templates, WP Query for reusable lists, WP Terms for term listings, WP Users for user listings, JSON for static or external shaped data.
- When a dynamic key does not resolve, output the parent object temporarily (`{this}` or current loop object) to inspect available data, then remove the debug output.



## Etch Element Defaults And Reuse

Before building a section, inspect the active site's reusable Etch components/classes and default Etch element styles. Do not recreate patterns that already exist, especially CTA/button components or global classes.

Use Etch's native elements deliberately:

- `etch/element` for semantic/static tags and layout primitives such as section, container, div/flex-div, heading-like wrappers, anchors, lists, and structural wrappers.
- `etch/text` for editable text content.
- `etch/dynamic-element` when the semantic tag must be configurable, such as component heading levels.
- `etch/dynamic-image` for WordPress media-library images that need media IDs, srcset, sizes, alt text, loading, and decoding behavior.
- `etch/svg` for SVG/icon output, especially when SVG media IDs or currentColor styling are useful.
- `etch/condition`, `etch/loop`, and slots for real conditional, repeated, or open content structures.

Always account for active Etch default element styles before adding custom CSS. In Etch 1.4.x projects, verify the site-specific `etch_styles` option, but common defaults include:

- `[data-etch-element="section"]`: full inline size, flex column, centered children.
- `[data-etch-element="container"]`: full inline size, flex column, `max-inline-size: var(--content-width, 1366px)`, centered with `margin-inline: auto`.
- `[data-etch-element="flex-div"]`: full inline size, flex column.
- `[data-etch-element="iframe"]`: full inline size, auto height, 16:9 aspect ratio.

Because the Container element may already own content width and centering, avoid duplicating `max-width`, width, and margin-centering rules on every custom `__inner` class. Add custom layout CSS only when the section design intentionally overrides or extends the default element behavior. Likewise, verify whether gap/spacing is already supplied by the chosen element, existing class, or ACSS token before adding new `gap`, padding, or margin declarations.
## Component Authoring Rules

- Plan component props as either data props (label, href, image/media, text) or behavior props (showIcon, newTab, variant, size, alignment). Keep the public API small and predictable.
- Map props with braces in normal text/attribute contexts, e.g. `href="{props.url}"` or `{props.heading}`. Inside Etch expression contexts such as `{#if ...}`, `{#loop ...}`, and saved condition operands, use the expression without extra braces: `props.showIcon`, `props.items`, `props.variant === "primary"`.
- For booleans that truly hide/show markup, prefer a real Etch Condition element around the optional element so the markup does not render. Use CSS toggles only when the element must remain in the DOM for layout or interaction reasons.
- For boolean presence attributes such as `open`, `disabled`, `checked`, `required`, and `selected`, do not set the attribute to a dynamic string like `open="{props.openByDefault}"`. In HTML, the mere presence of the attribute is truthy, so false values can still render as active. Use conditional element branches instead: one branch with the presence attribute and one branch without it.
- For select props controlling styling, prefer `data-*` attributes plus component CSS selectors over many modifier classes. Example: `data-variant="{props.variant}"` with `.cta-button[data-variant="secondary"]` rules.
- Use a neutral wrapper around clickable components when the builder has trouble selecting an anchor root. Example: `span.cta-button-wrapper` outside `a.cta-button`.
- For sticky/fixed header components, avoid making an interactively dense semantic `<header>` component root own the sticky/fixed behavior when it causes Etch Builder canvas selection problems. Use a neutral root wrapper such as `.landing-header-shell` for sticky positioning and container queries, then place the real semantic `<header>` inside it. Keep the wrapper in normal document flow with `position: sticky` whenever possible; avoid `position: fixed` on page-level component instances until Builder clickability has been verified. Expose the behavior through a prop/data attribute if needed, but preserve Builder selection first.
- Use component-owned BEM names (`cta-button`, `cta-button__icon`, `cta-button__label`) instead of context-specific names (`bed-nav__button`) when the component is meant to be reused outside one section.
- When implementing `newTab`, avoid empty `target`/`rel` attributes. Use conditional link variants: normal link without those attributes, and new-tab link with `target="_blank"` plus `rel="noopener noreferrer"`.
- For user-selectable SVG icons in Etch components, use a Media Prop (`wpMediaId`) and render it with the Etch SVG element/widget via `src="{props.icon}"`. Etch can resolve SVG attachment IDs and inline them. Use text/URL props for SVGs only when the icon must be a fixed external URL or data URI.
- If the SVG should inherit text color, strip colors where appropriate and style it with `currentColor`.
- For normal WordPress/media-library images in Etch components, use a Media Prop (`type: { primitive: "string", specialized: "wpMediaId" }`) and render it with `wp:etch/dynamic-image` / `<etch:img />` using `mediaId="{props.image}"`, `useSrcSet`, `maximumSize`, and meaningful `alt` handling. Do not model regular component images as plain URL text props unless there is a specific reason.
- Do not assume a Media Prop alone provides responsive `srcset`; current Etch docs say the media prop itself does not support `srcset`. Use Dynamic Image with a media ID when responsive output matters.
- Use URL-mode/text props only for special direct `src` values such as external fixed assets or SVG/icon sources where the Etch SVG element expects a URL.
- Dynamic SVG elements in Etch 1.4.15+ can accept a WordPress attachment ID in `src` and resolve it to the attachment URL. Use this for SVG icon props where appropriate; use Dynamic Image for raster/content images.
- If a component evolves from a test component, migrate the verified structure into the live component ID, remove temporary component instances, then delete obsolete style keys and test components.
- Do not create a new component opportunistically without user approval. If implementation reveals a repeated pattern, propose the component API first and continue with local/static markup only if the user has not approved componentization.
- When reusing an existing component, treat its props and framework tokens as the styling contract. Do not override its variant, size, color, spacing, hover, or focus variables from a page/section wrapper unless the user explicitly asks for a contextual override. If a component needs a new visual option, propose a component-level prop/variant change first.
- For reusable Etch components, prefer small grouped public APIs over many flat props. Useful group lanes are `content` for text/HTML and accessibility labels, `display` for layout/variant/custom class, `spacing` for token-sized gaps, `style` for visual variants/sizes, `icon` for media/icon behavior, `link` for href/new-tab behavior, and `data` for static repeater defaults.
- Store component instance group prop values in Builder-compatible expression strings like `{{"variant":"secondary","size":"m"}}`. Dynamic values can live inside those JSON strings, for example `{{"label":"{this.acf.hero.cta_label}"}}` or `{{"mediaId":"{item.icon}"}}`; keep the outer group wrapper intact so the Builder Inspector remains editable.
- For components that can render either static repeater props or a custom-field loop, expose an explicit boolean such as `hasLoop` / `hasCfLoop`, a loop target string such as `loopProp` / `cfLoop`, and keep static repeater data behind a condition property. In the component markup, branch between `props.data.items` and `props.general.cfLoop` with Etch conditions and loops.
- For select-like styling props, prefer `data-*` attributes on the component root (`data-variant`, `data-layout`, `data-size`, `data-icon-mode`, etc.) and style them in the component's attached selector CSS. This keeps section templates from needing contextual CSS overrides.
- For optional component parts such as eyebrow, handwritten text, body slot, icons, and new-tab links, use boolean props plus Etch Conditions. Do not leave empty markup visible just because a prop is blank.
- Etch Builder-visible styling belongs primarily in attached selector styles (`etch_styles`) referenced by the block's `styles` array. Etch's CSS Panel is the single source of truth for a selector and is designed to support CSS nesting, descendant selectors, pseudo-classes, pseudo-elements, and responsive queries inside the attached selector block.
- Store class CSS with real newline characters, never literal `\n`; literal backslash-n breaks reliable Builder/frontend CSS interpretation. After programmatic updates, verify both the `etch_styles` data and the rendered Builder/frontend CSS.
- Do not move section/component-owned responsive rules, hover/focus states, or `::before`/`::after` decoration into `etch_global_stylesheets` just to make the frontend work. Doing so makes the Builder feel disconnected because the CSS no longer lives with the selector the user is editing.
- Use `etch_global_stylesheets` sparingly for truly global CSS only: custom media definitions, global resets, global tokens, page-wide one-offs not owned by a selector, or cross-cutting exceptions that cannot reasonably attach to a specific element.
- When a change appears on the frontend but not in Etch Builder, check in this order: the selected element has a valid selector pill; the block references the correct `styles` key; the CSS lives in `etch_styles` rather than only a global stylesheet; the CSS uses valid nesting syntax; there are no literal `\n` sequences; no later component/variant rule overrides the intended variables; cache/preview reload has been cleared.
- Components are stored as `wp_block` posts with `etch_component_html_key` meta; always query those before recreating a reusable component. Insert existing components with `wp:etch/component` and `ref`, passing props through the block `attributes` object.
- For Etch component Group Properties in a `wp:etch/component` instance, do not save the instance value as a nested JSON object (for example `attributes: { content: { label: "..." } }`). Etch 1.4.x Builder may render it on the frontend but break component selection/Inspector controls. Store group values in the Builder-compatible expression string shape, for example `attributes: { content: "{{\"label\":\"Kontakt\",\"ariaLabel\":\"Kontakt aufnehmen\"}}" }`. The PHP resolver removes one outer brace from each side and parses the JSON, so the exact object shape is `{{"key":"value"}}`; do not generate `{{{"key":"value"}}}` with an extra JSON object brace wrapper. Verify Builder clickability after adding any group prop instance values.

## ACF-Backed Landing Page Templates

- For fixed marketing landing page sections, model ACF fields in the same order and shape as the visual template instead of using a generic Flexible Content intro. Flexible Content is useful when editors must compose arbitrary layouts; it is the wrong default when the design has a fixed text/list/button order and the client should only edit values.
- Prefer section-local direct fields such as `results.intro_text`, `results.intro_items`, `target.cta_label`, and `registration.regular_price` over a shared cloned `intro_body` when each section has a known composition. This keeps Etch bindings simple, avoids `acf_fc_layout` conditions, and reduces editor choice overload.
- Reusable Etch components still work well with direct ACF fields: pass the section fields into component props, and use component slots for custom body markup. Good patterns include an `Intro` component for eyebrow/heading/body slots, an `Icon List` component with `hasCfLoop`/`cfLoop` props for ACF repeaters, and card/button/stat components with stable group props.
- When converting an ACF Flexible Content field to direct fields, make a backup of the field group schema, target post values, and active `wp_template` first. Then migrate values into the new direct postmeta keys, delete only the old section-specific `*_intro_body*` meta after verification, and read back through ACF in a fresh request to avoid stale local field caches.
- For nested group repeaters in ACF, direct `update_field()` may not write rows correctly immediately after programmatic schema changes. If needed, write the native postmeta shape explicitly: `<group>_<repeater>` stores the row count, `_ <group>_<repeater>` stores the repeater field key, and each row stores `<group>_<repeater>_<i>_<subfield>` plus its underscore field-key reference. Verify with `acf-read-values`.
- After simplifying field names, audit the template for stale dynamic references. A template can contain both new direct fields and an old prop such as `this.acf.results.intro_body`; remove or neutralize stale references before final handoff so future edits do not rely on deleted ACF schema.
## ACSS Rules

- Treat Automatic.css as variable-first, not utility-first. Build semantic, component-owned BEM/custom classes with `var(--*)` tokens before reaching for utilities.
- Use ACSS utilities only when they are generated in the active project, semantically clear, genuinely reduce complexity, and intentionally chosen. Keep markup readable and avoid utility-heavy HTML when a BEM class with ACSS variables is more maintainable.
- Use variables for custom CSS: `var(--space-m)`, `var(--section-space-l)`, `var(--gutter)`, `var(--content-gap)`, `var(--container-gap)`, `var(--grid-gap)`, `var(--primary)`, `var(--base)`, `var(--radius-m)`, etc.
- Before writing custom component CSS, inspect the active generated ACSS files and the existing Etch selector styles when available. Keep only declarations that change the component's intended visual/layout behavior. If the browser, Etch element default, ACSS smart spacing, inherited typography, or an existing selector already provides the desired value, leave it implicit instead of documenting it in CSS.
- Think in ACSS roles rather than raw colors. Main palette roles are `primary`, `secondary`, `tertiary`, `accent`, `base`, and `neutral`; enabled roles generate shade variables such as `--primary-ultra-light`, `--primary-light`, `--primary`, `--primary-hover`, `--primary-dark`, and `--primary-ultra-dark`. Use brand/action roles intentionally: primary for the main action, secondary/supporting actions, accent sparingly, base for core text/backgrounds, neutral for structure.
- When using background utility classes and Automatic Color Relationships are enabled, foreground relationships only trigger from the class (for example `.bg--ultra-dark`), not from `background: var(--bg-ultra-dark)` in custom CSS. Use a background utility intentionally when automatic foreground/button relationship behavior is desired; use variables when the component owns all foreground styling itself.
- Use typography variables for sizes: `var(--h1)` through `var(--h6)` and `var(--text-xxl)` through `var(--text-xs)`. Prefer bridge variables such as `var(--h1-to-h3)`, `var(--text-xl-to-m)`, or `var(--text-l-to-s)` when a design needs stronger mobile reduction. Do not invent fluid `clamp()` values unless the ACSS token set cannot represent the design.
- For readable text color, prefer the framework text tokens before custom mixes: on dark backgrounds use `var(--text-light-muted)` for body copy and `var(--text-light)` for `strong`; on light backgrounds use `var(--text-dark-muted)` for body copy and `var(--text-dark)` for `strong`. For paragraph/rich text emphasis, keep `strong` font weight at `700` unless the design explicitly calls for a non-body display treatment.
- Respect Smart Spacing. ACSS removes default margins from headings, paragraphs, lists, and list items so parent `gap` can produce even spacing, then re-applies intelligent spacing in rich text/smart-spacing contexts. Do not add defensive paragraph/heading margins by habit; use `gap`, `var(--content-gap)`, or `.smart-spacing`/`.smart-spacing--off` deliberately based on the content model.
- ACSS adds medium section spacing to top-level `section` elements by default when default section styles are active. Do not add `padding-block`/`padding-inline` to every custom section class automatically; only set section padding when the design intentionally overrides the ACSS default, when the element is not a real top-level `section`, or when a section-specific size/background requires it.
- Use section spacing variables for deliberate section rhythm overrides: `var(--section-space-xs)` through `var(--section-space-xxl)` and bridge variables such as `var(--section-space-xl-to-s)` where available. Utility classes such as `.section--xl` are acceptable only if the active ACSS output includes them and a utility is clearer than a component-owned class.
- Keep the standard section structure: section owns the gutter/section spacing; the direct child container establishes content width and usually should not add extra inline padding unless it has its own panel/background styling. Use `var(--content-width)` or `var(--content-width-safe)` only when the active builder element does not already supply content width.
- Use contextual spacing variables: `var(--container-gap)` for multiple containers inside a section, `var(--content-gap)` for content rhythm inside a content wrapper, and `var(--grid-gap)` for grids/columns. If a contextual gap needs adjustment, prefer small `calc()` adjustments around the contextual variable rather than abandoning context.
- Prefer grid variables in reusable classes: `grid-template-columns: var(--grid-3)` or `var(--grid-auto-3)` with `gap: var(--grid-gap)`. Use grid utility classes only when they are generated in the current project and the layout is not a reusable component pattern.
- Use ACSS button classes/variables as a contract. Button classes use `.btn--{color}`, optional `.btn--outline`, optional light/dark variants, and size classes `.btn--xs` through `.btn--xxl` when generated. For custom button components, map props to ACSS button classes or reference ACSS button variables such as `--btn-background`, `--btn-text-color`, `--btn-padding-block`, `--btn-padding-inline`, `--btn-radius`, `--btn-font-size`, and `--focus-color`. Do not override component button variables from a section wrapper unless the user explicitly asks for a contextual exception.
- ACSS 4.x documentation recommends `color-mix()` or relative color syntax for transparencies instead of relying on old transparency token names. Even if a project config or class index contains legacy/transparency-looking names, verify the generated `automatic.css` and prefer `color-mix(in oklch, var(--color) X%, transparent)` for new custom CSS.
- Use the Card Framework when the project has it enabled and the pattern is a true card. ACSS 4.x cards can use the `color-scheme` property; `--light`/`--dark` suffixes on card class names can force a card scheme when color scheme support is active. Otherwise, custom BEM cards should still consume ACSS card/radius/color/spacing tokens.
- Avoid random pixel values in spacing, widths, radii, typography, and color when a token exists. Pixel values are acceptable for tiny physical details such as hairline borders or precise design artifacts, but should be deliberate.
- Before using an ACSS utility class, verify it exists in the site's generated `automatic.css`. If absent, implement the behavior in a BEM class using ACSS variables.
- For site-specific work, inspect `automatic_css_settings` and the generated files under `wp-content/uploads/automatic-css/` (`automatic.css`, `automatic-variables.css`, editor CSS) before relying on a class, token, default style, or dashboard feature.

## Responsive Rules

- Prefer container queries for reusable components and layout pieces whose behavior should react to the available parent/container width.
- In Etch, prefer container queries for section internals when the layout should react to the section/container width. Put `container-type: inline-size` and a stable `container-name` on the parent/root, then place `@container <name> (...)` rules in attached selector CSS for descendants. Remember that a container query cannot style the container element itself based on its own size; keep true page/viewport decisions as media queries.
- Default to container queries whenever they are the better fit: component internals, card grids, media/text compositions, sidebar/content splits, reusable section layouts, and any element that should adapt to its available container rather than the whole viewport. Reach for viewport media queries only when the decision is genuinely page/global, depends on viewport features, or the element cannot reasonably be placed under a query container.
- Use media queries for viewport-level decisions, page-level rhythm, global navigation behavior, and cases where no meaningful query container exists.
- Use a combination when appropriate: container queries for component internals and media queries as a reliable fallback or for broad viewport layout.
- Do not add container queries mechanically. First ensure the relevant wrapper is a query container or that the target environment already provides one.
- Mobile behavior must remain explicit: stack narrow grids, avoid text overflow, preserve touch targets, and keep builder-editable content selectable.

## Quality Bar

Before finishing, check:

- Existing working components, sections, classes, styles, properties, and instances were not changed beyond the approved scope. If an optimization would improve them, report it first and wait for approval.
- Markup is semantic and not div soup; tables are used only for true tabular data, while visual lists/grids use `ul`/`ol` plus BEM classes.
- Repeated design patterns are components or reusable custom classes.
- Dynamic data is scoped correctly and passed through props where needed.
- Spacing uses ACSS section, standard, or contextual variables.
- Typography uses ACSS typography variables unless there is a deliberate local exception.
- Colors and buttons use ACSS variables/tokens rather than raw hex values.
- Utility classes are present only when generated by the active ACSS configuration and clearer than variable-based BEM CSS.
- Mobile behavior is explicit: grids collapse, text fits, images have alt data, touch targets remain usable, and container queries/media queries are chosen deliberately.
- The result remains editable in WordPress/Etch and does not hide essential content in code-only islands.

## Sources Snapshot

Built from official docs reviewed on 2026-04-28 and refreshed on 2026-05-15:

- Etch Documentation: `https://docs.etchwp.com/` plus component props, mapping, slots, conditions, group/repeater, media prop, component namespace, dynamic data modifiers pages; Etch changelog through 1.4.17 reviewed on 2026-05-07
- Automatic.css Documentation: `https://docs.automaticcss.com/`, especially Spacing, Smart Spacing, Section Spacing, Typography/Text & Heading variables, Color System/Color Relationships, Button Classes/Variables, Grid Variables, Cards, and 4.x transparency/color guidance.
- Etch template/ACF guidance refreshed on 2026-05-27 against official Etch docs for Single Post Templates, dynamic component usage, Custom Fields, ACF Flexible Content, Dynamic Image, and dynamic-data hooks. Cross-checked against installed Etch 1.4.19 code paths: `classes/Services/TemplatesService.php`, `classes/RestApi/Routes/TemplatesRoutes.php`, `classes/Traits/DynamicData.php`, `classes/Traits/ACFData.php`.
## Continuous Learning

When working on any EtchWP + Automatic.css project, preserve durable lessons instead of leaving them only in the conversation. If a new pattern, constraint, bug, builder behavior, ACSS token rule, component strategy, responsive technique, or validation step will help future Etch/ACSS work, update the relevant global skill or create a focused new skill. Store project-specific facts in the project skill or project memory, but store generally reusable Etch/ACSS knowledge in this global skill or a dedicated global reference.





# EtchWP + Automatic.css Combined Workflow

Use this reference when implementing real pages, components, templates, loops, or CSS with both Etch and ACSS.

## Build Flow

1. Model the content in WordPress.
   - Page-only unique content can be static.
   - Repeated content should usually be posts, CPTs, terms, users, ACF/custom fields, or options.
2. Choose the Etch surface.
   - Static page: page canvas.
   - Repeated pattern: component.
   - Single item layout: template using `{this.key}`.
   - Archive/listing: template with Main Query loop.
   - Reusable feed/list: WP Query loop.
3. Choose semantic Etch markup before choosing layout mechanics.
4. Add component/section-owned BEM custom classes to meaningful wrappers and elements.
5. Add ACSS variables through those BEM/custom classes for system-level layout.
6. Style custom classes with ACSS variables.
7. Use ACSS utility classes only when they are generated in the active project and make the markup clearer.
8. Pass dynamic data into components via props.
9. Verify front-end output, mobile behavior, and editor maintainability.


## Reusable Component Workflow

1. Build or inspect the static markup first.
2. If a new reusable component seems useful and was not explicitly requested, stop and propose it before building: name, purpose, semantic root, props, slots, variants, and BEM classes.
3. Identify data props separately from behavior props.
4. Keep class names component-owned and BEM-based.
5. Put styling variants on `data-*` attributes when they come from select/boolean props.
6. Use Etch Conditions for optional DOM, especially icons, media, wrappers, and alternate links.
7. Use slots for open-ended rich content; use props for controlled fields.
8. Use Group props for structured objects; for repeaters, create a Group prop and use Make Repeatable so the editor exposes repeatable subfields.
9. For configurable semantic tags, use Etch Dynamic Element with a select prop such as `headingTag`; in saved block JSON set both top-level `tag` and `attributes.tag` to the dynamic expression, e.g. `{props.headingTag}`.
10. Verify by rendering representative prop combinations: default, boolean on/off, each select value, dynamic heading tags, empty slot, filled slot, inline style props, and mobile layout. Check that style attributes render literal CSS custom properties such as `--intro-max-width`, not escaped `u002du002d...` text.
11. After replacing a test component, migrate into the live component ID, remove test instances, delete obsolete style keys, and check the front end for old classes.

Current Etch 1.4.x component notes:

- Prop descriptions are available; add one when the expected value, token, or dynamic-data shape is not obvious.
- Boolean and loop props can be target-mapped in the builder, which wraps the target in a condition/loop. Treat this as a productivity tool, then inspect the saved structure.
- Condition props work in the Builder and Gutenberg; current changelog fixes resolved recent Gutenberg breakage, but still verify condition-heavy components in both contexts.
- Repeater props are now editable in Gutenberg, can be reordered by drag-and-drop, and are safer than early 1.4 builds. Keep using Group + Make Repeatable for real repeaters.
- The `{component}` namespace is available inside component HTML for `component.id`, `component.name`, `component.key`, and `component.defaults.<propKey>`. It is not available from parent/page context.

Protected existing work:

- Treat existing working components, sections, classes, properties, instances, content, and style keys as protected.
- If an optimization would require changing a working component or established style, explain the proposal and get approval before editing.
- When the user asks to use an existing component, insert/configure an instance only. Do not edit the component definition unless explicitly approved.
- If componentization seems useful during page work, propose the component API first: semantic root, props, slots, variants, BEM classes, and data ownership.

Button/CTA component pattern:

```html
<span class="cta-button-wrapper">
  {#if !props.newTab}
    <a class="cta-button" href="{props.href}" data-variant="{props.variant}" data-size="{props.size}" data-icon-position="{props.iconPosition}">
      {#if props.showIcon}
        <!-- Etch SVG element with class cta-button__icon -->
      {/if}
      <span class="cta-button__label">{props.label}</span>
    </a>
  {/if}
  {#if props.newTab}
    <a class="cta-button" href="{props.href}" target="_blank" rel="noopener noreferrer" data-variant="{props.variant}" data-size="{props.size}" data-icon-position="{props.iconPosition}">
      ...
    </a>
  {/if}
</span>
```

Use CSS like `.cta-button[data-icon-position="right"] { flex-direction: row-reverse; }` rather than one-off global helper classes.
## Component Pattern

Build component internals with props:

```html
<article class="event-card">
  <etch:img source={props.image} />
  <div class="event-card__content">
    <etch:element tag={props.headingTag}>
      {props.title}
    </etch:element>
    <p>{props.excerpt}</p>
    <a class="btn--primary" href={props.url}>{props.ctaText}</a>
  </div>
</article>
```

Use in a loop by filling props:

```text
image: {item.image.id}
headingTag: h2
title: {item.title}
excerpt: {item.excerpt}
url: {item.permalink.relative}
ctaText: Mehr erfahren
```

Useful dynamic-data modifiers:

```text
{item.slug.replaceAll("-", " ").toUpperCase()}
{this.title.truncateWords(8)}
{item.categories.pluck("name").join(", ")}
{this.etch.header.applyData()}
```

Use `.replace()` / `.replaceAll()` for simple string cleanup, `.truncateWords()` / `.truncateChars()` for display-only excerpt trimming, `.pluck()` + `.join()` for arrays of objects, and `.applyData()` only when a stored value intentionally contains dynamic-data placeholders.

CSS:

```css
.event-card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--white);
}

.event-card__content {
  display: grid;
  gap: var(--content-gap);
}
```

Image/media component pattern:

- For normal WordPress media-library images, use a Media Prop rather than a text URL prop.
- In saved Etch component properties, a media prop is `type: { primitive: "string", specialized: "wpMediaId" }`.
- Render media-library images with Etch Dynamic Image / `<etch:img />`, passing the media ID: `mediaId="{props.image}"`.
- Enable WordPress responsive output where appropriate with `useSrcSet` and set a suitable `maximumSize` such as `thumbnail`, `medium`, `medium_large`, or `large`.
- Keep `imageAlt` explicit when the per-instance alt text differs from the media-library alt. Otherwise verify the media-library fallback is acceptable.
- Use URL/text props only for direct external assets or SVG/icon sources where the SVG/icon element expects a URL.
- Etch media props can reference media-library files or URLs, but the media prop itself does not currently provide `srcset`; use Dynamic Image + media ID when responsive image output matters.
- Etch 1.4.15+ keeps media ID props as IDs instead of silently rewriting them to URLs, so prefer passing IDs from loops such as `{item.image.id}`.
- Dynamic Image placeholders now preserve block classes/attributes in the builder, but still check the front-end image output and alt text.
- Dynamic SVG can accept a WordPress attachment ID in `src`, useful for SVG icon props. For raster/photos/content images, stay with Dynamic Image.

Saved block example:

```html
<!-- wp:etch/dynamic-image {"metadata":{"name":"Image"},"tag":"img","attributes":{"class":"event-card__image","mediaId":"{props.image}","maximumSize":"medium","useSrcSet":"true","alt":"{props.imageAlt}","loading":"lazy","decoding":"async"},"styles":["event-card__image"]} -->

<!-- /wp:etch/dynamic-image -->
```

## Archive Template Pattern

Use Main Query for routed archives:

```html
<section class="archive section--l">
  <div class="archive__inner">
    <header class="archive__header content-gap">
      <h1>{this.title}</h1>
      <p>{this.description}</p>
    </header>

    <ul class="archive__grid grid--3 grid--s-1 grid-gap">
      {#loop main-query as item}
        <li>
          <!-- Component instance with props from item -->
        </li>
      {/loop}
    </ul>
  </div>
</section>
```

Do not build a fresh WP Query for a standard archive unless the design intentionally overrides WordPress routing.

## Page Section Pattern

```html
<section class="benefits section--xl bg--base-ultra-light">
  <div class="benefits__inner">
    <div class="benefits__intro content-gap">
      <p class="eyebrow">Vorteile</p>
      <h2>Klare Struktur fuer bessere Entscheidungen</h2>
      <p>...</p>
    </div>

    <div class="benefits__grid grid--3 grid--s-1 grid-gap">
      ...
    </div>
  </div>
</section>
```

```css
.benefits__inner {
  display: grid;
  gap: var(--container-gap);
}

.benefits__intro {
  max-width: 65ch;
}
```

## Decision Rules

Use semantic HTML first, then BEM/custom classes, then ACSS variables. Use ACSS variables in BEM/custom classes by default.

Etch style storage rules:

- Global Etch styles/classes are stored in the WordPress option `etch_styles`. Do not assume page meta `etch_styles` is the active source; check `get_option('etch_styles')` first.
- When writing CSS into `etch_styles`, store real newline characters. Never store literal `\n` text inside CSS values.
- In PHP, prefer double-quoted strings, nowdocs/heredocs, or arrays joined with `"\n"` for multiline CSS. Avoid single-quoted strings containing `\n`.
- After updating styles, check rendered HTML/CSS for literal `\n`, old obsolete class selectors, and the expected new selector.


Semantic element rules:

- Use `section` for standalone page sections and `header` for section intro/header groups when appropriate.
- Use `ul/li` or `ol/li` for repeated labels, features, cards, audiences, benefits, and steps.
- Use `table` only for true tabular data where row/column headers and cell relationships matter.
- Use links for navigation or URL targets; use buttons for actions.
- Use `figure/figcaption` for media with captions and `blockquote` for real quotes.
- Use layout CSS on semantic elements instead of replacing semantic lists with generic `div` grids.
Use an ACSS utility only when:

- The class names remain short and understandable.
- The value is a global system value.
- The decision is unlikely to change per component.
- The utility is present in the active generated ACSS stylesheet.

Use a custom class when:

- The pattern repeats.
- The element needs more than a few utilities.
- The style has component meaning.
- The CSS needs calculations, selectors, states, or media queries.
- The same result can be achieved cleanly with ACSS variables.

Use an Etch component when:

- The user explicitly approves componentization or asks for a component.
- A UI pattern appears more than once.
- A card/list item/template fragment needs consistent structure.
- Editors need safe fields through props.

Use WordPress data when:

- The same type of content appears in multiple places.
- The client will add, edit, reorder, categorize, or filter it.
- Templates or archives should update automatically.

## Common Mistakes to Avoid

- Building repeated cards as static duplicated markup.
- Putting `{item.title}` inside a component instead of passing it as a prop.
- Reading `{component}` from outside a component; it only exists inside the component's own HTML.
- Editing or refactoring existing working components without first proposing the change and getting approval.
- Creating a new component without first proposing its props, slots, variants, and semantic root unless the user already explicitly requested that component.
- Using a normal WP Query for archive templates that should inherit Main Query.
- Styling every element with many utility classes instead of component classes powered by ACSS variables.
- Modeling normal WordPress images as plain URL text props instead of `wpMediaId` media props plus Etch Dynamic Image.
- Assuming a Media Prop alone gives responsive `srcset`; use Dynamic Image with a media ID for responsive WordPress image output.
- Saving CSS into `etch_styles` with literal `\n` sequences instead of real line breaks.
- Assuming an ACSS utility exists without checking the generated `automatic.css`; variables are safer than utility classes across ACSS settings.
- Using raw `px` spacing when `var(--space-*)`, `var(--section-space-*)`, or contextual gaps fit.
- Creating custom font-size `clamp()` values when `var(--h*)` or `var(--text-*)` fits.
- Hardcoding colors instead of ACSS variables/classes.
- Switching away from the Etch Theme mid-project.
- Creating decorative wrappers that Etch/HTML do not need.

## Review Checklist

- Content source is appropriate and WordPress-native.
- Etch structure is semantic and editor-friendly.
- BEM classes are present on sections/components and meaningful child elements.
- Lists, cards, steps, and audience labels use `ul/ol/li`; tables are reserved for actual data tables.
- Components expose the right props.
- Dynamic data scope is correct.
- ACSS handles global rhythm, colors, spacing, and buttons.
- Custom CSS uses ACSS variables.
- Etch global styles were read/written from `get_option('etch_styles')` when editing reusable/global classes.
- Multiline CSS in Etch styles uses real line breaks, not literal `\n`.
- WordPress/media-library images in components use media props and Dynamic Image unless there is a documented exception.
- Utility classes are verified before use.
- Mobile layout is intentional.
- Debug dynamic-data dumps are removed.








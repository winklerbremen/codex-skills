# Bricks Dynamic Data, Queries, Conditions, And Interactions

Use this reference when editing dynamic Bricks pages/templates, query loops, conditional elements, ACF/meta integrations, or interactive behavior.

## Dynamic Data Basics

Bricks dynamic data uses tags wrapped in braces, for example:

```text
{post_title}
{post_url}
{featured_image}
{post_terms_category}
{acf_field_name}
{cf_native_custom_field}
```

Dynamic data can be inserted in text contexts with `{`, or through dynamic data controls for images, videos, links, backgrounds, and other non-text settings.

Use dynamic data heavily in templates, CPT layouts, archive cards, related posts, event listings, team pages, and any content that should remain editable in WordPress.

## Common Standard Tags

Post:

- `{post_title}`
- `{post_id}`
- `{post_url}`
- `{post_slug}`
- `{post_type}`
- `{post_date}`
- `{post_modified}`
- `{post_excerpt}`
- `{post_content}`
- `{read_more}`
- `{featured_image}`

Taxonomies:

- `{post_terms_category}`
- `{post_terms_post_tag}`
- `{post_terms_my_taxonomy_slug}`
- `{term_id}`
- `{term_name}`
- `{term_slug}`
- `{term_url}`
- `{term_description}`
- `{term_meta:my_key}`

Author/user:

- `{author_name}`
- `{author_archive_url}`
- `{author_avatar}`
- `{author_meta:meta_key}`
- `{wp_user_display_name}`
- `{wp_user_email}`
- `{wp_user_role:value}`
- `{wp_user_meta:my_key}`

Site/archive:

- `{site_title}`
- `{site_url}`
- `{archive_title}`
- `{archive_description}`
- `{url_parameter:my_key}`

Native custom fields:

- Prefix native WordPress custom fields with `cf_`, e.g. field `phone_number` becomes `{cf_phone_number}`.

## Dynamic Data Filters And Arguments

Useful filters:

- `:link`: render as link.
- `:newTab`: open link in new tab.
- `:plain`: strip HTML.
- `:value`: return raw value instead of label, essential for ACF true/false or choices in conditions.
- `:url`: return URL from an ID/file field.
- `:image`: render image tag.
- `:format`: preserve HTML format in supported contexts.
- `:timestamp`: convert date/time to timestamp.
- `:slug` / `:term_id`: term-related values.
- `:array_value|key`: read a key from array-returning dynamic data.
- numeric filters such as `{post_excerpt:20}` limit words.

Key-value arguments use `@key:value`, for example:

```text
{acf_text_field @fallback:'No content found'}
{acf_image @fallback-image:554}
{format_date @date:'{cf_event_date}' @from:'Y-m-d' @to:'d.m.Y'}
```

Use `@sanitize:false` only when necessary and only for trusted content. It allows unsanitized output.

## ACF And Meta Integrations

Bricks supports common custom field systems, including ACF, Meta Box, JetEngine, Pods, CMB2, and Toolset.

ACF:

- ACF fields appear in dynamic data controls.
- ACF Relationship, Repeater, Flexible Content, Nested Groups, Image, Gallery, Post Object, True/False, User, and Taxonomy fields have special dynamic/query uses.
- For ACF True/False in conditions, use `:value`; compare false with `== 0` and true with `== 1`.
- `{acf_get_row_layout}` can identify a Flexible Content layout.

Meta Box:

- Meta Box fields appear in dynamic data.
- Cloneable group fields and relationships can feed query loops.

JetEngine:

- JetEngine post types, taxonomies, meta boxes, options, relations, repeaters, and CCT-related data can be available depending on plugin support.

## Query Loop

Bricks Query Loop can be enabled on layout elements and also on supported nestable elements such as accordion, tabs, and slider.

Object types:

- Posts: WP_Query for posts, pages, media, CPTs, WooCommerce products.
- Terms: WP_Term_Query for categories/taxonomies/product categories.
- Users: WP_User_Query for authors/team/community lists.
- Array: array/JSON/PHP arrays, including API response arrays in newer Bricks versions.

Posts query controls include:

- Post type
- Order by / order, including multi-order in newer versions
- Posts per page
- Offset
- Ignore sticky posts
- Disable query merge
- Child of
- Include/exclude, including dynamic tags in newer versions
- Exclude current post
- Taxonomy query
- Meta query
- Random seed TTL
- Is main query for archive/search templates

Query loop rules:

- The loop element is the repeated item. Child elements render inside each result.
- Give query elements descriptive labels; pagination must be linked to the related query.
- For archive/search templates, set exactly one correct main query and configure pagination against it.
- For non-main loops in headers, footers, and related sections, use Disable Query Merge when needed.
- Add ID as a secondary order criterion when paginating by non-unique values to avoid duplicate results across pages.

Important frontend detail:

- Bricks outputs `<!--brx-loop-xxxxx-->` comments for AJAX pagination, filters, Load More, and Infinite Scroll. Do not remove these comments with optimization tools.

## Media Query Loop

For media galleries, query attachment/media posts.

Inside a media loop, use:

- `{post_id}` as image source/ID.
- `{post_title}` for image title.

Avoid `{featured_image}` in attachment loops because the queried items are already media posts.

## Array Query Loop

Array query loops support nested arrays and API-like data.

Use:

```text
{query_array}
{query_array @key:'name'}
{query_array @key:'nested'}
```

For nested arrays, add a nested Array Loop and set the nested path to the parent value, e.g. `{query_array @key:'cars'}`.

## Element Conditions

Element Conditions control whether an element renders on the frontend. They are server-side. If conditions fail, the element HTML is not in the source.

Use conditions for:

- Logged-in / role-based content.
- Date/time-sensitive content.
- Post/user/meta/dynamic data checks.
- Optional ACF fields.
- Membership-like visibility.

Rules:

- Prefer element conditions over CSS hiding when markup should not exist.
- Use `:value` for ACF true/false and choice fields.
- Use timestamp conversion when comparing dates.
- For complex programmatic conditions, use the `bricks/element/render` filter rather than fragile builder-only logic.

## Interactions

Interactions bind events to actions such as:

- Show/hide element.
- Set/remove/toggle attributes.
- Toggle offcanvas.
- Load more query results.
- Start animation.
- Scroll to.
- Run JavaScript function.
- Browser storage actions.

Interactions do not run inside the builder. Verify on the frontend.

Interactions can be defined on global classes for reusable behavior. Treat class-based interactions as shared behavior and inspect usage before editing.

## Echo And PHP Dynamic Data

Bricks supports `{echo:function_name}` dynamic data for whitelisted PHP functions.

Safety:

- Since Bricks 1.9.7, echo functions must be explicitly allowed with `bricks/code/echo_function_names`.
- Echo function arguments should be single-quoted.
- Functions should return values, not echo output.
- Avoid using echo tags when a native dynamic tag or query loop can solve the problem.

## Source Snapshot

Official Bricks Academy pages reviewed on 2026-04-30:

- Dynamic Data
- Query Loop
- Element Conditions
- Interactions
- `bricks_render_dynamic_data`

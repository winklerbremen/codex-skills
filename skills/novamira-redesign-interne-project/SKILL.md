---
name: novamira-redesign-interne-project
description: Project-specific guidance for work on redesign.internet-akquise-webinar.com through the novamira-redesign-interne MCP server, especially EtchWP + Automatic.css landing page implementation, semantic accessibility, and Windows/Node MCP connection notes.
---

# Novamira Redesign Interne Project

Use this skill whenever working on `redesign.internet-akquise-webinar.com`, the MCP server `novamira-redesign-interne`, or the Internet Akquise Webinar redesign landing page project.

## Current Handoff

- If continuing after a context reset or in a new chat, first read `C:\Users\Anwender\Documents\New project\CHAT_HANDOFF_NOVAMIRA_REDENESIGN_INTERNE.md`.
- That handoff file contains the current page state, component inventory, Etch/ACSS lessons, open todos, and the next planned section.

## Confirmed Stack

- WordPress 6.9.4, PHP 8.2.31, locale `de_DE`.
- Active builder/system: Etch v1.4.18 + Automatic.css v4.0.0-rc-1.
- Active Novamira control layer: Novamira v1.2.0 + Novamira Pro v1.0.0-rc1.
- WPCodeBox 2 is installed but inactive.
- No active Bricks Builder or Core Framework observed during initial discovery. Do not use Bricks/Core Framework workflows unless later inspection proves they own the target content.

## Required Companion Skills

- Use `novamira-wordpress-start` before live WordPress edits through MCP.
- Use `etchwp-automaticcss` for page, section, component, CSS, layout, and ACSS-token decisions.
- Use `semantic-accessibility` for headings, landmarks, links/buttons, images, forms, keyboard/focus, WCAG-oriented checks, and responsive accessibility.

## Implementation Preferences

- Prefer semantic HTML first: real `main`, `section`, `header`, `nav`, `footer`, headings, lists, buttons, links, and figures where meaningful.
- Use Etch-editable structures and WordPress-native storage/content patterns wherever practical.
- For Etch Builder-editable section styling, keep section/component CSS in attached selector styles (`etch_styles`) referenced by the blocks. Do not place Hero-owned responsive rules, pseudo states, or nested descendant rules only in a global stylesheet.
- When using the existing CTA component, do not override its color/size/variant variables from the Hero. Choose the component props (`variant`, `size`, etc.) and let the component plus Automatic.css framework tokens define the visual output.
- Use container queries for Hero internals where possible; keep viewport media queries only for decisions that genuinely belong to the page/viewport or the query container itself.
- Across the project, default to container queries whenever they are semantically the better fit for component/section internals, cards, grids, media/text compositions, or reusable layouts. Use viewport media queries only for page/global decisions or cases where a container query cannot reasonably apply.
- Think Automatic.css variable-first: use component/section-owned BEM classes powered by ACSS variables and tokens before reaching for utility classes.
- Use ACSS utilities only when they are generated in the active site, semantically clear, genuinely reduce complexity, and intentionally chosen. Do not treat Automatic.css as utility-first.
- Prefer ACSS variables such as `var(--section-space-*)`, `var(--space-*)`, `var(--content-gap)`, `var(--grid-gap)`, `var(--gutter)`, typography tokens, color tokens, and button tokens.
- Before adding local CSS, check whether Etch element defaults, ACSS defaults, inherited styles, existing global classes, or component props already provide the behavior. Avoid restating widths, margins, gaps, colors, font weights, and padding unless the section/component intentionally diverges from those defaults.
- Avoid `!important`; use it only for narrowly scoped third-party/plugin overrides when selector ownership, source order, variables, or a component-level API cannot solve the conflict. JetForm field/button overrides are an acceptable exception when the plugin's generated styles must be forced.
- For normal body copy, use tokenized text colors instead of custom `color-mix()` blends: on dark backgrounds use `var(--text-light-muted)` and `strong { color: var(--text-light); font-weight: 700; }`; on light backgrounds use `var(--text-dark-muted)` and `strong { color: var(--text-dark); font-weight: 700; }`.
- Verify ACSS utilities before relying on them; fall back to BEM CSS with variables if generated utilities are unknown.
- Keep landing page markup editable in WordPress/Etch and avoid code-only islands unless explicitly approved.
- Do not modify existing components, global styles, or reusable structures opportunistically without explaining the impact and getting approval.

## Accessibility Baseline

- Target WCAG 2.2 AA in practical implementation.
- Maintain a logical heading outline and sane landmarks.
- Use links for navigation and buttons for actions.
- Preserve visible focus and keyboard order.
- Give images meaningful alt behavior; decorative media should stay silent.
- Ensure mobile layouts keep touch targets usable and text/content readable.

## Windows MCP Note

This Windows 11 client needed `NODE_OPTIONS = "--use-system-ca"` for `@automattic/mcp-wordpress-remote`; Node failed TLS verification with `UNABLE_TO_VERIFY_LEAF_SIGNATURE` until it used the Windows system certificate store. Keep credentials in MCP env vars only.

## Current Homepage Notes

- Homepage/page ID `12` contains the webinar hero section.
- Hero image attachment ID `28` is `margit-hero_aktuell-bearbeitet.png` with alt text `Margit Dürich am Mikrofon im roten Outfit`.
- Reuse the existing Etch component `CTA-Button` (`wp_block` component ID `25`, key `CtaButton`) for CTA buttons instead of rebuilding local anchor markup.
- The homepage hero currently uses `CTA-Button` with `variant="secondary"` and `size="l"` directly in `.webinar-hero__actions`; do not add a local wrapper just to override CTA styling.
- The hero media background should stay a smooth dark-blue gradient without the former translucent diagonal stripe pattern.
- Sticky header component ID `58` (`LandingHeader`) uses an Etch slot named `cta` for the existing CTA component. The placeholder is stored in the same paired form used by the working `Intro-1` component. Its root follows the imported `Header Alpha` pattern: `data-etch-element="section"`, `etch-section-style`, and a `position` prop that renders `data-header-position="{props.position.toLowercase()}"`; `.landing-header` activates fixed behavior only with `&[data-header-position="fixed"]`.
- Header repeater lesson: component ID `58` became unclickable in Etch Builder when `navItems` repeater array values were written directly into the page instance via code. The stable approach is to define the `navItems` repeater prop and loop in the component, but let the Builder/user add and save repeater items. Do not patch `navItems` arrays into the instance manually.

## Landing Page Single Template Guardrails

- For the `lp` CPT, a WordPress block template can be registered as a `wp_template` post with `post_name` `single-lp` and the `wp_theme` taxonomy term `etch-theme`. When active, `get_block_templates(['slug__in' => ['single-lp']], 'wp_template')` should resolve it as `etch-theme//single-lp`.
- Do not build landing page rendering through a standalone PHP renderer, shortcode renderer, or custom presentation layer. A first implementation proved that this can technically map ACF data, but it breaks the existing Etch visual system, header/menu behavior, CTA buttons, component styling, and design fidelity.
- Treat homepage/page ID `12` as the canonical visual source for the landing page template. Read its Etch block structure, component instances, `etch_styles`, classes, props, slots, and responsive styling, but do not edit the active homepage when creating the `single-lp` template.
- Build `single-lp` as a proper Etch/block template that preserves the existing homepage structure and component visuals. Replace static copy section-by-section with ACF dynamic data only after the copied Etch structure matches the homepage visually.
- Reuse existing Etch components exactly where they already define the design language: `Landing Header` ID `58`, `CTA-Button` ID `25`, `Intro` ID `21`, `Proof Stats` ID `112`, `Outcome Card` ID `125`, `Icon List` ID `138`, `Method Step Card` ID `170`, and `Testimonial Card` ID `250`.
- Any ACF adapter should normalize data only for Etch props, loops, and dynamic keys. It must not own markup, styling, component presentation, or button/header rendering.
- This is now verified against both the Etch documentation and installed Etch 1.4.19 code. Official Etch docs support: Single templates use `{this.*}` dynamic data; ACF fields resolve as `{this.acf.field_name}`; components must receive template/loop data through props; Flexible Content loops use `this.acf.flexible_field` and `acf_fc_layout`; Dynamic Image should receive media IDs for responsive media output. Installed code support: `TemplatesService` stores templates as published `wp_template` posts tied to the active theme via `wp_theme`; `DynamicData` builds post dynamic data, adds ACF under `acf`, and exposes `etch/dynamic_data/post`; `ACFData` formats images, repeaters, and flexible content recursively.
- When testing a draft `lp` post, prefer both the clean logged-in URL form `?post_type=lp&p={id}` and the real editor preview path. A prior `preview=true` draft preview hit a critical path, so preview handling must be verified separately from the normal logged-in draft render.
- Before reactivating or replacing an existing `single-lp` template, create a fresh backup and inspect any existing template instead of overwriting it.

## Current Component Inventory

- `Intro` component: ID `21`, key `Intro1`. Grouped props: `content`, `display`, `spacing`. Uses `headingHtml`, `headingTag`, `eyebrow`, `handwritten`, `layout`, `align`, `maxWidth`, `variant`, `headerGap`, and `bodyGap`.
- `CTA Button` component: ID `25`, key `CtaButton`. Grouped props: `content`, `link`, `style`, `icon`. Use its props instead of local overrides. Current Hugeicons arrow media ID is `214`.
- `Landing Header` component: ID `58`, key `LandingHeader`. Has CTA slot and `navItems` repeater. Repeater values must be edited through Builder, not injected into page instances.
- `Text Marquee` component: ID `104`. Class naming has been refactored to `text-marquee`.
- `Proof Stats` component: ID `112`. Uses a single Builder-editable repeater prop `items`; each item has `value`, `suffix`, `after`, and `label`. General grouped props are `ariaLabel`, `variant`, `valueSize`, and `padding`. `valueSize` has Small/Medium/Large options and controls both value and label font size together, without changing the internal gap. `padding` has None/Small/Medium/Large options.
- `Outcome Card` component: ID `125`, key `OutcomeCard`.
- `Icon List` component: ID `138`, key `IconList`. Grouped props: `general`, `icon`, `spacingType`, `items`. Variants: `default` and `card`. Current Hugeicons check media ID is `192`.
- `Method Step Card` component: ID `170`, key `MethodStepCard`.

## Gastgeberin/Margit Section

- The Gastgeberin/Margit Dürich section has been added after the Zielgruppe section.
- Use the existing `Intro` component for the eyebrow and heading: `Ihre Gastgeberin`, `Margit <span class="text-mark">Dürich.</span>`.
- Keep the bio copy below the Intro, not inside the Intro slot.
- Use a semantic `figure` for the left image area. Do not include the old blue placeholder stripes or placeholder text. Keep only meaningful overlay badges such as `30+ Jahre Erfahrung` and `Amazon-Bestseller-Autorin`.
- Reuses `Proof Stats` via the `items` repeater with three items; keep the book list local unless reuse need emerges.

## Automatic.css Site Snapshot

- Active Automatic.css version: `4.0.0-rc-1`.
- Generated ACSS files live in `wp-content/uploads/automatic-css/`; `automatic.css`, `automatic-variables.css`, and editor/iframe CSS are generated.
- Generated utility classes observed in this project are intentionally limited. Verified present: `.btn--primary`, `.btn--secondary`, `.btn--outline`, `.btn--xs` through `.btn--xxl`, `.section--*`, `.text--*`, `.bg--ultra-light`, `.bg--light`, `.bg--dark`, `.bg--ultra-dark`, `.card`, `.content-grid`, `.content-grid--off`.
- Verified absent in generated `automatic.css` at this point: `.grid--3`, `.content-gap`, `.container-gap`, `.btn--accent`, `.text--primary`, `.bg--primary-trans-10`, and `--primary-trans-10`.
- Variables are available for colors, spacing, sections, typography, grid, radius, focus, and button systems. Prefer variables such as `--content-gap`, `--container-gap`, `--grid-gap`, `--grid-auto-3`, `--section-space-*`, `--space-*`, `--text-*`, `--h*`, `--radius-*`, `--btn-*`, and palette roles.
- Current key color roles: `--primary` is dark blue, `--secondary` is red, `--accent` is yellow, `--base` is dark blue/base, `--neutral` is black/white scale. Primary and secondary button systems are enabled; many other semantic/extra button systems are not generated.

## Local Fonts

- Fonts are self-hosted via WordPress media attachments and registered in Etch global stylesheet `typography` (`Typography`).
- The Etch stylesheet entry must use `type: "default"` to appear under Style Manager > Stylesheets. `type: "global"` renders on the frontend but is not shown as a normal stylesheet in the Style Manager list.
- Font attachment IDs:
  - `41`: `oswald-variable-latin.woff2`, family `Oswald`, weight `200 700`.
  - `42`: `noto-sans-variable-latin.woff2`, family `Noto Sans`, weight `100 900`.
  - `43`: `kalam-400-latin.woff2`, family `Kalam`, weight `400`.
  - `44`: `kalam-700-latin.woff2`, family `Kalam`, weight `700`.
- Typography tokens:
  - `--heading-font-family: "Oswald", sans-serif;`
  - `--text-font-family: "Noto Sans", system-ui, sans-serif;`
  - `--script-font-family: "Kalam", cursive;`
- Use `var(--script-font-family)` for handwritten/accent text such as the hero script line.


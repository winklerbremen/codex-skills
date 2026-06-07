---
name: coreframework
description: Expert workflow for inspecting, configuring, extending, and debugging WordPress sites that use Core Framework as the design-token, utility-class, component, color, typography, spacing, breakpoint, dark-mode, and generated-CSS system. Use when Codex is asked to work with Core Framework projects, .core imports/exports, Core Framework variables/classes/components, Core Framework plugin settings, generated framework CSS, design-system architecture, Gutenberg/Oxygen/Bricks-neutral Core Framework usage, or WordPress sites where Core Framework is active but the page builder target is not yet confirmed.
---

# Core Framework

Use this skill for the Core Framework layer itself: variables, classes, components, generated CSS, project imports, dark/light mode, breakpoints, and design-system decisions independent of a specific builder.

If the target is confirmed as Bricks-authored content, use `bricksbuilder-coreframework` after this skill. If the target is Etch, Gutenberg, Oxygen, or custom theme code, keep this skill as the design-system guide and use the appropriate builder/theme workflow separately.

## First Moves

1. Confirm the target surface before editing: Core Framework project data, generated CSS, Bricks/Etch/Gutenberg content, theme files, WPCodeBox snippet, or custom plugin/theme code.
2. Inspect the active context: WordPress version, active theme, Core Framework version, active builder/addons, current generated CSS, and whether a `.core` import/export is involved.
3. Treat Core Framework as the source of truth for reusable design tokens. Prefer changing variables, classes, components, or project settings over hardcoded one-off CSS when the change is system-wide.
4. Treat existing generated CSS as output, not the preferred editing source. Edit the Core Framework project/settings when possible, then regenerate/save through the plugin UI or documented storage path.
5. Use raw CSS only for behavior that Core Framework cannot express cleanly, for target-specific fixes, or for temporary verification.
6. Preserve the site's current class strategy. If the project uses BEM + variables, keep doing that. If it uses utilities heavily, verify the generated utilities exist before relying on them.
7. Avoid mixing design systems. Do not assume Automatic.css naming, spacing tokens, recipes, or utility semantics on a Core Framework project.
8. Ask before destructive or broad design-system edits: global color scales, typography scales, breakpoints, root variables, component defaults, `.core` overwrite imports, or generated class renames.

On Bricks + Core Framework sites, Core Framework variables/classes, generated CSS, Bricks Theme Styles, and Bricks element defaults are separate design-system layers. Respect all of them. Do not duplicate or override a value in local CSS when a Bricks control, Theme Style, Core Framework token, or inherited default already produces the intended result.

### Bricks + Core Framework Guardrail

For every Bricks + Core Framework project, the workflow is class-first, controls-first, defaults-first, and variable-first:

1. Let existing Bricks element defaults and Core Framework defaults stand unless a requested design requires a scoped override.
2. Use existing Core Framework tokens in Bricks controls for spacing, color, typography, radius, width, rhythm, and surfaces.
3. Use meaningful classes first for reusable section, component, and pattern styling.
4. Use element ID styling only for true one-off exceptions where a class is not sensible.
5. Do not write Page CSS, header style tags, custom scripts, Theme Styles, global class definitions, Core Framework token changes, or WPCodeBox snippets as a shortcut.
6. If Bricks controls or class controls cannot express a needed behavior, stop and explain the specific unsupported behavior before using custom CSS.

After any Bricks + Core Framework edit, verify Page Settings were not changed unintentionally, classes were not avoided in favor of IDs, and Bricks/Core Framework defaults were not overwritten without a scoped reason.

## Reference Loading

Load only what the task needs:

- `references/coreframework.md` for Core Framework concepts, setup, project storage, variables, colors, typography, spacing, breakpoints, components, utilities, dark mode, and debugging.
- `references/wordpress-workflow.md` for Novamira/WordPress inspection, file/snippet safety, generated CSS discovery, and verification.
- `references/class-strategy.md` for BEM + variables, utility usage, component class architecture, naming, and maintainability.
- `../webdesign-standards/SKILL.md` when CSS architecture, BEM, nesting, specificity, or class ownership is central.
- `../semantic-accessibility/SKILL.md` when semantics, focus, keyboard access, headings, forms, links/buttons, or WCAG-oriented checks matter.

## Default Architecture

Prefer semantic markup with component-owned BEM classes powered by Core Framework variables:

```html
<section class="feature-band">
  <div class="feature-band__inner">
    <header class="feature-band__header">
      <p class="feature-band__eyebrow">Services</p>
      <h2 class="feature-band__heading">Built for repeatable growth</h2>
    </header>
    <ul class="feature-band__grid">
      <li class="feature-card">...</li>
    </ul>
  </div>
</section>
```

```css
.feature-band {
  background: var(--bg-body);
  color: var(--text-body);
}

.feature-band__inner {
  max-inline-size: var(--max-screen-width);
  margin-inline: auto;
  padding-block: var(--space-2xl);
  padding-inline: var(--space-m);
}

.feature-band__grid {
  display: grid;
  gap: var(--space-l);
}
```

Use Core Framework utilities only when they are generated in the active project and make the HTML clearer. Use Core Framework components for shared primitives such as buttons, badges, cards, links, icons, avatars, dividers, and inputs when the project already models them there.

## Core Framework Rules

- Verify variable names from the active site before using them. Common defaults include `--primary`, `--bg-body`, `--text-body`, `--text-title`, `--text-m`, `--space-m`, `--space-2xl`, and `--max-screen-width`, but projects can rename, remove, or extend them.
- Prefer `var(--token)` over raw hex values, pixel spacing, local `clamp()` formulas, or duplicated breakpoints.
- Keep color intent clear: background tokens for surfaces, text tokens for copy/headings, brand tokens for actions/accents, semantic tokens for state.
- Keep typography fluid through Core Framework typography variables where available. Do not invent local type scales unless the project lacks the needed token.
- Keep layout widths aligned with project variables such as `--max-screen-width`. Do not silently introduce a competing container width.
- Preserve dark/light mode semantics. Use theme-aware variables instead of fixed light-mode colors when the site supports theme switching.
- Use `.core` imports carefully. An import with overwrite behavior can replace large parts of the design system; back up or export first when possible.
- Keep generated utility classes and custom BEM classes distinct. Utilities are for small, reusable declarations; BEM classes own component structure and local composition.
- In Bricks projects, use real Bricks elements and controls first, then Core Framework tokens inside those controls. Custom CSS should be the last layer for unsupported selectors/properties, not the default implementation surface.

## Bricks Component Properties

On Bricks + Core Framework sites, component property IDs must be stable, namespaced keys, not generic element-setting names. Use IDs like `exp_title`, `exp_text`, `card_image`, or `fc_txt`; avoid bare IDs such as `text`, `title`, `image`, `tag`, `link`, or `class`, because they are easy to confuse with Bricks element settings and can surface as `undefined` in the builder UI.

Use Bricks-supported property types observed in the active site before creating properties. For plain copy use `text`; for rich/body content use `editor`; for images use `image`; for select controls use an options array of `{ id, label, value }` objects. Do not use `textarea` for Bricks component properties unless the current Bricks version has been verified to support it; on Carisurf staging with Bricks 2.3.3 it caused the `Text` property to become undefined.

When renaming component property IDs, migrate every existing component instance from the old property keys to the new keys in page/template Bricks data, then verify both the component definition and rendered frontend output.

## Quality Bar

Before finishing:

- Verify that the edited surface still uses Core Framework as the design-system source of truth.
- Verify generated CSS/front-end rendering, not only saved settings.
- Verify mobile behavior for grids, text, buttons, cards, images, and section rhythm.
- Confirm no unrelated global tokens, components, classes, imports, snippets, or builder structures changed.
- Summarize the exact target edited, whether the change was Core Framework project-level or local CSS, and any regeneration/cache step required.

## Sources Snapshot

Built from official Core Framework documentation reviewed on 2026-04-30:

- `https://docs.coreframework.com/`
- `https://docs.coreframework.com/builder-integrations/overview`
- `https://docs.coreframework.com/builder-integrations/bricks-builder`
- `https://docs.coreframework.com/official-ui-kits/blocks`
- `https://docs.coreframework.com/official-templates/agency`

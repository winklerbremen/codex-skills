# Novamira WordPress Startcheck

Use this reference only when the task is live WordPress work through Novamira MCP. It is a routing step, not an Etch or Bricks implementation guide.

## Core Rule

Run this startcheck only when:

```text
connection = Novamira MCP
and task = live WordPress edit/inspection
and stack/target surface is not already confirmed in this turn
```

Then choose the next skill/reference path from the actual target content, not from plugin presence alone.

## When To Ask First

Ask a short clarifying question before editing only when the target cannot be discovered safely:

- Whether broad/global changes are allowed when the change touches framework tokens/classes, Bricks Theme Styles/global classes/components/templates, WPCodeBox snippets, or reusable code.
- Which page/template/component should be edited?
- Is this meant to be live, a test component, or a reusable component?
- If multiple builders are present, should the existing builder for this target be preserved?
- If code is needed, should WPCodeBox 2, theme/plugin files, builder settings, or page-local CSS own it?

If the target and requested change are clear, inspect first and proceed.


## Connection Resolution

When the user asks to connect to a named site, resolve the target in this order:

1. Check currently available MCP tool/server names in the active session.
2. Check own Codex site notes under `C:\Users\Anwender\.codex\project-sites\` for aliases, MCP server names, endpoints, and stack summaries.
3. Check the active workspace `.vscode/mcp.json` for MCP server names and credential environment values.
4. Use `C:\Users\Anwender\.codex\config.toml` only when the Codex runtime itself needs a preconfigured server.

Do not duplicate WordPress application passwords into Markdown notes or skills. Credentials live in MCP config environment values. If credentials fail, ask before changing them.

Ignore project `.cursor` and `.claude` folders for routing and workflow by default. Use own Codex skills plus `.vscode/mcp.json` unless the user explicitly asks to inspect or migrate project-specific Cursor/Claude material.

## MCP Remote Setup And Windows Checks

When the user asks to add, reconnect, fix, or verify a Novamira WordPress MCP server, load `windows-mcp-remote.md` before editing config or suggesting changes.

Key rules:

- For `@automattic/mcp-wordpress-remote`, pass credentials only through `env`: `WP_API_URL`, `WP_API_USERNAME`, `WP_API_PASSWORD`.
- Do not pass WordPress credentials through CLI flags such as `--url`, `--username`, or `--password`.
- Use args exactly `["-y", "@automattic/mcp-wordpress-remote@latest"]` unless the user explicitly chose otherwise.
- On Windows, if `npx` works in PowerShell but not in the AI client, suspect PATH inheritance and use the absolute `npx.cmd` path after verifying it.
- After config changes, restart/reload the MCP session or full client and verify by listing the server tools.
- If startup fails, show the `npx` stderr before proposing config changes.

## Read-Only Stack Check

Through Novamira MCP, inspect:

1. Active theme and child theme.
2. Active plugins and versions:
   - Etch / Etch Theme
   - Bricks Builder
   - Automatic.css
   - Core Framework
   - Frames or other design helpers
   - ACF/custom fields when content modeling matters
3. Actual target storage:
   - `wp:etch/*` blocks in `post_content` or reusable `wp_block` components
   - Bricks data/meta on the target page/template
   - native Gutenberg blocks
   - mixed/static migrated markup
4. Design framework evidence:
   - ACSS plugin active and/or generated ACSS variables/classes
   - Core Framework plugin active and/or generated Core Framework variables/classes such as `--bg-body`, `--text-body`, `--text-title`, `--text-m`, `--space-m`, `--space-2xl`, `--max-screen-width`
   - Etch global styles such as `etch_styles`
   - Bricks global classes/theme styles
   - existing BEM/custom class conventions
5. Current semantic/reusable structure:
   - components, cards, buttons, intros, loops, templates
   - heading hierarchy, lists, links, buttons, slots, conditions

## Decision Matrix

### Novamira + Etch + Automatic.css

Use only when:

- Connection is Novamira MCP.
- The target page/template/component is authored in Etch, e.g. `wp:etch/*` blocks or Etch reusable `wp_block` component.
- Automatic.css is active or the target CSS clearly uses ACSS tokens.

Then use:

- `etchwp-automaticcss/SKILL.md`
- `etchwp-automaticcss/references/etchwp.md`
- `etchwp-automaticcss/references/automaticcss.md`
- `etchwp-automaticcss/references/combined-workflow.md`

Do:

- Preserve Etch block/component structure.
- Build with Etch props, slots, conditions, loops, Dynamic Element, Dynamic Image, and semantic HTML.
- Style BEM/component classes with ACSS variables first.
- Verify front-end output and Etch builder stability.

Do not:

- Edit this target as Bricks data.
- Inject Bricks-specific structures.
- Use Etch only because the Etch plugin is active; target content must actually be Etch.

### Novamira + Bricks Builder + Automatic.css

Use only when:

- Connection is Novamira MCP.
- The target page/template is authored in Bricks data/meta.
- Automatic.css is active or the target CSS clearly uses ACSS tokens.

Then use:

- `bricksbuilder-automaticcss/SKILL.md`
- `bricksbuilder-automaticcss/references/bricksbuilder.md`
- `bricksbuilder-automaticcss/references/automaticcss.md`
- `bricksbuilder-automaticcss/references/combined-workflow.md`

### Novamira + Bricks Builder + Core Framework

Use only when:

- Connection is Novamira MCP.
- The target page/template is authored in Bricks data/meta.
- Core Framework is active or the target CSS clearly uses Core Framework variables/classes.

Then use:

- `bricksbuilder-coreframework/SKILL.md`
- `bricksbuilder-coreframework/references/bricks-coreframework.md`
- `bricksbuilder-coreframework/references/coreframework-in-bricks.md`
- `bricksbuilder-coreframework/references/novamira-bricks.md`
- `coreframework/SKILL.md` or targeted `coreframework/references/*` files when the task touches the Core Framework project/design-system layer.

Do:

- Preserve Bricks element structure and builder editability.
- Use Core Framework variables inside Bricks controls/custom CSS.
- Verify generated Core Framework CSS before relying on classes or variables.
- Keep Bricks global classes, Core Framework-generated classes, and local BEM classes in separate ownership lanes.

Do not:

- Apply Automatic.css class names, recipes, spacing behavior, or token assumptions.
- Pick this path from Core Framework plugin presence alone; target content must actually be Bricks when using the Bricks-specific skill.

### Novamira + Gutenberg + Automatic.css

Use when:

- Connection is Novamira MCP.
- The target content is native Gutenberg/block markup.
- Etch and Bricks are not the active target surface.
- Automatic.css is active or the design system uses ACSS tokens.

Then use:

- general WordPress/Gutenberg knowledge
- ACSS reference from the relevant skill or shared reference
- semantic HTML/block-pattern judgment

Do:

- Preserve Gutenberg/block structure.
- Use ACSS variables/classes where appropriate.
- Avoid Etch and Bricks storage formats.

### Novamira + Core Framework + Gutenberg / Custom / Unknown Builder

Use when:

- Connection is Novamira MCP.
- Core Framework is active or the target CSS clearly uses Core Framework variables/classes.
- The target is native Gutenberg/block content, custom theme/plugin output, WPCodeBox/snippet output, or the builder surface is not confirmed.

Then use:

- `coreframework/SKILL.md`
- `coreframework/references/coreframework.md`
- `coreframework/references/wordpress-workflow.md`
- `coreframework/references/class-strategy.md`

Do:

- Preserve the actual editable surface.
- Use Core Framework variables/classes as the design-system layer.
- Ask before broad `.core` imports, global token changes, generated class renames, or component default changes.

### Novamira + Mixed / Unknown

Use when:

- Multiple builders are active and target storage is unclear.
- Plugin presence conflicts with actual target structure.
- The target contains static HTML plus builder fragments.

Then:

- Do not pick Etch or Bricks yet.
- Inspect the exact target page/template/component.
- Ask before converting between builder systems.
- Choose the workflow from the actual editable surface.

### Not Novamira MCP

If the connection is not Novamira MCP:

- Do not run this router as a required step.
- Use the directly relevant skill if the user explicitly asks for Etch, Bricks, ACSS, or local code work.

## Safety Rules

- Never switch theme or builder system as a routine edit.
- Back up page/component content in post meta before structural live edits.
- Keep edits scoped to the requested target.
- Treat existing working site structures as protected. Do not alter finished components, sections, classes, global styles, properties, content, instances, or builder storage formats unless the user explicitly approved that exact change.
- For Bricks targets, root order and `children` arrays are protected structure. Non-structural work such as CSS, responsive tuning, global classes, WPCodeBox snippets, or JS must preserve `parent`, `children`, sibling order, section order, and component instance placement. Snapshot root order and the relevant parent/container `children` arrays before any Bricks write, then compare after saving and browser-check visible heading/section order.
- Share optimization ideas before implementing them. Include what would change, why, and possible impact; wait for approval before touching existing working elements.
- On all Bricks work, preserve Bricks and active design-system defaults first, use existing tokens/variables in Bricks controls, use classes first for reusable section/component/pattern styling, and use element ID styling only for true one-off exceptions. Do not use Page CSS, header style tags, custom scripts, Theme Styles, global class definitions, design-system token changes, or WPCodeBox/snippets as shortcuts without explicit approval.
- On Carisurf specifically, Bricks work is mobile-first as a project rule. Build and style the mobile/base layout first with Bricks controls/classes, then add tablet/desktop enhancements. If mobile is wrong, fix the Bricks breakpoint/control ownership at the base/mobile level instead of patching with Page CSS, header style tags, scripts, or ID-only styling.
- When editing WordPress block `post_content` through PHP, always preserve escaping with `wp_slash($content)` in `wp_update_post`. Unslashed updates can corrupt Etch component repeater attributes by turning escaped JSON into invalid `u0022` text and causing builder saves to drop repeater values.
- Verify rendered front-end HTML after changes.
- Browser verification is required after visual, layout, responsive, CSS, animation, or interaction changes. Prefer Playwright MCP or the active browser automation plugin. Check desktop and mobile at minimum; add tablet/mobile-landscape for responsive changes and test interactive states, not just static screenshots. If browser verification is unavailable, state that clearly.
- Verify builder-selectability risks when changing components, anchors, SVGs, slots, conditions, or dynamic elements.

## Internal Start Summary

Keep a short internal summary before implementing:

```text
Connection: Novamira MCP / other / local only
Target: page/template/component + ID when known
Theme: active theme
Plugins: Etch yes/no, Bricks yes/no, ACSS yes/no, Core Framework yes/no
Actual target surface: Etch / Bricks / Gutenberg / mixed / unknown
Design-system layer: ACSS / Core Framework / other / unknown
Decision: which skill/reference path
Open questions: only if needed
```




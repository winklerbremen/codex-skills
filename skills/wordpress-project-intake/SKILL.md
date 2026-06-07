---
name: wordpress-project-intake
description: Intake and routing workflow for WordPress implementation work before choosing stack-specific skills. Use when Codex needs to clarify or inspect a WordPress project's stack, builder, design framework, WPCodeBox usage, content model, mobile-first preference, approval boundaries, and where code or styles should live before editing. Triggers include new WordPress pages, redesigns, Bricks/Etch/Gutenberg/custom-theme work, Core Framework or Automatic.css projects, WPCodeBox 2 snippet decisions, broad layout/style changes, and any request where the correct skill path is not obvious.
---

# WordPress Project Intake

Use this skill before implementation when the stack, scope, or approval boundary is not already clear. The goal is to route work to the right builder/design-system skill and avoid global changes without consent.

Do not turn every task into an interview. Inspect what can be discovered safely, then ask only the unresolved questions that materially change implementation.

## First Moves

1. If a Novamira MCP connection is available, inspect before asking:
   - Active theme and child theme.
   - Active plugins: Bricks, Etch, Gutenberg/block plugins, Core Framework, Automatic.css, WPCodeBox 2, ACF, performance/cache plugins.
   - Actual target storage: Bricks data, Etch blocks, Gutenberg blocks, custom template/theme output, WPCodeBox snippet, or unknown.
   - Generated CSS/design-system evidence: Core Framework variables/classes, ACSS variables/classes, local BEM CSS, Bricks Theme Styles/global classes.
2. If the target is confirmed, route to the specific skill:
   - Bricks + Core Framework: `bricksbuilder-coreframework`.
   - Bricks + Automatic.css: `bricksbuilder-automaticcss`.
   - Core Framework general/custom/Gutenberg: `coreframework`.
   - Etch + Automatic.css: `etchwp-automaticcss`.
   - Bricks mobile-first/responsive: `bricks-mobile-first`.
   - Semantic/accessibility review: `semantic-accessibility`.
   - CSS architecture or standards: `webdesign-standards`.
3. Ask before any fundamental or broad edit:
   - Global design tokens, utility class generation, `.core` imports, ACSS/Core Framework settings.
   - Bricks Theme Styles, Bricks global classes, Bricks components, templates, template conditions.
   - WPCodeBox 2 snippets, PHP hooks, site-wide JS/CSS, custom plugin/theme files.
   - Any change likely to affect multiple pages or future editing workflow.
4. Work framework-aware and variable-first. If Core Framework or ACSS is active, use the framework's variables/classes instead of recreating fluid typography, spacing scales, color systems, utilities, or button systems.

## Intake Questions

Ask only the questions that remain unknown after inspection. Prefer a short grouped prompt:

```text
Before I edit this, I need 3 decisions:
1. Should this be local to this page, or reusable/global?
2. Is mobile-first required, or should I preserve the existing Bricks breakpoint direction?
3. If code is needed, should it live in WPCodeBox 2, the theme/plugin, or only page-level settings?
```

Use `references/intake-questions.md` for a full question bank.

## Placement Decisions

When the user asks "where should this go?", decide by ownership:

- Design token/color/type/spacing: framework dashboard/project first.
- Builder-wide default: Bricks Theme Styles or builder global settings.
- Reusable builder pattern: Bricks global class or component.
- Page-only section style: page custom CSS or section BEM class.
- Reusable WordPress logic: WPCodeBox 2 snippet or functionality plugin.
- Durable project code: child theme/custom plugin when versioning and deployment matter.
- Content structure: CPT, taxonomy, ACF field group, options page, template, or query loop instead of hardcoded builder content.

Use `references/placement-guide.md` for details.

## Approval Gate

For fundamental changes, stop and propose:

```text
I can do this two ways:
- Local: affects only this page/section. Lower risk, less reusable.
- Global: updates the framework/Theme Style/global class so future sections inherit it. Better system fit, broader impact.

I recommend [choice] because [reason]. Should I apply the [local/global] change?
```

Proceed only after approval for the broad/global path.

## Quality Bar

Before handing off or implementing:

- The actual editable surface is known or explicitly assumed.
- The design-system layer is known or explicitly unknown.
- WPCodeBox 2 usage is checked when code placement matters.
- The correct downstream skill is selected.
- Global/fundamental changes have explicit approval.
- The user gets a concise summary of assumptions, chosen path, and scope.

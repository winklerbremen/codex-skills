---
name: novamira-wordpress-start
description: Startcheck and routing workflow for live WordPress work through a Novamira MCP connection. Use before editing a Novamira-connected WordPress site to detect whether the actual target stack is Etch + Automatic.css, Bricks Builder + Automatic.css, Gutenberg + Automatic.css, mixed, or unknown, then route to the correct builder-specific skill/reference set.
---

# Novamira WordPress Start

Use this skill only for live WordPress work through a Novamira MCP connection or a clearly equivalent Novamira remote-control layer.

Do not use this skill for local-only HTML/CSS work, generic WordPress questions, or non-Novamira sites.

## Start

Read `references/start.md` before making live edits when the stack is not already confirmed in the current turn.

If the task is about adding, fixing, reconnecting, or verifying a Novamira WordPress MCP server, read `references/windows-mcp-remote.md` before proposing config changes. This is especially important on Windows, where `npx`-based MCP servers often fail because credentials were passed as ignored CLI flags, Node/npx cannot be resolved by the client process, or the client was not restarted after config changes.

The startcheck decides whether to continue with:

- `wordpress-project-intake` when the task scope, approval boundary, target stack, WPCodeBox placement, or global-vs-local ownership is unclear before editing.
- `etchwp-automaticcss` when Novamira MCP + actual target stack is Etch + Automatic.css.
- `bricksbuilder-automaticcss` when Novamira MCP + actual target stack is Bricks Builder + Automatic.css.
- `bricksbuilder-coreframework` when Novamira MCP + actual target stack is Bricks Builder + Core Framework.
- `coreframework` when Novamira MCP + Core Framework is the confirmed design-system layer but the editable target is Gutenberg, custom theme code, WPCodeBox, or not yet builder-specific.
- `bricks-mobile-first` alongside the relevant Bricks design-system skill when the task is mobile-first or responsive Bricks work.
- `webdesign-standards` alongside any builder/design-system skill when CSS architecture, BEM, nesting, specificity, or class ownership is central.
- `semantic-accessibility` alongside any builder/design-system skill when semantic HTML, WCAG, focus, keyboard access, forms, links/buttons, headings, landmarks, or touch targets are central.
- a Gutenberg/ACSS fallback when Novamira MCP + target content is native Gutenberg + ACSS.
- inspection/questions when the stack is mixed or unknown.

Never choose a builder workflow from installed plugins alone. Inspect the actual target page/template/component storage.

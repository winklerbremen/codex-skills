# Core Framework WordPress Workflow

## Table of Contents

- Read-only startcheck
- Finding Core Framework output
- Editing decision tree
- Novamira safety
- Verification

## Read-Only Startcheck

For live WordPress work:

1. List active plugins and versions.
2. Confirm Core Framework is active.
3. Confirm builder/theme stack: Bricks theme, Bricks integration addon, Gutenberg, Oxygen, Etch, custom theme, WPCodeBox.
4. Inspect the target post/template/content storage before choosing a builder workflow.
5. Inspect generated CSS or plugin project data before using variables/classes.
6. Identify caches and CSS generation mode.

Do not choose a builder-specific workflow from plugin presence alone. A site can have Bricks active while the target is Gutenberg content, custom PHP, or a shared template.

## Finding Core Framework Output

Useful places to inspect:

- Rendered front-end HTML: stylesheets, inline CSS, body classes, class names.
- WordPress options for Core Framework project/config data.
- Core Framework plugin directories and generated assets.
- WPCodeBox snippets if the site extends Core Framework behavior.
- Theme or child-theme CSS if Core variables are consumed there.
- Builder global classes/theme styles if a builder integration mirrors Core Framework tokens.

Search targets:

- `--primary`
- `--bg-body`
- `--text-body`
- `--text-title`
- `--space-`
- `--max-screen-width`
- Known component classes from the target markup.

## Editing Decision Tree

Use Core Framework project/settings when:

- The change is global or reusable.
- A variable, color, typography scale, spacing scale, breakpoint, utility, or component default is wrong.
- The user wants the design system updated.

Use builder settings/classes when:

- The target is a Bricks/Etch/Gutenberg element and the design-system token is correct.
- The change is page/template-specific.
- The builder has a native control that maps cleanly to the desired CSS.

Use page/custom CSS when:

- The change is local to one page/section.
- The behavior is not practical in Core Framework or the builder UI.
- You need a narrow compatibility fix.

Use PHP/theme/plugin code when:

- The behavior depends on WordPress data, hooks, conditions, enqueueing, dynamic output, or structured content modeling.

## Novamira Safety

With Novamira MCP:

- Prefer read-only abilities first: discover abilities, inspect posts, read files, get Bricks content/settings/styles.
- Back up content/settings before broad replacements.
- Use targeted patch/insert abilities for builder content.
- Use `execute-php` carefully and return structured data.
- Do not write PHP outside allowed/sandboxed paths unless the user explicitly approved the deployment path.
- Do not delete files, overwrite `.core` project data, or replace full Bricks element trees unless that is the requested operation.

## Verification

After edits:

1. Confirm saved data.
2. Confirm generated CSS contains expected tokens/classes.
3. Confirm rendered HTML contains expected classes/attributes.
4. Confirm visual result on desktop and mobile.
5. Confirm builder editability remains intact.
6. Clear/regenerate caches as needed.
7. Document any manual dashboard action still required, such as saving Core Framework settings or importing a `.core` file.

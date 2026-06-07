# Core Framework In Bricks

## Table of Contents

- Integration addon behavior
- Token usage in Bricks controls
- Class sync and ownership
- Official setup values
- Blocks and template patterns
- Import risks
- Troubleshooting

## Integration Addon Behavior

The Core Framework Bricks integration addon is designed to align Core Framework classes, colors, and variables with Bricks' native workflow. Official docs describe these workflow improvements:

- Variable UI from Bricks inputs.
- Variable autosuggestions.
- Core Framework class sync into Bricks autosuggest.
- Preview of class/variable styles on hover.
- Theme mode toggle in the Bricks top bar.

The addon is helpful but not the source of truth. The source of truth is still the Core Framework project and generated CSS.

## Token Usage In Bricks Controls

Use Core Framework variables directly in Bricks controls where accepted:

```text
Background color: var(--bg-body)
Text color:       var(--text-body)
Heading color:    var(--text-title)
Font size:        var(--text-m)
Padding top:      var(--space-2xl)
Padding right:    var(--space-m)
Container width:  var(--max-screen-width)
```

If a control rejects a variable:

1. Check the expected Bricks value shape.
2. Use the value in page/custom CSS instead.
3. Check whether the Core Framework addon needs syncing.
4. Avoid replacing the token with a raw value unless necessary.

## Class Sync and Ownership

There are three likely class sources:

- Core Framework generated classes/utilities.
- Bricks global classes.
- Local custom/BEM classes in page CSS.

Choose one owner for a class. If `.card` exists in Core Framework and Bricks global classes, edits become hard to predict. Prefer scoped BEM names such as `.pricing-card` or `.service-card` to avoid collisions.

## Official Setup Values

Official Core Framework Bricks setup guidance includes:

- Theme Style condition: Entire website for global defaults.
- Site background: `var(--bg-body)`.
- Link typography color: `var(--primary)`.
- Body text color: `var(--text-body)`.
- Body font size: `var(--text-m)`.
- All headings color: `var(--text-title)`.
- Section padding: `var(--space-2xl)` top/bottom and `var(--space-m)` left/right.
- Container width: `var(--max-screen-width)`.
- Default heading tag recommendation: `h2`.
- Image caption: no caption.

Use these as setup/audit guidance, not as mandatory edits for every task.

## Blocks and Template Patterns

Core Framework's official Blocks kit combines:

- Core Framework utility classes.
- Core Framework variables.
- Core Framework native components.
- Custom BEM classes.
- Accessibility-oriented markup.
- Bricks global components.

This supports a hybrid architecture: use Core Framework tokens for consistency, BEM for readable custom sections/components, and Bricks components/global classes for reusable builder-native patterns.

## Import Risks

Core Framework templates or kits can involve `.core` imports and Bricks remote templates. Importing may overwrite project settings if the user chooses overwrite behavior.

Before importing a kit/template:

1. Confirm the user wants that kit/template installed.
2. Export/back up the current Core Framework project if possible.
3. Confirm required addons/licenses.
4. Confirm Bricks remote template settings if needed.
5. Confirm SVG upload/code execution settings only when the imported template requires them and the user accepts the security implications.

Do not enable code execution or broad SVG upload settings as a routine fix without explaining why.

## Troubleshooting

Class appears in Bricks but no front-end style:

- Generated Core Framework CSS may not include it.
- Stylesheet may not be enqueued.
- Cache may be stale.
- Class may be preview-only from addon sync.

Variable appears in CSS but Bricks field looks blank:

- Bricks control may not support arbitrary CSS variables in that field.
- Integration addon may need refresh.
- Use custom CSS or the correct Bricks value shape.

Dark mode wrong:

- Hardcoded color used instead of semantic token.
- Token value not defined for alternate mode.
- Bricks preview theme toggle differs from front-end state.

Container width wrong:

- Bricks Theme Style width differs from Core Framework `--max-screen-width`.
- Page section has local width/max-width override.
- Nested container adds unexpected padding or max width.

Specificity conflict:

- Bricks element ID styles can beat class CSS.
- Inline styles beat generated classes.
- Bricks global classes and Core Framework classes may both target the same selector.

Fix with the least global change that matches ownership.

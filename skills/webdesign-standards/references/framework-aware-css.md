# Framework-Aware CSS

## Rule

If a framework is active, do not rebuild the framework locally. Inspect the framework output and use its variables/classes first.

## Core Framework

When Core Framework is active:

- Use Core Framework variables for typography, spacing, colors, widths, and theme-aware values.
- Do not invent separate fluid typography if Core typography variables exist.
- Do not create competing spacing scales.
- Use Core Framework utilities only after verifying they are generated.
- Use BEM classes powered by Core variables for custom sections/components.

Common documented variables include:

- `--primary`
- `--bg-body`
- `--text-body`
- `--text-title`
- `--text-m`
- `--space-m`
- `--space-2xl`
- `--max-screen-width`

Verify names before use.

## Automatic.css

When ACSS is active:

- Detect the ACSS major version before using version-sensitive classes/tokens.
- Prefer ACSS variables and established recipes/utilities.
- Do not assume Core Framework token names.
- Use BEM/custom classes with ACSS variables for custom components.

## Custom Framework

When the project has its own variables:

- Search generated CSS for `:root`, `--space`, `--color`, `--text`, `--radius`, `--container`.
- Reuse naming patterns.
- Add tokens only when reuse is clear.

## Mixed Frameworks

If multiple systems are active, inspect actual target CSS and markup before choosing one. Do not mix ACSS and Core Framework classes in one component unless the project already has that convention.

## Variable-First Order

1. Existing semantic framework variable.
2. Existing utility/global class.
3. Component/BEM class using framework variables.
4. Local component custom property wrapping framework variables.
5. Raw value only for deliberate exceptions.

## Approval

Ask before changing framework settings, generated utilities, global classes, or tokens. These are broad system edits.

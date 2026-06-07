# Bricks + ACSS Workflow

Use this reference when implementing a real change on a Bricks + Automatic.css site through Novamira.

## Read-Only Checklist

1. Identify target post/template ID and area.
2. Detect active Automatic.css major version.
3. Identify whether the target is page content, template, component, global class, theme style, or a mix.
4. Read current Bricks content.
5. Read page/template settings when CSS, conditions, template conditions, or scripts may be involved.
6. Inspect relevant existing classes, component instances, dynamic data, query loops, and CSS.
7. Confirm ACSS is active and token usage is present.
8. If a reference page is supplied, compare against the reference before changing structure, order, or visual behavior.

## Edit Strategy

Choose the smallest operation:

- Text/settings-only fix: patch existing elements.
- Add a section/card/list: insert a new subtree.
- Replace a broken area: set full content only after capturing current content.
- Styling-only change: use page custom CSS or the relevant existing class system.
- ACSS 3.x project: preserve existing 3.x utility and token conventions.
- ACSS 4.x project: prefer custom classes with variables and recipes over utility-heavy markup.
- Component change: edit the main component only if the requested change should affect all instances; otherwise adjust instance properties or ask.
- Template change: inspect conditions first and avoid changing where templates apply unless explicitly requested.
- Query/dynamic change: preserve loop comments, query IDs, pagination relationships, and dynamic data filters.
- Semantic-only change: preserve visual output. Use Bricks controls to reset browser defaults before using Custom CSS. Ask before class additions or class edits.

## New Section Pattern

Use a semantic Bricks tree:

```text
section.section-name
  container.section-name__inner
    heading.section-name__heading
    text-basic.section-name__intro
    container.section-name__grid
      block.section-name__card
        heading.section-name__card-title
        text-basic.section-name__card-text
```

Style with ACSS variables:

```css
.section-name__inner {
  display: grid;
  gap: var(--container-gap);
}

.section-name__grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: var(--grid-gap);
}

.section-name__card {
  display: grid;
  gap: var(--content-gap);
  padding: var(--space-m);
  border-radius: var(--radius-m);
  background: var(--white);
}

@media (max-width: 767px) {
  .section-name__grid {
    grid-template-columns: 1fr;
  }
}
```

## Dynamic Content

Prefer WordPress-native modeling for repeated content:

- CPTs for repeatable entities.
- Taxonomies for categories/filtering.
- ACF fields for structured content.
- Bricks query loops for lists.

Do not hardcode repeated business data when the site already has content models.

## Verification

After editing:

1. Re-read the Bricks content or settings that were changed.
2. Fetch the rendered front-end HTML for the target page.
3. Check for missing hrefs, empty headings, visible code blocks, duplicated IDs, broken class references, removed loop comments, incorrect query pagination, and obvious layout issues.
4. Check mobile behavior when the edit changes layout.
5. For dynamic content, verify at least one representative loop item and empty/fallback state when practical.
6. Report exactly what changed and what was verified.

## Ask Before

Ask before:

- Editing Bricks global class definitions.
- Adding a new CSS class or changing an existing class.
- Editing component properties or component definitions.
- Editing Bricks Theme Styles.
- Changing template conditions.
- Replacing a full Bricks area.
- Migrating content between builders.
- Creating a reusable component or design-system pattern that affects multiple pages.

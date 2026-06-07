# Semantic HTML Reference

## Landmarks

Use:

- `header` for introductory/site or section header content.
- `nav` for major navigation.
- `main` for the primary page content. Usually one per page.
- `footer` for footer content.
- `aside` for complementary content.

Avoid landmark noise. Not every wrapper needs a landmark.

## Sections And Articles

Use `section` for a distinct thematic group that would make sense in an outline and usually has a heading.

Use `article` for self-contained reusable/distributable content:

- Blog post card.
- Case study card.
- Testimonial.
- News item.

Use `div` for purely structural wrappers without semantic meaning.

## Headings

Headings communicate document structure. Choose tags by hierarchy, not size.

Rules:

- One primary page `h1` in normal pages.
- Do not skip levels casually.
- Card titles can be `h3`/`h4` depending on surrounding section.
- Use CSS/classes for visual size.

## Links And Buttons

Use `a` when the user navigates to a URL or document location.

Use `button` when the user triggers an action:

- Opens menu.
- Toggles accordion.
- Submits custom UI.
- Opens modal.
- Filters content without navigation.

Do not make `div`/`span` clickable.

## Lists

Use lists for repeated related items:

- Feature lists.
- Card grids.
- Steps.
- Navigation.
- Benefits.

Style the list with CSS; do not replace it with divs because it is visually a grid.

## Tables

Use tables only for real row/column data. Do not use tables for layout.

## ARIA

ARIA is for gaps, not decoration.

Prefer native HTML. Add ARIA when:

- A custom control needs state such as `aria-expanded`.
- A relationship needs to be exposed such as `aria-controls` or `aria-describedby`.
- A landmark needs a label.

Do not add redundant ARIA to native elements.

## Hidden Content

Use the right hiding method:

- `hidden` or `display:none` when content should not be available.
- Visually hidden class when content should be available to assistive tech but not visible.
- `aria-hidden="true"` only when content should be ignored by assistive tech.

Never hide focusable interactive content from assistive tech.

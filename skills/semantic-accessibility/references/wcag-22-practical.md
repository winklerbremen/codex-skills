# WCAG 2.2 Practical Reference

## Target

Use WCAG 2.2 AA as the default practical target unless the user specifies otherwise.

WCAG is organized around four principles:

- Perceivable.
- Operable.
- Understandable.
- Robust.

## WCAG 2.2 New Criteria To Remember

WCAG 2.2 adds criteria beyond 2.1. Practical AA/A items especially relevant to site builds:

- 2.4.11 Focus Not Obscured (Minimum) AA: focused components should not be fully hidden by author-created content such as sticky headers.
- 2.5.7 Dragging Movements AA: provide a non-dragging pointer alternative when drag is required.
- 2.5.8 Target Size (Minimum) AA: targets should be at least 24 by 24 CSS px or have sufficient spacing/exceptions.
- 3.2.6 Consistent Help A: help mechanisms should appear consistently.
- 3.3.7 Redundant Entry A: do not force users to re-enter the same information unnecessarily.
- 3.3.8 Accessible Authentication (Minimum) AA: authentication should not depend on cognitive function tests unless alternatives exist.

## Keyboard

Check:

- Every interactive control is reachable by keyboard.
- Focus order follows visual/reading order.
- Focus is visible.
- Sticky headers/overlays do not obscure focus.
- Menus, accordions, tabs, modals, filters, and forms can be operated without a mouse.

## Contrast And Visual State

Check:

- Text contrast.
- Focus indicator contrast.
- Hover/focus/active state not conveyed by color alone.
- Disabled states remain understandable.

Ask before changing visual design significantly.

## Forms

Every form control needs:

- Programmatic label.
- Clear visible label or accessible name.
- Help text when needed.
- Error message connected to the field.
- Required indication that is not color-only.
- Autocomplete attributes where appropriate.

## Images

- Informative image: meaningful alt text.
- Decorative image: empty alt or hidden from assistive tech.
- Functional image/link: alt describes action/destination.
- Background image: only expose as image/label if it conveys content.

## Motion

Avoid motion that is required to understand content. Respect reduced-motion preferences for animations and parallax.

## Manual Testing

Automated checks are not enough. Manually test:

- Keyboard-only tab path.
- Screen-reader-like accessible names.
- Focus visibility.
- Mobile tap targets.
- Headings and landmarks.
- Zoom/reflow where relevant.

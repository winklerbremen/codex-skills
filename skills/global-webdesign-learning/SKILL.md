---
name: global-webdesign-learning
description: Global instruction for Codex to preserve durable web design, WordPress, EtchWP, Automatic.css, accessibility, component, and implementation lessons by updating global skills or creating focused new skills when knowledge should apply beyond a single project.
---

# Global Webdesign Learning

Use this skill as a meta-rule for web design and WordPress implementation work, especially EtchWP, Automatic.css, semantic accessibility, and screenshot-to-section workflows.

## Rule

Do not leave durable implementation lessons only in the conversation. When a new finding is generally useful beyond the current project, save it globally by updating the relevant global skill or creating a focused new skill/reference.

When a mistake is discovered and the corrected approach is understood, treat that as a learning event, not merely as a bug fix. After fixing the immediate issue, decide whether the lesson is project-only or generally reusable. If it is generally reusable, update the relevant global skill automatically without waiting for the user to ask again. If the lesson is project-only, update the project skill, project handoff, or project memory.

Do not require the user to explicitly say "save this" or "update your skill" after a clear failure pattern has been identified. The expected behavior is: fix the issue, verify it, then preserve the durable lesson in the right place.

When the immediate fix tempts you to deviate from the user's requested architecture, component, builder primitive, or reusable block because the first attempt did not work, pause and ask before substituting a different implementation strategy. For example, if the user requested an existing component, do not replace it with locally recreated markup just because the component props are difficult. First inspect working examples of that component and ask before using a fallback that changes the implementation model.

## Where To Save

- Project-only facts, URLs, client preferences, page IDs, media IDs, and local decisions: project skill or project memory.
- General EtchWP behavior, component patterns, props, block-storage rules, builder quirks, and validation steps: `etchwp-automaticcss` or a dedicated Etch skill/reference.
- General Automatic.css behavior, token usage, color configuration, variable-first rules, card/button/layout patterns, utility constraints, and ACSS 4.x findings: `etchwp-automaticcss` or a dedicated Automatic.css skill/reference.
- General accessibility rules: `semantic-accessibility`.
- Reusable screenshot-to-section planning workflow: global skill/reference if it applies across projects.

## Before Saving

Only save knowledge that is durable, verified, and likely to be reused. Do not globalize one-off project content, temporary workarounds, or unverified guesses.

Use this threshold: if the lesson would have prevented a real mistake, user frustration, broken layout, broken builder editing, incorrect component substitution, or repeated trial-and-error in another project, it is probably worth saving globally.

## Verified Global Lessons

- Etch 1.4.x component Group Property instance values can break Builder clickability when saved as real nested JSON objects in `wp:etch/component` block attributes. Even if the frontend renders, the Builder may stop selecting the component or showing Inspector controls. Store group values as Etch-compatible expression strings in the shape `{{JSON}}`, for example `attributes: { content: "{{\"label\":\"Kontakt\",\"ariaLabel\":\"Kontakt aufnehmen\"}}" }`, and verify Builder clickability after each prop-group change.
- If a user asks for an existing builder component or section pattern "eins zu eins", keep using that component/pattern. Do not replace it with equivalent-looking local markup as an unapproved workaround. When a component instance does not render as expected, inspect working saved instances of the same component, understand the exact prop/repeater storage shape, and preserve the component. Ask before changing to a fallback implementation model.
- After fixing any builder/block-serialization mistake, update the relevant global or project skill automatically when the lesson is durable. The user should not have to remind Codex to learn from repeated mistakes.
- Existing builder/framework defaults, component CSS class names, and Etch style keys are part of the editing contract. Do not rename or broadly rewrite them during implementation; extend only through approved properties, existing selectors, narrowly scoped data attributes, or minimal variant rules after checking the default behavior first.
- Bricks `children` arrays and root element order are protected content. In style/CSS/JS/global-class/WPCodeBox tasks, never change `parent`, `children`, sibling order, root order, section order, or component instance placement unless the user explicitly requested that structure change. Snapshot root order and relevant parent `children` arrays before Bricks writes, then compare after saving and browser-check visible heading/section order. This prevents accidental moves such as headings landing below cards or maps moving above/below location content during otherwise non-structural work.

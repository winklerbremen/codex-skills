# ACSS 3.x vs 4.x For Bricks Projects

Use this reference before using version-sensitive Automatic.css classes, recipes, breakpoints, color syntax, transparency, dark mode, width classes, or migration advice on Bricks projects.

## Project Rule

Bricks + ACSS projects may be either ACSS 3.x or ACSS 4.x. Detect the installed Automatic.css version first.

EtchWP + ACSS projects in this workspace are assumed to use ACSS 4.x by convention, so this split mainly belongs in the Bricks workflow.

## Detection

Prefer direct plugin metadata:

- WordPress plugin list: `automaticcss-plugin/automaticcss-plugin`
- Use the active version number, e.g. `3.3.6` means ACSS 3.x.

Also inspect generated CSS before relying on any utility class. ACSS modules can be disabled in both generations.

## Non-Negotiable Compatibility Rule

Do not treat ACSS 4.x as an in-place upgrade for 3.x sites. Official ACSS guidance says 4.x is not backward compatible with 3.x and recommends leaving existing 3.x projects on 3.x while starting new projects with 4.x.

For a 3.x Bricks site:

- Preserve the existing ACSS 3.x conventions.
- Do not rewrite utilities to 4.x syntax unless the user explicitly requested a migration.
- Do not remove transparency tokens, alt-color usage, width t-shirt classes, or breakpoint-based utilities as a routine cleanup.

For a 4.x Bricks site:

- Prefer variable-first, BEM-first custom classes.
- Use recipes and modern CSS features instead of removed utility modules.
- Avoid deprecated 3.x-only tokens and utility assumptions.

## Major Differences To Remember

### Framework Philosophy

ACSS 3.x supports a broader utility-class workflow and Pro Mode/Classless tuning.

ACSS 4.x is more strongly variable-first and BEM-first. Most utility class modules were removed or replaced by recipes.

### Deprecated Features

ACSS 4.x removed previously deprecated features. Existing 3.x work may still contain old-but-working patterns. Treat those as compatibility dependencies unless actively migrating.

### Utilities And Recipes

ACSS 3.x:

- Many utilities may be available if their modules are enabled.
- Recipes exist and documentation historically describes recipe expansion with the older 3.x workflow.
- Existing Bricks markup may rely heavily on utility classes.

ACSS 4.x:

- Most utility modules are gone.
- Critical utility behavior is often accessed through recipes inside custom classes.
- Recipe calls use `?`, e.g. `?btn` or `?flex-grid`.

### Breakpoints And Responsive Syntax

ACSS 3.x:

- Uses configured breakpoint presets such as `XL`, `L`, `M`, `S`, plus optional `XXL` and `XS`.
- Bricks breakpoints should be mapped to ACSS breakpoints.
- Existing utility syntax may use the older pattern such as `.grid--m-2` or `.grid--s-1`.

ACSS 4.x:

- ACSS framework no longer depends on preset breakpoint classes.
- Use media queries and container queries directly when needed.
- Where responsive utility syntax exists in surrounding docs/articles, the direction is value-first then breakpoint, e.g. `.grid--2-m`, not `.grid--m-2`.

### T-Shirt Sizes

ACSS 3.x:

- Uses `XXL` naming in many classes/tokens, e.g. section sizing and optional breakpoint naming.

ACSS 4.x:

- Changes XXL t-shirt naming to `2XL`.
- Do not use `2XL` assumptions on a 3.x project.

### Colors

ACSS 3.x:

- Historically uses HSL-oriented color partials and shipped transparency tokens such as `--primary-trans-10` and `--base-ultra-dark-trans-60`.
- Existing projects may use alt colors and transparency variables.

ACSS 4.x:

- Uses an OKLCH-based color system.
- Removes predefined transparency token libraries.
- Use `color-mix()` or relative color syntax for transparency.
- Dark mode uses `color-scheme` and `light-dark()`; alt colors were removed.

### CSS Layers And Specificity

ACSS 4.x implements CSS Layers for better framework specificity control. Do not assume the same cascade behavior as 3.x when debugging overrides.

### Forms

ACSS 4.x refactored form styling around supported third-party form systems. Do not blindly copy 3.x form class assumptions into a 4.x project, or 4.x form expectations into a 3.x site.

### Width Classes

ACSS 3.x may use t-shirt style width conventions depending on enabled modules.

ACSS 4.x width classes use 10-point literal values, e.g. `.width--60`, and related variables may be module-controlled.

### Builder Integrations

ACSS 4.x still supports Bricks, Gutenberg, and Etch, but builder integrations are simplified. Bricks workflow enhancements are more limited than Etch; right-click context menus are the notable Bricks exception.

## Manual Migration Assessment

Assume manual ACSS 3.x to 4.x migration is medium-to-high effort and should be treated as a redesign-level QA task, not a routine plugin update.

Low effort only when:

- The site already uses mostly BEM/custom classes with stable ACSS variables.
- Utility classes are sparse and easy to audit.
- No complex dark mode, forms, transparency tokens, width utilities, or breakpoint utility chains are central to the design.

Medium effort when:

- Pages mix BEM/custom classes with common ACSS utilities.
- Several Bricks templates and global classes need responsive class cleanup.
- Transparency tokens and old width/breakpoint utilities appear but are localized.

High effort when:

- The site is utility-heavy.
- Many Bricks global classes, templates, query loops, cards, forms, and responsive class chains depend on ACSS 3.x output.
- Dark mode, alt colors, transparency tokens, forms, or custom framework settings are deeply used.
- The project relies on disabled/enabled ACSS module choices that do not map cleanly to 4.x.

Migration steps:

1. Clone/stage the site; never migrate ACSS major versions on production first.
2. Inventory ACSS version, enabled modules, Bricks global classes, templates, page custom CSS, and frontend class usage.
3. Find 3.x-only usage: transparency tokens, alt colors, XXL naming, breakpoint utility syntax, width t-shirt classes, deprecated utilities, and utility modules removed in 4.x.
4. Replace utility-heavy patterns with BEM/custom classes using variables and recipes.
5. Rebuild responsive behavior with explicit media/container queries where 4.x no longer provides matching utilities.
6. QA every template and representative page across desktop/tablet/mobile.
7. Only then switch the live project.

## Sources Reviewed

- Official ACSS 4.x "What's New" documentation, reviewed 2026-04-30.
- Official ACSS 3.x and 4.x Bricks setup documentation, reviewed 2026-04-30.
- Official ACSS 3.x and 4.x spacing/recipes/transparency documentation, reviewed 2026-04-30.
- Official ACSS blog post "Preparing for ACSS 4.0", reviewed 2026-04-30.

# Artifact

Build one self-contained HTML page that compares the current version and every variant. Do not change the repository.

## Choose the output

- If an `Artifact` tool is available, publish the page with it, and follow any design guidance that the tool or its related skills require.
- Otherwise, write a standalone `.html` file to a temporary location and give the user its full path. Do not add it to the repository unless asked.

## Stay close to the real product

An Artifact only imitates the app, so fidelity comes from what you copy into it:

- Reuse the real markup structure, CSS values, design tokens, icons, and font choices
- Rebuild the current version first, and use it to check the match
- Include the surrounding context when it affects the judgment, such as a sidebar or page header

## Match the product's color scheme

The whole page, including its own header, labels, and controls, uses the product's color scheme, not a generic artifact theme. Take the real color values from the product's tokens or styles.

- **Product has light and dark schemes:** define both, and add a visible light/dark switcher in the page header. Start in the product's default scheme. If the product follows the system setting, start from the viewer's system setting. The switcher changes the page chrome and every variant together.
- **Product has one scheme:** use only that scheme. Do not add a dark or light alternative, do not add a switcher, and do not adapt to the viewer's system setting.

This rule takes precedence over generic artifact guidance that asks every page to follow the viewer's system theme. If a tool requires both schemes for a single-scheme product, set both to the product's scheme.

## Protect real data

Use realistic sample content. Do not include secrets, internal URLs, customer data, or personal data, because the page may be shared beyond the user.

## Page content

- Put the current version first, labeled as the reference
- Give each variant its letter, type tag, name, one-line idea, and main trade-off. Make the letter large and the type tag visible on every variant, in both the side-by-side and the focused view
- Show the source tag next to the type tag on variants built with a design plugin
- Let the user jump to a variant by its letter
- Offer a side-by-side view and a focused view of one variant at a time
- Make controls and main interactions work, when the interaction is part of the design
- Keep the page usable at phone width, without horizontal page scroll

## Verify

Check once that every variant renders and that the page has no console errors, if browser tools are available. Fix all failures in one batch, then confirm once. Otherwise, say that the page has not been checked in a browser. Keep browser profiles and check scripts in a new temporary folder, and use a free debugging port, because other sessions can run at the same time.

## Hand over

Give the user the link or file path. If published, say that the page is private until they share it. State that the page is a design approximation, and that real data or app behavior can change how a variant works.

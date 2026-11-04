# Artifact

Build one self-contained HTML page that compares the current version and every variant. Do not change the repository.

## Choose the output

- If an `Artifact` tool is available, publish the page with it, and follow any design guidance that the tool or its related skills require.
- Otherwise, write a standalone `.html` file to a temporary location and give the user its full path. Do not add it to the repository unless asked.

## Stay close to the real product

An Artifact only imitates the app, so fidelity comes from what you copy into it:

- Reuse the real markup structure, CSS values, design tokens, icons, and font choices
- Rebuild the current version first, and use it to check the match
- Support light and dark modes if the product does
- Include the surrounding context when it affects the judgment, such as a sidebar or page header

## Protect real data

Use realistic sample content. Do not include secrets, internal URLs, customer data, or personal data, because the page may be shared beyond the user.

## Page content

- Put the current version first, labeled as the reference
- Give each variant its name, one-line idea, and main trade-off
- Offer a side-by-side view and a focused view of one variant at a time
- Make controls and main interactions work, when the interaction is part of the design
- Keep the page usable at phone width, without horizontal page scroll

## Verify

Check once that every variant renders and that the page has no console errors, if browser tools are available. Fix all failures in one batch, then confirm once. Otherwise, say that the page has not been checked in a browser. Keep browser profiles and check scripts in a new temporary folder, and use a free debugging port, because other sessions can run at the same time.

## Hand over

Give the user the link or file path. If published, say that the page is private until they share it. State that the page is a design approximation, and that real data or app behavior can change how a variant works.

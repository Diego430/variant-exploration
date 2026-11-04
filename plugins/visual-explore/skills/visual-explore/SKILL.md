---
name: visual-explore
description: Creates a visual exploration of an existing feature, screen, or component by reading its current implementation and generating design variants to compare, from subtle refinements to radical rethinks. Delivers the variants as a live in-app switcher or as a shareable Artifact, and recommends the better fit. Use this whenever the user wants to explore, compare, or try design options, alternatives, or directions for UI that already exists, including requests like "show me other ways this could look", "give me some variants of this card", or "how else could we design this page", even if they do not say "exploration". Do not use it to build new UI from scratch or to apply one already-decided design change.
argument-hint: "[component or feature] [similar|moderate|wide] [app|artifact]"
---

# Visual Explore

Turn an existing feature or component into a set of distinct design variants that the user can compare, then apply the one they choose.

Input: $ARGUMENTS

## Workflow

### 1. Understand the current version

Find the target from the user's request. If several components match, ask which one they mean.

Read enough to design from what exists:

- The component source, its parents, and where it renders
- Styling, design tokens, fonts, and shared UI components
- The data it shows and its states, such as loading, empty, and error
- Project instructions such as `CLAUDE.md` or `AGENTS.md`
- How to run the app locally, and whether it can run here

Variants that ignore the real component look like generic templates, so this step sets the quality of everything after it.

### 2. Ask for the difference level

Skip this question if the user already gave a level. Treat a request to "go radical" as Wide. Otherwise ask with `AskUserQuestion` when available, or as a plain question. Describe each level in terms of the actual component, and show its mix.

Each variant belongs to one of three types:

| Type | What may change | What stays fixed |
|---|---|---|
| Similar | Spacing, density, type scale, emphasis, color use, radius, and shadow, within existing tokens | Layout, structure, content, interactions |
| Different | Hierarchy, arrangement, UI patterns, layout, and interaction model, such as a table becoming cards | Purpose and data |
| Radical | Concept, information structure, interaction, and visual language | The user's goal only |

The level sets how many variants of each type to build. The default is 6 variants:

| Level | Similar | Different | Radical |
|---|---|---|---|
| Similar | 4 | 1 | 1 |
| Moderate | 3 | 2 | 1 |
| Wide | 2 | 2 | 2 |

Every exploration includes at least one variant of each type, because even a narrow request benefits from seeing one distant option. If the user asks for another number, scale the mix to that number and keep this minimum. For example, 3 variants are always 1 Similar, 1 Different, and 1 Radical.

The user's explicit request overrides the minimum, such as "no radical variants" or "only similar ones". If the user asks for only 2 variants, ask which 2 types they want. Label each variant with its type, so that the user can see the mix.

### 3. Recommend a delivery method

Skip the question if the user already chose a method. If their choice differs from what you would recommend, say so in one line and follow their choice.

Otherwise, offer both methods and mark exactly one as recommended, placing it first with `(Recommended)` on its label. Give the reason in terms of this change, not in general terms.

Recommend the **in-app switcher** when the design depends on what only the running app provides:

- Real data, authentication, or application state
- The surrounding page layout
- Hooks, business logic, or interaction that is costly to imitate
- Small differences where true rendering decides the choice

Recommend the **Artifact** when comparing concepts matters more than integration:

- Different or Radical variants that would need heavy code changes
- People without a local setup need to see or discuss the variants
- Side-by-side comparison is the main need
- The app cannot run here, or the repository should stay untouched

If the signals tie, recommend the in-app switcher at Similar or Moderate, and the Artifact at Wide. If one method is impossible, for example because the app cannot run, offer the other and explain why.

### 4. Plan the variants

First, write one sentence that records the current identity: real color values, fonts, layout, and surface treatment. Use values from the code, not impressions such as "clean". Similar variants keep this identity. Different variants keep its visual language but may change structure. Radical variants may leave it, and must explain why from the product's purpose.

Then give each variant a short name and a one-line idea, such as "Scan-first: compresses each row so that users compare prices faster". Give each Similar and Different variant its own main axis:

- Hierarchy
- Layout
- Typography
- Color use
- Density
- Interaction

Reuse an axis only when the count requires it, and then make the ideas clearly distinct.

Before building, review the plan:

- Compare the one-line ideas side by side. Replace any variant that repeats another or differs only in decoration.
- Check each Similar variant against the identity sentence. Rework it if it drifts.
- Ask whether each plan would fit any similar product. If it would, tie it to this product's users, data, or subject.
- Challenge common generated-design habits unless the identity or the idea needs them: identical rounded cards with soft shadows, decorative gradients, thick colored side borders, uppercase labels above every heading, and numbered markers on content that has no order.

Apply these rules to every variant:

- Show the same realistic content, so differences come from design rather than data
- Use the project's existing components and tokens as far as the level allows
- Keep the variant usable and accessible, including contrast, focus, and keyboard use
- Cover the states where the design choice matters
- Add no dependencies without asking

### 5. Build the exploration

Read the guide for the chosen method and follow it:

- In-app switcher: [references/in-app-switcher.md](references/in-app-switcher.md)
- Artifact: [references/artifact.md](references/artifact.md)

Keep checking bounded, because open-ended self-review costs time and rarely improves the result. Build everything, inspect once, fix all findings in one batch, and confirm once. Then stop, even if small polish ideas remain, and leave them to the user's review.

If the real app cannot be reached, for example because a login blocks it, run the project's own checks, report the browser check as blocked, and list what the user must check. Do not build substitute setups such as throwaway test harnesses, static renders, or stub APIs. They cost more than they prove, and the user's review in the running app answers the question.

### 6. Present the variants

Tell the user how to open the exploration. Report each check as passed, failed, unavailable, or blocked, and do not present an unavailable check as passed. Then list the current version and each variant with its name, idea, and main trade-off. Give your own pick and the reason. Ask which variant they prefer. Combinations are welcome, such as the layout of B with the density of D.

### 7. Offer to apply the choice

After the user chooses, confirm before changing production code. On confirmation:

1. Implement the chosen design in the real component, following project conventions. Treat an Artifact variant as a design reference, and rebuild it with real components and data.
2. Remove all exploration code by following the cleanup steps in the switcher guide.
3. Run the project's relevant type check, lint, and tests.
4. Report the changed files and the check results.

Do not commit unless the user asks.

If the user declines, offer to remove the exploration code. If they keep it, say which files remain and that they must not ship.

---
name: variant-exploration
description: Creates a visual exploration of an existing feature, screen, or component by reading its current implementation and generating design variants to compare, from subtle refinements to radical rethinks. Delivers the variants as a live in-app switcher or as a shareable Artifact, and recommends the better fit. Use this whenever the user wants to explore, compare, or try design options, alternatives, or directions for UI that already exists, including requests like "show me other ways this could look", "give me some variants of this card", or "how else could we design this page", even if they do not say "exploration". Do not use it to build new UI from scratch or to apply one already-decided design change.
argument-hint: "[component or feature] [similar|moderate|wide] [app|artifact]"
---

# Variant Exploration

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

### 2. Find the problem

Find out what is wrong with the current version, if anything. Variants that answer a real problem are easier to compare and to choose from than variants that only change the look.

Skip this step if the request already names the problem, such as "users miss the upgrade button", or if the user says they only want to see options. Also skip it if the user asks to skip the questions. In that case, use your own guess as the problem and continue.

Otherwise, ask one short question. Do not interview the user. Guess the problem from what you read in step 1, and ask with `AskUserQuestion` when available, or as a plain question:

- Question: one sentence, such as "What should the variants fix?"
- Options: one or two concrete problems you found in the code, such as "Price is below the fold on mobile", and **Just show options**. Keep each description to a few words.
- The user can describe another problem through "Other".

Ask a follow-up only if the answer does not tell you what to fix. Ask at most one, in one sentence. Do not play back a problem statement for confirmation.

Write the result as one line: **Problem:** the chosen problem, or "None, open exploration". Use it in step 6 to aim the variants, and in step 8 to explain your pick. If there is no problem, plan the variants for range, as usual.

### 3. Ask for the difference level

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

The user's explicit request overrides the minimum, such as "no radical variants" or "only similar ones". If the user asks for only 2 variants, ask which 2 types they want. Label each variant with its type, so that the user can see the mix. Step 6 defines the full set of labels.

If step 2 found a problem, describe each level by what it can do about that problem. For example, Similar may fix a missed button through emphasis alone, while Different may move it.

### 4. Offer design plugins

Check whether design plugins are installed. Other design tools can widen the range of the exploration, because each one brings its own design habits.

Find the installed plugins and skills:

- The skill list in your context, where plugin skills have names such as `plugin:skill`
- `~/.claude/plugins/installed_plugins.json`, when you can read it

A plugin is design-related when its description covers visual design, UI, frontend styling, design systems, or a design tool, such as Figma. Do not count this skill, and do not count plugins for other work, such as commits, reviews, or tickets. If no design plugin is installed, skip this step and do not mention it.

Otherwise, ask with `AskUserQuestion` when available, or as a plain question. Name each design plugin you found, with a few words on what it does. Offer at least these options:

- **Replace some variants:** build some of the planned variants with the plugins. The total stays the same.
- **Add variants:** build extra variants with the plugins, beside the variants from step 3.
- **No plugins:** build all variants with this skill only.

If you found more than one design plugin, ask in the same call which plugins to use, as a multiple-choice question. If the user asked to skip the questions, do not use plugins, and name them in one line when you present the variants.

Plan the plugin variants:

- **Count:** build one variant for each chosen plugin, unless the user gives another number.
- **Replace:** take the slots from the type with the most variants, and keep at least one variant of each type. The plugin variant keeps the type of the slot it takes.
- **Add:** give each extra variant the type that fits what the plugin does best. Give it the next free letter.
- **Source tag:** add a tag that names the plugin, such as `via impeccable`, next to the type tag. Show it wherever the variant appears. Variants without a source tag come from this skill.

To build a plugin variant, load the plugin's skill and follow its design guidance for that variant only. Give the plugin the problem statement from step 2, so that it does not interview the user again. The rules of this exploration still apply: the same content, the type limits, no new dependencies, and no changes to production code. Skip any plugin step that conflicts with them, such as writing to production files or creating its own documentation.

### 5. Recommend a delivery method

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

### 6. Plan the variants

First, write one sentence that records the current identity: real color values, fonts, layout, and surface treatment. Use values from the code, not impressions such as "clean". Similar variants keep this identity. Different variants keep its visual language but may change structure. Radical variants may leave it, and must explain why from the product's purpose.

Then label each variant:

- **Letter:** assign letters in build order, starting at A: A, B, C, and so on. The current version has no letter and is always "Current". Keep each letter fixed for the whole exploration, including after rework, so that the user can refer to a variant as "B" at any time. If a variant is replaced, the new variant takes the old letter.
- **Name:** two to four words that state what changes compared with the current version, such as "Denser rows", "Cards instead of table", or "Price leads". The name states the change, not a quality, so avoid names such as "Cleaner" or "Modern". Two variants must not share a name.
- **Idea:** one line that explains the change and why it helps, such as "Compresses each row so that users compare prices faster".
- **Type tag:** a visible tag that shows how far the variant moves from the current version: Similar, Different, or Radical. Give each type its own consistent tag style, so that the user sees the mix at a glance. The current version has no type tag; label it "Reference".

Show the letter, the name, the type tag, and the source tag of plugin variants wherever the variant appears: in the plan, in the switcher or the Artifact, and in the final presentation.

Give each Similar and Different variant its own main axis:

- Hierarchy
- Layout
- Typography
- Color use
- Density
- Interaction

Reuse an axis only when the count requires it, and then make the ideas clearly distinct.

If step 2 found a problem, make each variant attack that problem through its own axis,. State in the idea line how the variant addresses the problem.

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

### 7. Build the exploration

Read the guide for the chosen method and follow it:

- In-app switcher: [references/in-app-switcher.md](references/in-app-switcher.md)
- Artifact: [references/artifact.md](references/artifact.md)

Keep checking bounded, because open-ended self-review costs time and rarely improves the result. Build everything, inspect once, fix all findings in one batch, and confirm once. Then stop, even if small polish ideas remain, and leave them to the user's review.

If the real app cannot be reached, for example because a login blocks it, run the project's own checks, report the browser check as blocked, and list what the user must check. Do not build substitute setups such as throwaway test harnesses, static renders, or stub APIs. They cost more than they prove, and the user's review in the running app answers the question.

### 8. Present the variants

Tell the user how to open the exploration. Report each check as passed, failed, unavailable, or blocked, and do not present an unavailable check as passed. Then list the current version and each variant in letter order, with its letter, type tag, source tag if any, name, idea, and main trade-off. For example: "**B** `Different` Cards instead of table: shows each plan as a card so that users compare features faster. Trade-off: ...". Give your own pick by letter and the reason. If step 2 found a problem, judge the pick by how well it fixes that problem. Ask which variant they prefer, by letter. Combinations are welcome, such as the layout of B with the density of D.

### 9. Offer to apply the choice

After the user chooses, confirm before changing production code. On confirmation:

1. Implement the chosen design in the real component, following project conventions. Treat an Artifact variant as a design reference, and rebuild it with real components and data.
2. Remove all exploration code by following the cleanup steps in the switcher guide.
3. Run the project's relevant type check, lint, and tests.
4. Report the changed files and the check results.

Do not commit unless the user asks.

If the user declines, offer to remove the exploration code. If they keep it, say which files remain and that they must not ship.

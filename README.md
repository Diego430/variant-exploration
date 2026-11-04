# Visual Explore

A Claude Code skill that turns an existing feature or component into a set of design variants you can compare, then applies the one you choose.

## Install

Run these commands in Claude Code:

```text
/plugin marketplace add Diego430/visual-explore
/plugin install visual-explore@visual-explore
```

## Use

Describe what you want to explore:

```text
Show me other ways the pricing card could look
```

Or run the skill directly. All arguments are optional:

```text
/visual-explore:visual-explore PricingCard moderate app
```

## How it works

1. Reads the current component, its styles, and its data.
2. Asks how far the variants should move from the current version.
3. Recommends a delivery method and lets you choose.
4. Builds the variants, with the current version as reference.
5. After you choose a variant, offers to apply it and remove the exploration code.

### Difference levels

The level sets the mix of variant types. The default is 6 variants:

| Level | Similar | Different | Radical |
|---|---|---|---|
| Similar | 4 | 1 | 1 |
| Moderate | 3 | 2 | 1 |
| Wide | 2 | 2 | 2 |

- **Similar variants** change spacing, density, type, and color use.
- **Different variants** change hierarchy, layout, UI patterns, and interaction model.
- **Radical variants** change concept, structure, and visual language.

Every exploration includes at least one variant of each type, unless you ask otherwise, such as "no radical variants". You can also ask for another number of variants. The skill scales the mix to that number.

### Delivery methods

- **In-app switcher:** a development-only panel on the real page swaps variants live. Best when real data, app state, or page context affects the design.
- **Artifact:** a single page shows the variants side by side, without repository changes. Best for concept exploration and for sharing with people who do not run the app. Without the `Artifact` tool, the skill writes a standalone HTML file.

## What it changes

The in-app switcher adds temporary code to your project, in `__visual-explore__` folders and at marked render sites. The skill does not commit this code. Do not ship the exploration code: review the diff before you commit.

## License

[MIT](LICENSE)

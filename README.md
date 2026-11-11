# Variant Exploration

A Claude Code skill that turns an existing feature or component into a set of design variants you can compare, then applies the one you choose.

## Why use it

Design questions are easier to answer when you can see the options. Ask "how else could this look?" and get six real variants of your own UI, built from your actual code, styles, and data.

- **Starts from what you have.** Variants build on your current component, so they fit your product and are ready to apply.
- **From safe tweaks to bold ideas.** Each exploration mixes small refinements with new layouts and radical concepts, so you see the full range before you decide.
- **Compare side by side.** Switch between variants live on the real page, or share a single page with your team.
- **Clean finish.** Choose a variant and the skill applies it, then removes the exploration code.

## Install

Run these commands in Claude Code:

```text
/plugin marketplace add Diego430/variant-exploration
/plugin install variant-exploration@variant-exploration
```

## Use

Describe what you want to explore:

```text
Show me other ways the pricing card could look
```

Or run the skill directly. All arguments are optional:

```text
/variant-exploration:variant-exploration PricingCard moderate app
```

## How it works

1. Reads the current component, its styles, and its data.
2. Asks a few short questions, one at a time, to find what is wrong with the current version, if anything. The variants then aim at that problem. Skip the questions if you only want to see options.
3. Asks how far the variants should move from the current version.
4. If design plugins are installed, such as Impeccable or Frontend Design, asks whether to build some variants with them. Plugin variants can replace some of the planned variants or come in addition to them.
5. Recommends a delivery method and lets you choose.
6. Builds the variants, with the current version as reference.
7. After you choose a variant, offers to apply it and remove the exploration code.

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

### Variant labels

Each variant gets a letter (A, B, C, ...) so that you can refer to it, for example "use B with the spacing of D". The letter stays fixed for the whole exploration. Each variant's name states what changes compared with the current version, such as "Cards instead of table". A type tag (Similar, Different, or Radical) shows how far the variant moves from the current version.

### Delivery methods

- **In-app switcher:** a development-only panel on the real page swaps variants live. Best when real data, app state, or page context affects the design.
- **Artifact:** a single page shows the variants side by side, without repository changes. Best for concept exploration and for sharing with people who do not run the app. The page uses your product's color scheme. If your product has light and dark schemes, the page has a switcher between them. Otherwise it shows only your product's scheme. Without the `Artifact` tool, the skill writes a standalone HTML file.

## What it changes

The in-app switcher adds temporary code to your project, in `__variant-exploration__` folders and at marked render sites. The skill does not commit this code. Do not ship the exploration code: review the diff before you commit.

## License

[MIT](LICENSE)

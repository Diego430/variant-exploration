# In-app switcher

Build a floating, development-only control that swaps the target between its current version and each variant, live on the real page. Use the project's framework and conventions. The React example below shows the shape, not required code.

## Keep the change removable

The exploration must be easy to remove completely, because it is temporary code inside a real codebase.

1. Check `git status` first. Tell the user about uncommitted changes in files you need to touch.
2. Put all exploration code in one folder named `__variant-exploration__`, next to the target component. Follow a different location only if project conventions require it.
3. Add a `README.md` to that folder. List every file outside the folder that you changed, and how to undo each change.
4. Change as little existing code as possible, ideally one render site. Mark each change with a `variant-exploration` comment so that a search finds it.
   - Check who else uses the target and the components it wraps. Place the swap at a render site that only the explored page uses, so that other pages keep the original design. If that is not possible, tell the user which pages also change.
5. Leave the original component's code intact, and use it as the "Current" option.

## Switcher behavior

- Render only in development, using the framework's own check. Production builds must render the original component and include no switcher.
- Show "Current" and every variant as buttons labeled with the variant letter, type tag, and name, such as "B `Different` Cards instead of table", with `aria-pressed` on the active one.
- Show the active variant's letter, type tag, name, and one-line idea.
- Show the source tag next to the type tag on plugin variants, such as "E `Different` `via impeccable` Editorial layout".
- Use the variant letter as the query parameter value, such as `?variant=b`.
- Let the user collapse the panel, so that it does not hide the design being judged.
- Store the active variant in a URL query parameter, so that it survives a reload and can be linked.
- Keep the active variant during in-app navigation when the target appears across pages, such as a sidebar or header. Internal links usually drop the query parameter, so restore it from temporary browser storage or add it to those links inside exploration code only.
- Style the panel independently of app styles, and keep it above page content.

## Framework pitfalls

- **Server rendering:** render "Current" first and read the query parameter after mount. This avoids a hydration mismatch.
- **Server components:** make the switcher a client component. Pass server-rendered variants to it as props or children, so that data fetching stays on the server.
- **State:** give each variant the same props and data source as the original. Shared hooks keep behavior comparable.

## Shape in React

```tsx
// variant-exploration: temporary exploration code, do not ship
const options = [
  { id: "current", tag: "Reference", name: "Current", idea: "Existing design", render: () => <Original {...props} /> },
  { id: "a", tag: "Similar", name: "Denser rows", idea: "Compresses rows for faster comparison", render: () => <VariantA {...props} /> },
  { id: "e", tag: "Different", source: "impeccable", name: "Editorial layout", idea: "Leads with the plan story", render: () => <VariantE {...props} /> },
];
```

## Verify

1. Run the project's type check and lint.
2. Start the app, or use the dev server that already runs. Confirm that the server runs this checkout, because another checkout can hold the same port.
3. Keep browser profiles and check scripts in a new temporary folder, and use a free debugging port, because other sessions can run at the same time.
4. Open the page and select every option. Use browser tools if available, and capture screenshots of each variant.
5. Fix all failures in one batch, then confirm once. If you cannot open the page here, report the browser check as blocked, list what the user needs to check, and stop checking. Do not build a substitute setup.

## Cleanup

1. Delete every `__variant-exploration__` folder.
2. Undo the changes listed in its `README.md`.
3. Search the project for `variant-exploration` and `__variant-exploration__`. The search must find nothing, apart from intended references in this skill or its installation.
4. Run the type check again.

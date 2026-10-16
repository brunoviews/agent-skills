# Refactor: from one large component to the four files

For a component that mixes rendering, state, effects and styles in one file. The goal is the same behavior with every line in its place, not a redesign: change the structure first, and behavior in a separate step if at all.

---

## 1. Inventory before moving anything

Read the whole component and sort every piece into one of these buckets. Write the list down; it becomes the plan.

| Bucket                  | Examples                                                     | Goes to                  |
| ----------------------- | ------------------------------------------------------------ | ------------------------ |
| Markup                  | JSX elements, component composition                          | `index.tsx`              |
| Styling                 | `StyleSheet` entries, `style={{ ... }}`, colors, spacing     | `styles.ts`              |
| State and effects       | `useState`, `useEffect`, data loading, timers, subscriptions | `viewmodel.ts`           |
| Decisions               | conditions inside JSX, ternaries choosing text or colors     | `viewmodel.ts`           |
| Derived values          | formatting, totals, percentages computed in render           | `viewmodel.ts`           |
| Event handlers          | `onPress={() => setX(...)}`, inline handlers with logic      | `viewmodel.ts` (actions) |
| Types                   | props, local unions, data shapes used only here              | `types.ts`               |

Anything that fits two buckets is usually two things glued together: a decision plus its look. Split it: the decision becomes a semantic value in the viewmodel, the look becomes a variant in the styles.

## 2. Create the folder and move the types

Create `src/views/<name>/` (or the project's equivalent), move the component there as `index.tsx`, and move its types to `types.ts`. Update imports and confirm it still compiles before going on.

## 3. Move the styles

Move every style to `styles.ts`, including the inline `style={{ ... }}` objects. Replace raw colors and spacing with theme tokens as you go.

Where a style depended on a condition (`color: isWrong ? red : green`), don't move the condition: turn it into a semantic state (`state: 'wrong' | 'correct'`) that the styled component receives, and map states to tokens in `styles.ts`. The condition itself moves to the viewmodel in step 4.

## 4. Extract the viewmodel

Create `viewmodel.ts` with `useViewModel(...)` and move into it:

1. All state and effects, unchanged.
2. Every decision from the inventory, as a named value (`canPause`, `stockState`, `addLabel`).
3. Every handler, as an action named after what the user does (`onPause`, not `handlePress2`).

Then replace each setter that `index.tsx` used directly with an action that only allows valid changes. A `setIndex` exposed to the view becomes `onNext` / `onPrevious` / `onGoTo(index)` with the bounds checked inside.

## 5. Shrink `index.tsx` to rendering

`index.tsx` now destructures from `useViewModel()` and renders. Search it for anything left over:

- `?` and `&&` that **compute** something rather than read a decided value
- template strings and `.toFixed`, `.length /`, `Math.` — derived values
- `style=` — styling
- `set` — setters

Each hit goes back to the viewmodel or the styles.

## 6. Verify nothing changed

- Run the type checker and the existing tests.
- Walk through the screen's states by hand: loading, empty, error, and the main path.
- If the view had no tests, the viewmodel can now be tested on its own with `renderHook`, without rendering anything. That's often the first test the view ever gets.

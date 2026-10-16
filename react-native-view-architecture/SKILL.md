---
name: react-native-view-architecture
description: "Structure React Native screens and components as a folder of up to four files: index.tsx (JSX only), styles.ts (styling only), viewmodel.ts (state, effects and decisions) and types.ts (the view's own types), with one rule for where each line goes. Use when creating a new screen or component, when a component has grown large and mixes rendering, state and styling, or when the project already organizes its views this way. Not for navigation setup, data services or global state."
---

# React Native View Architecture

A view is a folder, and each file in it has one job. Where a line of code goes is not a judgement call but a question: does it decide **what** the user sees, or **how** it looks?

```
src/views/productDetail/
├── index.tsx      JSX only: renders what the viewmodel returns
├── styles.ts      styling only: no inline styles anywhere else
├── viewmodel.ts   useViewModel(): state, effects and every decision
└── types.ts       types that belong to this view, only if there are any
```

- A complete view, file by file: `references/example-view.md`
- Splitting an existing large component into this shape: `references/refactor.md`

---

## Before you start

- **The project already uses this architecture**: read one existing view first and copy its exact conventions — folder location, hook name, export style, styling library.
- **It doesn't**: don't restructure it on your own. Ask whether to use it for new views only, or to migrate existing ones too.
- **Styling library**: whatever the project uses. With styled-components or `StyleSheet`, `styles.ts` exports the styled components or the style sheet. With NativeWind, it exports the class strings and variant maps, so `index.tsx` never assembles a `className` itself.

---

## The four files

### `index.tsx` — JSX only

- Calls `useViewModel()`, destructures what it needs, renders it.
- It may **branch on values the viewmodel already decided** (`{isTimed && <Clock />}`, `if (loading) return <Skeleton />`). It never **computes** them (`mode === 'full' && !paused`).
- No colors, no style objects, no derived values (`answers.size / total`), no text chosen by a condition (`adding ? 'Adding…' : 'Add'`).

**Why:** A decision inside the JSX can only be found by reading the markup, and only tested by rendering the screen. When the rule changes (say, when a timed session can be paused), you have to hunt down every place the condition was written, and it's easy to miss one. In the viewmodel the rule has a name (`canPause`), lives in one place, and a `renderHook` test checks it without rendering anything.

### `styles.ts` — styling only

- Styled components, a `StyleSheet`, or class strings. Colors and spacing come from theme tokens, never raw values.
- Visual variants arrive as a **semantic state** the viewmodel decided (`state: 'correct' | 'wrong'`), and `styles.ts` maps each state to tokens. It never knows the business rule behind the state.
- No `style={{ ... }}` in `index.tsx`. Values known only at runtime (an animated style, a measured width) are built in the viewmodel or a hook and passed in ready to use.

**Why:** With every style in `styles.ts` there is exactly one place to look when a margin has to change, and `index.tsx` stays readable as pure structure. An inline `style={{ color: '#E11D48' }}` is also the easiest way to bypass the theme: it doesn't switch with dark mode, doesn't follow the token when the token changes, and nobody notices it until the screen looks wrong.

### `viewmodel.ts` — state, effects and decisions

- Exports one hook per view, `useViewModel(...)`, which receives the route params or props it needs.
- Holds the state, the effects (loading data, subscriptions, timers) and **every decision**: what is shown, what is enabled, which state each item is in.
- Returns three kinds of things:
  - **values ready to render**: already derived and formatted (`priceLabel`, `progress`, `stockLabel`);
  - **actions** named after what the user does (`onIncrement`, `onAddToCart`);
  - **per-item queries** when a list needs a decision per row (`getOptionState(index)`).
- Never returns setters (see "Actions, not setters" below).
- Never imports from `styles.ts` and knows nothing about colors.
- Can be tested without rendering anything, with `renderHook`.

### `types.ts` — the view's own types

- The view's props or route params, its semantic states (`type StockState = 'available' | 'low' | 'soldOut'`), and data shapes that only this view uses.
- Types shared across views stay in the project's shared types folder.
- **Don't create it empty.**

**Why:** A `types.ts` in the folder tells the reader this view has a contract worth knowing: semantic states, route params, its own data shapes. An empty one sends them to open a file for nothing, and teaches them to stop trusting what the folder says.

---

## Where does this line go? What vs how

| Code                                                 | Goes in                                  | Because                         |
| ---------------------------------------------------- | ---------------------------------------- | ------------------------------- |
| `quantity < stock`                                   | viewmodel → `canIncrement`               | decides what's enabled          |
| `disabled ? 0.4 : 1` (opacity)                       | styles, from a `disabled` prop           | how "disabled" looks            |
| `stock <= 5 ? 'low' : 'available'`                   | viewmodel → `stockState`                 | a business threshold            |
| `low` → warning color                                | styles                                   | how a state looks               |
| `(price * quantity).toFixed(2)`                      | viewmodel → `priceLabel`                 | a derived value                 |
| `{canIncrement && ...}` / `disabled={!canIncrement}` | index.tsx                                | only reads a decided value      |

When in doubt, ask: _if the rule changed, which file should I have to open?_ A product decision should never require opening `styles.ts`, and a visual tweak should never require opening `viewmodel.ts`.

**Why:** Whether an option shows as right or wrong isn't a visual question. In a quiz app with an exam mode, the exam must not reveal the answer while practice mode must. If `styles.ts` decided between green and red, the styles would need to know the mode and the correct answer: a product rule hidden in a styling file, and an easy way to leak the answer in the one mode where it has to stay hidden. With `getOptionState` the rule lives next to the rest of the quiz logic, and the styles only know that `correct` is green.

---

## Actions, not setters

```ts
// ❌ The view can write any value, including invalid ones
return { quantity, setQuantity };

// ✅ The view can only ask for what makes sense
return {
  quantity,
  canDecrement: quantity > 1,
  canIncrement: quantity < stock,
  onDecrement: () => setQuantity((q) => (q > 1 ? q - 1 : q)),
  onIncrement: () => setQuantity((q) => (q < stock ? q + 1 : q)),
};
```

**Why:** With `setQuantity` exposed, nothing stops the view from writing `onPress={() => setQuantity(0)}` or `setQuantity(stock + 10)`, and every screen using the viewmodel has to remember the limits. With `onIncrement` and `onDecrement` the limits are checked once, inside the viewmodel, and the view simply has no way to express an invalid quantity. The same applies to any index or position: expose `onNext` and `onPrevious`, not `setCurrentIndex`.

---

## Components

Reusable components in `src/components/` use the same folder shape: `index.tsx`, `styles.ts`, and `types.ts` for their props. If a component has logic of its own, it goes in a hook next to it named after the component (`AnimatedSplash/useAnimatedSplash.ts`) — `viewmodel.ts` is for screens. A component with no styles and no props can be a single `index.tsx`.

---

## Workflow: a new view

1. Read one existing view in the project and copy its conventions.
2. Write `types.ts` first if the view has semantic states or route params: it's the contract between the other three files.
3. Write `viewmodel.ts`: state, effects, decisions and actions. Return everything `index.tsx` needs, already decided.
4. Write `styles.ts` with theme tokens, with variants keyed by semantic state.
5. Write `index.tsx`: destructure and render. If you're about to compute something here, move it to the viewmodel.
6. Run the checklist.

For an existing large component, follow `references/refactor.md` instead.

## Checklist

- [ ] `index.tsx` has no style objects, no colors and no computed conditions — it only reads what the viewmodel returns
- [ ] `styles.ts` has no business rules: variants arrive as semantic states
- [ ] The viewmodel returns no setters (nothing named `set…`)
- [ ] The viewmodel doesn't import from `styles.ts` and mentions no colors
- [ ] `types.ts` exists only if it has something in it
- [ ] Every decision lives in exactly one place

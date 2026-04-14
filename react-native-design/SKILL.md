---
name: react-native-design
description: Build, redesign, critique, refactor, or debug React Native UI with strong visual direction and platform-correct implementation. Handles mobile constraints (gradients, shadows, images, flexbox), diagnoses existing design systems before proposing changes, and adapts to the project's styling stack, including StyleSheet, NativeWind/Tailwind, styled-components, and Restyle. Use when creating or improving any React Native screen or component, translating web/Figma ideas to React Native, auditing an existing mobile UI, or fixing iOS/Android-specific styling and interaction issues.
---

# react-native-design

This skill guides the creation of distinctive, production-grade React Native UI that avoids generic "AI slop" aesthetics and respects the hard constraints of the mobile platform. Implement real, working code with exceptional attention to visual craft, accessibility, and platform correctness.

The user provides a screen, component, or UI direction to build, improve, critique, or debug. They may include context about the purpose, audience, target platform, or existing styling approach.

---

## Design Thinking

Before touching any code, establish a clear aesthetic direction:

- **Purpose**: What problem does this screen solve? What emotional tone should it carry?
- **Theme**: Commit to one direction. Dark themes demand depth, contrast, and layered surfaces. Light themes demand restraint, whitespace, and precise typography.
- **Personality**: Pick an intentional aesthetic — editorial/magazine, brutalist/raw, luxury/refined, playful/toy-like, utilitarian/data-dense, cinematic/immersive. Do not default to "clean and minimal" as a non-choice.
- **Differentiation**: What makes this screen memorable? What is the one thing a user will notice?

CRITICAL: Intentionality beats intensity. A well-executed minimal design and a well-executed maximalist design are both excellent. A half-hearted attempt at either is not.

---

## Project Context — Detect Before Designing

Before proposing any changes, determine what kind of project you are working with:

### New project (no existing screens or theme)

- Full creative freedom. Establish aesthetic direction from scratch.
- Define a color system, type scale, and spacing unit before writing any component.
- Apply the Visual Aesthetics Guidelines without constraint.

### Existing project (screens, components, or theme already present)

Run this diagnostic first:

1. **Inspect the theme/token system** — check for a `colors.ts`, `theme.ts`, `tailwind.config`, or equivalent. Understand the existing palette, surface layers, and typography scale.
2. **Inspect the existing screens** — identify the current hierarchy (header, content, empty states, interactive elements).
3. **Evaluate impact ceiling** — ask yourself: _if I only work within the existing design system, will the changes be visually meaningful or just incremental noise?_

**If working within the existing system is enough** (the theme has sufficient depth, the type scale has range, the spacing system is solid): proceed and apply the Visual Aesthetics Guidelines within those constraints.

**If the existing system limits impact** (flat color palette, no typographic hierarchy, uniform spacing, no surface layers): **stop and ask the user**:

> "I've analyzed the current design system. To make a meaningful visual improvement, I'd need to [describe what's missing — e.g. add a display font, introduce surface elevation, extend the color palette]. Do you want me to:
> A) Work strictly within what exists (smaller but safer changes)
> B) Propose additions to the design system to unlock bolder improvements (I'll show you what I'd add before touching any code)"

Never silently extend the design system. Never silently stay within it if doing so produces only pixel-level changes. Make the trade-off visible to the user.

### Impact check (applies to both cases)

Before finalizing any design proposal, verify:

- [ ] Has the **typographic hierarchy** changed? (sizes, weights, or scale — not just tokens)
- [ ] Has the **layout structure** changed? (not just padding values — actual arrangement of elements)
- [ ] Is there at least one decision that the user will **notice immediately** on first look?

If none of these are true, the work is not done. Incremental token swaps are not design decisions.

### Technical baseline (always inspect first)

Before making code changes, inspect the project's runtime and styling baseline:

1. **App runtime** — detect Expo vs framework-less React Native. Check `package.json`, `app.json`/`app.config.*`, and the entry file.
2. **React Native version** — note the RN version when available. Some styling capabilities are version-dependent.
3. **Architecture support** — detect whether the project uses the New Architecture before relying on newer style props such as `boxShadow`.
4. **Core UI libraries** — check for `react-native-safe-area-context`, `react-native-reanimated`, `react-native-gesture-handler`, `expo-router` / `@react-navigation/*`, `expo-blur`, and image libraries.
5. **Styling stack** — detect whether the project uses `StyleSheet.create`, NativeWind, styled-components, Restyle, or another established pattern.

If the project already has a clear stack and conventions, follow them. Do not introduce a new styling system or theming model unless the user explicitly wants that change.

---

## React Native Platform Constraints

These are hard rules — not preferences. Violating them produces broken or incorrect UI.

### Layout Engine

- All layout uses **Flexbox**. There is no CSS Grid, no `display: block`, no `float`.
- `flexDirection` defaults to `column` (NOT `row` like on the web).
- `alignItems` defaults to `stretch`. `alignContent` defaults to `flex-start`.
- `flexShrink` defaults to `0` (web default is `1`).
- All dimensions are **unitless density-independent pixels** — never `px`, `em`, `rem`, or `vw/vh`.
- Use `Dimensions.get('window')` or `useWindowDimensions()` for screen-relative sizing.
- Percentage values work (`width: '50%'`) but require a parent with defined dimensions.

### Gradients

- **CSS `background: linear-gradient(...)` does NOT work in React Native.** It is silently ignored.
- Use `expo-linear-gradient` (`<LinearGradient>`) or `react-native-linear-gradient` for all gradients.
- Pass colors as an array: `colors={['#000', '#333']}`. Control direction with `start` and `end` props.

### Shadows

- **iOS**: Use `shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius`.
- **Android**: Use `elevation` (integer). It also affects z-index stacking.
- There is no universally safe single cross-platform shadow property — handle iOS shadow props and Android `elevation` explicitly unless the project already relies on a newer supported API.
- Modern React Native exposes `boxShadow`, but it is tied to the New Architecture and newer platform support. Treat it as a version-gated enhancement, not a baseline assumption.
- `overflow: 'hidden'` on Android clips shadows completely. Avoid it on shadow containers.

### Images

- Static local images: `<Image source={require('./img/photo.png')} />` — dimensions are inferred automatically.
- Network images: `<Image source={{ uri: '...' }} style={{ width: 200, height: 200 }} />` — **you must specify dimensions manually**, or the image will not render.
- Provide `@2x` and `@3x` variants for static images to target screen densities.
- Use `<ImageBackground>` for background images, NOT `background-image` CSS.
- Modern RN supports per-corner radius props on `<Image>`, but rounded-image behavior can still be inconsistent depending on platform, resize mode, and image type. If corners do not render correctly, wrap the image in a clipping container or use Android `overlayColor` when appropriate.
- `resizeMode` values: `cover`, `contain`, `stretch`, `repeat`, `center`.

### Color

- Supports: hex (`#fff`, `#ffffff`), `rgb()`, `rgba()`, `hsl()`, `hsla()`, named CSS3 colors (lowercase only).
- Use `PlatformColor` to reference system colors (adapts to dark/light mode automatically).
- Use `DynamicColorIOS` to specify explicit light/dark color pairs on iOS.
- Use `useColorScheme()` (from `react-native`) as the base hook to detect the current theme (`'light'` | `'dark'` | `null`). This is the entry point for any manual dark/light theming.
- Prefer token-based color systems (a `colors.ts` constant file, a theme context, or Tailwind semantic tokens) over hardcoded values.

### Touch & Interaction

- Use `<Pressable>` as the default touchable primitive — it supports `pressed` state via render prop.
- `<TouchableOpacity>` is acceptable for simple opacity feedback.
- On Android, `<Pressable>` with `android_ripple` provides Material Design ripple — use it.
- The touch area **never extends past the parent view bounds** — use `hitSlop` to expand tap targets.
- Minimum tap target size: **44×44 points** (Apple HIG) / **48×48dp** (Material Design).

### Blur & Visual Effects

- Use `expo-blur` (`<BlurView>`) for frosted-glass effects — translucent modals, floating headers, tab bars with depth.
- `BlurView` has a `tint` prop: `'light'`, `'dark'`, `'default'`, `'prominent'` (iOS) and `'regular'`/`'prominent'` (Android API 31+).
- On Android < API 31, `BlurView` renders a semi-transparent fallback — always provide a solid `backgroundColor` as a graceful fallback.
- Do NOT simulate blur with opacity alone — it looks flat and does not match platform conventions.

### Lists

- Use `<FlatList>` for any list that could exceed the screen height — it virtualizes items and avoids memory issues.
- Use `<ScrollView>` only for short, bounded content (forms, detail screens with a fixed number of items).
- Always provide `keyExtractor` on `<FlatList>` — never rely on index as key for dynamic data.
- Provide `getItemLayout` if all items have a fixed height — it dramatically improves scroll performance and jump-to-index.
- Design empty states and loading skeletons as first-class UI — a blank screen or spinner-only state is an unfinished design.
- **`ListEmptyComponent` has no parent height** — `flex: 1` inside it does nothing. To vertically center empty state content, use `minHeight` with `Dimensions.get('window').height` and `justifyContent: 'center'` instead.

### Typography

- React Native does not support web fonts by default — use `expo-font` or `@expo-google-fonts/*` to load custom fonts.
- Platform defaults: iOS uses San Francisco (SF Pro), Android uses Roboto. These are acceptable for body/UI text — they are familiar to users. Override them for display/brand purposes only.
- `lineHeight` in RN is absolute (not a multiplier like CSS) — `lineHeight: 24` means 24pt, not 1.5×.
- `letterSpacing` is supported but has subtle platform differences.
- Avoid `fontWeight: 'bold'` on custom fonts — specify the exact weight variant (`fontFamily: 'Inter_700Bold'`).

### Platform Differences

- Treat negative `margin` on Android as unsafe. React Native still documents it as a known platform mismatch; prefer transforms, absolute positioning, or parent layout changes instead.
- `zIndex` works but is less reliable on Android — prefer `elevation` for stacking.
- `position: 'static'` only available with the New Architecture.
- Safe area insets differ by device — always use `<SafeAreaView>` or `useSafeAreaInsets()` from `react-native-safe-area-context`.
- Keyboard behavior differs: use `<KeyboardAvoidingView behavior={Platform.OS === 'ios' ? 'padding' : 'height'}>`.

---

## Visual Aesthetics Guidelines

### Dark Theme Reference Apps

Study and draw inspiration from:

- **Letterboxd** — deep black surfaces, careful typographic hierarchy, film-poster imagery, muted accent colors on dark
- **Spotify** — layered dark grays (not pure black), dominant album art color extraction, generous spacing
- **Narwhal (Reddit client)** — information density done well, subtle separators, no harsh borders
- **iStat Menus / Widgetsmith** — data visualization on dark, monospace + proportional font pairing

Principles for dark themes:

- Never use pure `#000000` as a background — prefer `#0D0D0D` to `#1C1C1E` (iOS system background).
- Create depth through **surface layers**: background → card → elevated card → modal (each ~8-10% lighter).
- Accent colors need higher saturation than on light — muted accents disappear on dark.
- Shadows are invisible on dark — use borders (`borderColor` with low opacity) or elevation to separate layers. **Minimum useful opacity for a border on dark is ~15% (`rgba(255,255,255,0.15)`)**. Below that it renders but is invisible to the human eye — you get the code cost without the visual benefit.
- Text hierarchy: primary `#FFFFFF` or `rgba(255,255,255,0.95)`, secondary `rgba(255,255,255,0.60)`, tertiary `rgba(255,255,255,0.38)`.

### Light Theme Reference Apps

Study and draw inspiration from:

- **Things 3** — radical whitespace, ultra-clean typography, color used sparingly for status only
- **Duolingo** — bold rounded shapes, accessible contrast, color-coded feedback states done right
- **Robinhood** — minimal chrome, data as the hero, strong typographic scale
- **Notion** — generous padding, clean card system, subdued iconography

Principles for light themes:

- Never use pure `#FFFFFF` as the only surface — combine `#FFFFFF` cards on `#F2F2F7` (iOS) or `#F5F5F5` (Android) backgrounds.
- Shadows are visible and essential — use them to establish card hierarchy.
- Color should carry meaning, not decoration — reserve brand color for primary actions.
- Text hierarchy: primary `#000000` or `#1C1C1E`, secondary `#3C3C43` at 60% opacity, tertiary at 30%.

### Typography

- Choose fonts that have **character and purpose**, not just legibility. Pair a distinctive display font with a neutral body font.
- Establish a clear type scale (minimum 4 steps: display, heading, body, caption).
- Never use uniform font sizes — hierarchy is the foundation of readable mobile UI.
- Avoid Inter, Roboto, Arial, and system-ui as **display/brand** fonts. They are readable but forgettable as a design statement. Using Roboto as the system body font on Android is fine — using it as the brand typeface is not.

### Spacing & Layout

- Use a consistent base unit (4pt or 8pt grid). All spacing, padding, and margin should be multiples of it.
- Screens should breathe — minimum horizontal padding of 16pt on most content.
- Cards benefit from 12–16pt internal padding.
- List items need at minimum 44pt height for touch comfort.
- Avoid crowding the edges: respect `SafeAreaInsets` and add extra bottom padding for home-indicator devices.

### Color & Theme

- Commit to a dominant color (background) + surface color + one brand accent. Anything beyond that must earn its place.
- Status colors (success, warning, error) should always differ from brand colors.
- If supporting both light and dark, use a theme context or token system — never condition on `colorScheme` inline in every component.

### Motion & Feedback

- **Prefer `react-native-reanimated`** for gesture-heavy UI, shared transitions, scroll-linked effects, and any animation the user will notice repeatedly.
- The built-in `Animated` API is still valid for simple opacity/transform cases, especially when `useNativeDriver` is available. Do not rewrite an existing simple `Animated` flow to `reanimated` unless there is a clear payoff.
- `LayoutAnimation` is fast to implement for simple layout transitions (list reorders, expand/collapse) but runs on the JS thread — avoid for performance-sensitive screens.
- Provide immediate visual feedback on every touch interaction — zero-delay state change on press.
- Reserve animations for transitions that help the user understand spatial relationships (not decoration).

---

## Styling Approach — Stack Agnostic

This skill applies regardless of styling library. **Before generating any code, check `package.json` dependencies** to detect the stack in use:

- Contains `nativewind` → inspect the NativeWind setup first and use `className` plus the established Tailwind token system
- Contains `styled-components` → use tagged template literals with `ThemeProvider`
- Contains `@shopify/restyle` → use `Box`/`Text` primitives with theme tokens
- None of the above → default to `StyleSheet.create`

| Stack                      | Pattern                                                                                              |
| -------------------------- | ---------------------------------------------------------------------------------------------------- |
| `StyleSheet.create`        | Plain RN objects, no runtime overhead, best for performance-critical components                      |
| `NativeWind` / Tailwind    | `className` prop on RN core components, semantic utility classes, design tokens in `tailwind.config` |
| `styled-components/native` | Tagged template literals, theme via `ThemeProvider`, interpolated theme tokens                       |
| `@shopify/restyle`         | Type-safe theme system, `Box`/`Text` primitives, enforces design tokens                              |

Regardless of stack:

- Never hardcode colors — reference a theme token or Tailwind semantic color.
- **Check token consistency between visually adjacent elements** (e.g. a badge border-color and its parent container background, a floating dot and the surface it sits on). On dark themes, two tokens that differ by 5-10% luminosity produce a visible unintended ring or halo — use the same token or remove the border entirely.
- Never mix styling approaches within the same component.
- Prefer composition (`<Card>`, `<Row>`, `<Stack>`) over deeply nested inline styles.

### NativeWind / Tailwind Detection

If `nativewind` or `tailwindcss` appears in the project, inspect these files before editing anything:

- `package.json` — dependencies, scripts, and whether the app uses Expo
- `tailwind.config.js|cjs|ts` — content globs, presets, theme extension, plugins
- `global.css` or the configured CSS input — Tailwind directives, theme variables, custom layers
- `babel.config.js|cjs` — `nativewind/babel` and `jsxImportSource`
- `metro.config.js|cjs` — `withNativeWind(...)` or equivalent Metro integration
- `nativewind-env.d.ts` — TypeScript environment support when present

Use NativeWind's conventions, not generic Tailwind-web assumptions.

- `className` works cleanly on core React Native components and on custom components that forward props correctly.
- Do not assume third-party components accept `className`. If they do not pass props through, NativeWind may require `remapProps` or `cssInterop`.
- `cssInterop` is powerful but not free — use it when needed, not by default.
- If the project already uses semantic tokens via `tailwind.config` or CSS variables, stay inside that token model instead of introducing ad-hoc utility colors.
- For dark mode in NativeWind, prefer the project's current approach. Many apps can use `dark:` variants directly; others may rely on `useColorScheme` / `colorScheme` integration.
- Be careful when mixing `className` with inline `style`. Use inline styles for calculated values, runtime measurements, or props that do not map cleanly to utilities. Keep the dominant styling source consistent within the component.

---

## What to NEVER Do

- ❌ `background: 'linear-gradient(...)'` — use `<LinearGradient>`
- ❌ CSS pseudo-selectors (`:hover`, `:focus`, `::before`) — they do not exist
- ❌ Assuming web CSS maps directly to RN. Even when a newer prop exists, verify platform/version support first.
- ❌ `display: 'grid'` or `display: 'block'` — only `flex` and `none` available
- ❌ Assuming a web Tailwind pattern is automatically valid in NativeWind — confirm the RN equivalent first.
- ❌ `px`, `em`, `rem` units — all values are unitless dp
- ❌ Uppercase CSS color names (`Red`, `Blue`) — only lowercase supported
- ❌ Generic, AI-slop aesthetics: purple gradients on white, Inter everywhere, cards with no hierarchy, flat gray UIs with no soul
- ❌ Relying on negative margins for Android layout if another solution is available
- ❌ Network images without explicit `width` and `height` in style — they will not render
- ❌ Font weights as `'bold'` with custom fonts — use the correct named weight variant

---

## File Edit Safety

When editing existing style files, **always read the full file before writing**. The most common failure mode is producing duplicate declarations when a block replacement inserts new content without removing the original block.

Before applying any edit to a styles file:

1. **Read the entire file** — identify the exact block being replaced, including all its lines.
2. **Replace the full block** — the replaced range must include every line of the original component, from `export const` to the closing backtick. Never use a partial match.
3. **After editing, verify** — check for compile errors. `Cannot redeclare block-scoped variable` means a duplicate was introduced; locate and remove the stale copy immediately.
4. **One replacement per component** — never add a new version of a component at the top of a file and leave the old version at the bottom.

If a replacement fails to match exactly: read the file again to get the literal text, then retry with the exact content.

---

## Output Requirements

Generated code must:

- Be **immediately runnable** — no placeholders, no `// TODO`, no missing imports
- Use the same styling approach already present in the file/project (detect from context)
- Respect `SafeAreaView` and platform-specific layout needs
- Include both iOS and Android considerations where behavior differs
- Match the aesthetic vision with precision — bold choices executed cleanly
- Include accessible touch targets, readable contrast, and sensible support for font scaling unless the project already has a deliberate alternative
- Handle first-run realism: loading, empty, error, and offline-adjacent states when the screen obviously needs them
- Verify the result on at least one small-screen mental model and one large-screen mental model before considering the work done

---
name: react-native-design
description: "Build, redesign, critique, refactor, or debug React Native UI with strong visual direction and platform-correct implementation. Handles mobile constraints (gradients, shadows, images, flexbox), diagnoses existing design systems before proposing changes, and adapts to the project's styling stack, including StyleSheet, NativeWind/Tailwind, styled-components, and Restyle. Use when creating or improving any React Native screen or component, translating web/Figma ideas to React Native, auditing an existing mobile UI, or fixing iOS/Android-specific styling and interaction issues. Also use when: creating a full theme from scratch, generating color palettes, designing specific views, improving existing color combinations, or making design decisions about layout, navigation, and component structure."
---

# react-native-design

This skill guides the creation of distinctive, production-grade React Native UI that avoids generic "AI slop" aesthetics and respects the hard constraints of the mobile platform. It acts as a **complete UX/UI replacement** — capable of generating color systems, designing screens from scratch, critiquing and improving existing UI, and making every design decision that a professional designer would make.

The user provides a screen, component, UI direction, app concept, or design problem. They may include context about the purpose, audience, target platform, existing styling approach, or preferred colors.

---

## Core Philosophy

1. **Every pixel is a decision.** Nothing should be accidental — not a color, not a spacing value, not a font choice.
2. **Design is problem-solving, not decoration.** The UI must serve the user's task, not look impressive in a screenshot.
3. **Platform correctness is non-negotiable.** Beautiful design that crashes or renders incorrectly is worthless.
4. **Consistency compounds.** A simple system applied rigorously beats complex rules applied inconsistently.
5. **Intentionality over trends.** A deliberate choice to be minimal is as powerful as a choice to be bold.

---

## Design Thinking — Expanded Framework

Before touching any code, answer these questions (internally or by asking the user):

### 1. Purpose & Audience

- What problem does this app/screen solve?
- Who is the target user? (age, tech literacy, context of use — commuting? at home? at work?)
- What emotional response should the UI trigger? (trust, excitement, calm, urgency, delight)
- Will users spend seconds or minutes on this screen?

### 2. Aesthetic Direction

Commit to ONE personality. Do not default to "clean and modern" — that is a non-choice.

| Aesthetic                    | Characteristics                                                                      | When to use                                       |
| ---------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------- |
| **Cinematic / Immersive**    | Deep darks, rich imagery, dramatic typography, ambient lighting effects              | Media, entertainment, streaming, photography      |
| **Editorial / Magazine**     | Strong typographic hierarchy, asymmetric layouts, generous whitespace, bold headings | Content-heavy, news, blogs, long-form reading     |
| **Luxury / Refined**         | Restrained palette, subtle animations, premium materials feel, serif fonts           | Finance, premium services, fashion                |
| **Brutalist / Raw**          | High contrast, exposed structure, monospace type, no rounded corners                 | Developer tools, creative portfolios, avant-garde |
| **Playful / Friendly**       | Rounded shapes, saturated colors, bouncy animations, illustration-heavy              | Education, kids, social, gamified experiences     |
| **Utilitarian / Data-dense** | Compact layouts, tables/grids, monospace numbers, minimal decoration                 | Dashboards, trading, analytics, productivity      |
| **Organic / Calm**           | Muted earth tones, soft gradients, breathing animations, natural imagery             | Wellness, meditation, health, journaling          |
| **Neo-brutalist / Bold**     | Thick borders, offset shadows, bright primaries, visible grid                        | Creative tools, indie products, portfolios        |

### 3. Theme Preference

- **Dark**: Depth, layers, glowing accents, contrast through luminosity
- **Light**: Whitespace, shadows for depth, color used sparingly
- **Both (dual)**: Requires a token system that maps semantic roles, not hardcoded values

### 4. Differentiation

- What is the ONE thing a user will notice and remember?
- What makes this different from the 10 closest competitors?

CRITICAL: Intentionality beats intensity. A well-executed minimal design and a well-executed maximalist design are both excellent. A half-hearted attempt at either is not.

---

## Color System — Complete Theory & Generation

This section enables creating professional color palettes from scratch, from user-provided colors, or improving existing ones.

### Color Theory Fundamentals

Every professional palette is built on color relationships:

| Scheme                  | Definition                        | Character                                | Best for                                    |
| ----------------------- | --------------------------------- | ---------------------------------------- | ------------------------------------------- |
| **Monochromatic**       | One hue, multiple tints/shades    | Elegant, cohesive, low cognitive load    | Utility apps, dashboards, minimalist brands |
| **Analogous**           | 2-3 adjacent hues on the wheel    | Harmonious, natural, warm/cool bias      | Lifestyle, wellness, organic brands         |
| **Complementary**       | Opposite hues (180°)              | High energy, vibrant tension             | CTAs, alerts, gaming, sports                |
| **Split-complementary** | Base + two adjacent to complement | Vibrant but balanced, less harsh         | Most apps — versatile and dynamic           |
| **Triadic**             | Three hues 120° apart             | Balanced energy, colorful but controlled | Multi-category apps, social, creative tools |

### Palette Generation Process

**Starting from an app concept (no user colors):**

1. Identify the emotional tone → pick a dominant hue family:
   - Trust/stability → Blue (210°–240°)
   - Energy/passion → Red-Orange (0°–30°)
   - Growth/health → Green (120°–160°)
   - Creativity/luxury → Purple (270°–310°)
   - Warmth/optimism → Yellow-Orange (30°–60°)
   - Calm/nature → Teal-Cyan (170°–200°)
   - Sophistication/tech → Blue-Violet (240°–270°)

2. Choose a relationship scheme (split-complementary is the safest default)

3. Generate the full system using the token structure below

**Starting from user-provided colors (1-3 colors):**

1. Identify the provided color(s) as HSL values
2. Determine their role: is it accent? background? brand mark?
3. Derive the missing roles:
   - If user gives **1 accent color**: build surfaces around it (dark: desaturate + darken for backgrounds; light: tint for surfaces)
   - If user gives **2 colors**: determine their relationship, fill remaining slots
   - If user gives **a brand palette**: respect the hierarchy, add functional colors (semantic states, surface layers)

4. Generate tints/shades of the accent at these stops: `50, 100, 200, 300, 400, 500 (base), 600, 700, 800, 900, 950`

**Improving an existing palette:**

1. Read ALL color tokens in the project
2. Check for these common problems:
   - Accent too close to semantic colors (can't distinguish "success" from "primary")
   - Surface layers without enough contrast between steps (< 5% luminosity gap = invisible)
   - Text colors that don't meet WCAG AA (4.5:1 for body, 3:1 for large text)
   - Too many unique colors that don't relate to each other
   - Borders/strokes that create unintended halos against their backgrounds
3. Propose specific fixes with before/after hex values

### Color Token Architecture — Mandatory Structure

Every theme MUST have these semantic layers. Names can vary by project convention, but the roles must be present:

```
SURFACES (background layers — establish depth)
├── base         → absolute deepest background (dark: #070B11-ish, light: #F8FAFC-ish)
├── main         → primary screen background
├── elevated     → cards, modals, floating elements
├── overlay      → scrim behind modals (semi-transparent)
└── interactive  → hover/pressed states on surfaces

ACCENT (brand color)
├── main         → primary accent (buttons, links, active states)
├── container    → filled surfaces with accent (CTAs, badges, selected states)
├── subtle       → tinted surfaces (sidebar active item, selected card bg)
├── on-accent    → text/icon color ON the accent fill

TEXT (foreground hierarchy)
├── primary      → highest contrast — titles, critical content
├── secondary    → body text, descriptions
├── tertiary     → muted — placeholders, metadata, timestamps
├── disabled     → clearly inactive
└── on-color     → text on colored surfaces (accent, semantic)

BORDERS (stroke hierarchy)
├── default      → card borders, dividers
├── subtle       → barely visible separators
├── strong       → focused inputs, active states
└── accent       → accent-colored borders

SEMANTIC (functional — MUST differ from accent)
├── success      → main + surface + text
├── warning      → main + surface + text
├── error        → main + surface + text
└── info         → main + surface + text

COMPONENT-SPECIFIC (optional but recommended)
├── button       → primary/secondary/ghost/danger states
├── input        → default/focused/error fills and strokes
├── navigation   → tab colors, active indicators
└── card         → specific card backgrounds if they differ from general surfaces
```

### Dark Theme Color Rules

| Rule              | Guidance                                                                                                                |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Background base   | NEVER pure `#000000`. Use `#050505`–`#121212`. Tint the black with your brand hue for warmth                            |
| Surface steps     | Each layer 5-12% lighter. Minimum 4 layers. Test: put a card on the background — can you see the edge without a border? |
| Accent saturation | Increase saturation 10-20% vs light theme. Muted accents disappear on dark                                              |
| Accent luminosity | The accent must be L≥50% in HSL to be readable against dark surfaces                                                    |
| Borders           | Use `rgba(255,255,255, 0.08–0.15)` or accent at low opacity. Anything below 8% opacity is invisible                     |
| Text primary      | `#F1F5F9` to `#FFFFFF` (never below L:90%)                                                                              |
| Text secondary    | `rgba(255,255,255, 0.55–0.70)`                                                                                          |
| Text tertiary     | `rgba(255,255,255, 0.35–0.45)`                                                                                          |
| Shadows           | Invisible. Don't waste code on them. Use border + elevation instead                                                     |
| Gradients         | Subtle — used for depth on CTAs or hero sections, not decoration                                                        |

### Light Theme Color Rules

| Rule              | Guidance                                                                               |
| ----------------- | -------------------------------------------------------------------------------------- |
| Background base   | `#F2F2F7` (iOS) or `#F5F5F5` (Material). Not flat white                                |
| Card surfaces     | Pure `#FFFFFF` or near-white. Shadows create separation                                |
| Accent saturation | Can be lower than dark theme — the contrast is already there                           |
| Accent luminosity | Must be L≤45% for readable text on white backgrounds                                   |
| Borders           | `rgba(0,0,0, 0.06–0.12)` for subtle dividers                                           |
| Text primary      | `#1C1C1E` to `#000000`                                                                 |
| Text secondary    | `rgba(0,0,0, 0.55–0.65)`                                                               |
| Shadows           | Essential for hierarchy — `shadowOpacity: 0.04–0.12` for cards, `0.15–0.25` for modals |

### Dual Theme System (Light + Dark)

When building both themes:

1. **Never hardcode colors in components** — always reference semantic tokens
2. **Map by role, not by value** — `background.main` resolves to different hex per theme
3. **Implementation patterns** (detect what the project uses):

```typescript
// styled-components: ThemeProvider swap
const darkTheme = { colors: { bg: { main: '#0C1219' }, ... } };
const lightTheme = { colors: { bg: { main: '#F8FAFC' }, ... } };

// NativeWind: dark: prefix OR CSS variables
// className="bg-surface-main dark:bg-surface-main-dark"

// Context-based: useTheme() hook returning current tokens
```

4. **Test both themes** — every component must be verified against both palettes

### Accessibility — Color Contrast Requirements

| Element                        | Minimum ratio (WCAG AA) | Target ratio |
| ------------------------------ | ----------------------- | ------------ |
| Body text on background        | 4.5:1                   | 7:1 (AAA)    |
| Large text (≥18px bold, ≥24px) | 3:1                     | 4.5:1        |
| UI components (borders, icons) | 3:1                     | 4.5:1        |
| Decorative/disabled            | No requirement          | —            |

**Quick contrast check formula** (for dark themes):

- Text at `rgba(255,255,255, 0.87)` on `#1C1C1E` = ~12:1 ✓
- Text at `rgba(255,255,255, 0.60)` on `#1C1C1E` = ~8:1 ✓
- Text at `rgba(255,255,255, 0.38)` on `#1C1C1E` = ~5:1 ✓ (large text only)

**For accents as text**: The accent color must have ≥4.5:1 against its surface. Teal `#2DD4BF` on `#0C1219` ≈ 9:1 ✓. But teal on `#253244` ≈ 5.5:1 — use with caution on elevated surfaces.

---

## Theme Creation — Step-by-Step Workflows

### Workflow A: Create a Theme from Scratch (New App)

When a user says "I'm building an app for X, create a theme":

1. **Ask or infer** the app category, audience, and aesthetic direction
2. **Choose a dominant hue** based on the emotional tone (see table above)
3. **Select a color scheme** (default to split-complementary unless the app is clearly monochromatic)
4. **Generate the full token structure** following the Color Token Architecture
5. **Select fonts**:
   - Display/brand: distinctive, memorable (not Inter/Roboto/system default)
   - Body/UI: highly legible, with available weight variants
   - Use `@expo-google-fonts` for easy loading
6. **Define the type scale** (minimum: display, title, headline, body, caption, label)
7. **Define spacing** (4pt or 8pt base grid)
8. **Define border radius** (consistent set: none, sm, md, lg, full)
9. **Output**: Complete theme file + type definitions + font loading instructions

### Workflow B: Improve an Existing Theme

When a user says "improve my colors" or "the UI feels off":

1. **Read all theme files** (colors, typography, spacing, components)
2. **Audit against the rules** in this skill:
   - Surface contrast steps sufficient?
   - Accent readable on all its surfaces?
   - Semantic colors distinct from accent?
   - Typography hierarchy has enough range? (min 3:1 ratio between smallest and largest)
   - Spacing consistent with a grid?
3. **Identify top 3 issues** — prioritize by visual impact
4. **Propose changes** with exact hex values, showing before → after
5. **Explain the reasoning** briefly (not pages of theory — just "this fixes X because Y")

### Workflow C: Create a Theme from User Colors

When a user provides specific colors:

1. **Analyze the colors** — convert to HSL, identify hue family, saturation, luminosity
2. **Assign roles**: which is accent? which is surface? Let the most saturated/distinctive color be the accent
3. **Generate complementary colors** using the theory above
4. **Build the full surface stack** around the provided colors
5. **Verify contrast** of every text/surface combination
6. **Output**: Complete theme, noting which colors are user-provided vs generated

### Workflow D: Design a Specific View

When a user says "design a login screen" or "create a profile view":

1. **Inspect existing theme and components** — work within what exists
2. **Select the appropriate screen pattern** from the catalog below
3. **Apply the theme tokens** — never introduce new colors not in the system
4. **Structure the layout** following the Layout Decision Framework
5. **Handle all states**: default, loading, error, empty, success
6. **Output**: Complete view code, styles, and types if applicable

---

## Screen Pattern Catalog

Reference designs for common screens. Each pattern includes structure, key design decisions, and what separates good from great.

### Authentication (Login / Register / Forgot Password)

**Structure**: Centered form, minimal distraction, clear primary CTA, secondary links below.

| Decision      | Guidance                                                                                  |
| ------------- | ----------------------------------------------------------------------------------------- |
| Visual weight | The input fields and CTA should dominate — minimize logos and decoration                  |
| Background    | Solid dark/light surface OR subtle gradient. Never busy imagery that competes with inputs |
| Inputs        | Generous height (48-56pt), clear focus states, inline validation                          |
| CTA           | Full-width, high contrast, immediate loading feedback                                     |
| Social login  | Secondary visual weight — below the primary form                                          |
| Keyboard      | Must not cover the active input. Use `KeyboardAvoidingView` + scroll                      |
| Error display | Inline per-field (red text below input) + optional toast for network errors               |

**Great**: Smooth keyboard transitions, immediate field validation, subtle brand personality through font/color choice, haptic on submit.

### Home / Dashboard

**Structure**: Scrollable surface with sections — greeting/status → primary content → secondary content.

| Decision          | Guidance                                                                                          |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| Header            | Personalized greeting OR key metric. Not generic "Welcome back"                                   |
| Content hierarchy | What does the user want MOST when they open the app? That goes first                              |
| Cards vs List     | Cards for visual/browseable content (media, products). Lists for scannable data (tasks, messages) |
| Empty first-use   | Dedicated onboarding prompt, not just "No items yet"                                              |
| Pull-to-refresh   | Expected if data is remote                                                                        |
| Floating action   | Only if there's ONE dominant creation action                                                      |

**Great**: Content-first (data visible without scrolling), progressive disclosure, personalized ordering.

### Detail View (Item/Product/Content)

**Structure**: Hero image/visual → title + metadata → body content → actions.

| Decision        | Guidance                                                                |
| --------------- | ----------------------------------------------------------------------- |
| Hero            | Full-width image or large visual. Aspect ratio 16:9 or 3:4 for media    |
| Scroll behavior | Image collapses on scroll, title becomes the header (parallax optional) |
| Metadata        | Compact row: date, category, rating — secondary text weight             |
| Actions         | Sticky bottom bar OR contextual buttons within content                  |
| Back navigation | Always visible — overlay on hero image if present                       |

**Great**: Smooth scroll-linked transitions, image loading with shimmer/blur-up, rich metadata without clutter.

### List / Collection

**Structure**: Optional filters/tabs at top → scrollable list → empty state.

| Decision    | Guidance                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------ |
| Layout      | Vertical list for text-heavy items. Grid (2-3 columns) for visual items (photos, products) |
| Item height | Consistent per type. Text list: 64-88pt. Visual grid: square or 3:4                        |
| Separator   | Subtle line (light theme) or gap (dark theme). Not both                                    |
| Filters     | Horizontal scroll pills OR top tabs for < 5 categories. Sheet/modal for complex filters    |
| Search      | Sticky at top if list is > 20 items                                                        |
| Loading     | Skeleton shimmer matching item shape — not a centered spinner                              |
| Empty       | Illustration + message + CTA. Centered vertically                                          |
| Pagination  | Infinite scroll with bottom loader for most apps. Explicit "Load more" for formal content  |

**Great**: Smooth scroll performance (FlatList + getItemLayout), item animations on appear, pull-to-refresh with custom animation.

### Profile / Settings

**Structure**: Avatar + info header → sections grouped by category → danger zone at bottom.

| Decision    | Guidance                                                                                  |
| ----------- | ----------------------------------------------------------------------------------------- |
| Header      | Avatar (or initials fallback) + name + optional bio/subtitle                              |
| Sections    | Group related items. Use section headers to separate concerns                             |
| Navigation  | Chevron/arrow for items that navigate to a sub-screen                                     |
| Toggles     | For instant-apply boolean settings. Not for actions                                       |
| Danger zone | Log out, delete account — visually distinct (red text or separated section) at the bottom |
| Edit mode   | Separate edit screen OR inline editing with clear save/cancel                             |

**Great**: Smooth avatar upload with crop, grouped sections with clear visual separation, confirmation for destructive actions.

### Search

**Structure**: Search input (auto-focused) → recent/suggestions → results.

| Decision          | Guidance                                                                    |
| ----------------- | --------------------------------------------------------------------------- |
| Input             | Auto-focus on mount. Full-width. Clear button on right                      |
| State: empty      | Recent searches + trending/suggested (if applicable)                        |
| State: typing     | Real-time suggestions (debounced 300ms minimum)                             |
| State: results    | Same layout as the collection it searches                                   |
| State: no results | Friendly message + suggestions. Not just "No results"                       |
| Cancel            | Platform-correct: iOS shows "Cancel" text button. Android uses back gesture |

**Great**: Instant results feeling (optimistic UI), search highlighting in results, filter integration.

### Onboarding / Walkthrough

**Structure**: 2-4 pages maximum → skip option → final CTA.

| Decision   | Guidance                                                                                 |
| ---------- | ---------------------------------------------------------------------------------------- |
| Pages      | Maximum 4. Each communicates ONE benefit. Not features — benefits                        |
| Navigation | Swipeable pages with dot indicator. "Next" and "Skip" always visible                     |
| Visuals    | Each page has ONE dominant visual (illustration, screenshot, or icon). Not walls of text |
| Final      | Clear CTA that enters the app. "Get Started" not "Done"                                  |
| Skip       | Always available. Some users don't want onboarding                                       |
| Re-entry   | Settings should have "Show onboarding" option                                            |

**Great**: Subtle page transition animations, micro-interactions on illustrations, progress feels fast (never sluggish).

### Modal / Bottom Sheet

**Structure**: Handle (for sheets) → title → content → actions.

| Decision       | Guidance                                                                   |
| -------------- | -------------------------------------------------------------------------- |
| Type           | Bottom sheet for contextual options. Center modal for confirmations/alerts |
| Handle         | Gray pill indicator (4×36pt) at top of bottom sheets                       |
| Dismiss        | Swipe down + backdrop tap. Never trap the user                             |
| Content height | Auto-height to content (max 85% screen). Scroll internally if needed       |
| Actions        | Primary right/bottom. Destructive in red. Cancel on left                   |
| Backdrop       | `rgba(0,0,0, 0.4–0.6)` with blur if available                              |

### Tab / Navigation Structure

| Pattern           | When to use                                                     |
| ----------------- | --------------------------------------------------------------- |
| Bottom tabs (3-5) | Primary navigation for most apps                                |
| Top tabs          | Sub-categories within a section (e.g., "All / Movies / Series") |
| Drawer            | Many sections (> 5), settings-heavy apps, enterprise            |
| Stack-only        | Linear flows (onboarding, checkout), single-purpose apps        |
| Tab + Stack       | Most common: tabs for sections, stack for detail/sub-flows      |

---

## Layout Decision Framework

When deciding HOW to structure a view, follow this decision tree:

### Content Type → Layout Choice

```
Is the content a list of similar items?
├── YES → Are items visual (images/thumbnails)?
│   ├── YES → Grid (2-3 cols) OR horizontal scroll carousel
│   └── NO  → Vertical list (FlatList)
└── NO  → Is it a form?
    ├── YES → Vertical scroll, single column, generous spacing
    └── NO  → Is it mixed content (hero + details + actions)?
        ├── YES → ScrollView with sections
        └── NO  → Is it a dashboard with multiple data types?
            ├── YES → Sections with different component types
            └── NO  → Centered content (auth, empty states, confirmation)
```

### Spacing Rules

| Context           | Horizontal padding       | Vertical spacing between sections | Internal card padding |
| ----------------- | ------------------------ | --------------------------------- | --------------------- |
| Standard screen   | 16-20pt                  | 24-32pt                           | 12-16pt               |
| Data-dense screen | 12-16pt                  | 16-20pt                           | 8-12pt                |
| Immersive/media   | 0pt (full-bleed) to 16pt | 16-24pt                           | 12-16pt               |
| Form              | 16-24pt                  | 16-24pt                           | —                     |

### Component Size Reference

| Element                 | Minimum height              | Recommended      |
| ----------------------- | --------------------------- | ---------------- |
| Touch target            | 44pt (iOS) / 48dp (Android) | 48pt             |
| Button                  | 44pt                        | 48-56pt          |
| Input field             | 44pt                        | 48-56pt          |
| List item (text only)   | 44pt                        | 56-72pt          |
| List item (with avatar) | 56pt                        | 64-80pt          |
| Tab bar                 | 49pt (iOS) / 56dp (Android) | Platform default |
| Navigation header       | 44pt (iOS) / 56dp (Android) | Platform default |
| Card (compact)          | —                           | 120-160pt        |
| Card (featured)         | —                           | 200-280pt        |

---

## Project Context — Detect Before Designing

Before proposing any changes, determine what kind of project you are working with:

### New project (no existing screens or theme)

- Full creative freedom. Follow **Workflow A** from Theme Creation.
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

### Reference Apps by Category

Study these apps for specific design qualities. When designing for a particular domain, reference the relevant category:

**Media & Entertainment** (Dark-first, immersive)

- **Letterboxd** — deep black surfaces, careful typographic hierarchy, film-poster imagery, muted accent colors
- **Spotify** — layered dark grays (not pure black), dominant art color extraction, generous spacing
- **Netflix** — full-bleed imagery, minimal chrome, red as the only accent, auto-playing heroes
- **Apple Music** — blurred album backgrounds, adaptive colors, clean segmented controls

**Productivity & Utility**

- **Things 3** — radical whitespace, ultra-clean typography, color used sparingly for status only
- **Notion** — generous padding, clean card system, subdued iconography, type as the hero
- **Linear** — dark purple-black surfaces, vibrant status colors, excellent keyboard shortcuts UX
- **Todoist** — clear priority system through color, satisfying completion animations

**Finance & Data**

- **Robinhood** — minimal chrome, data as the hero, strong typographic scale, green/red only for gains/loss
- **Revolut** — premium dark theme, card-as-hero design, smooth transitions between sections
- **Coinbase** — clean data visualization, restrained color, clear hierarchy in complex data

**Social & Communication**

- **Discord** — comfortable dark theme for long sessions, clear hierarchy between channels/messages/servers
- **Threads** — minimal, content-first, generous whitespace, typography-driven

**Health & Wellness**

- **Headspace** — warm illustrations, rounded shapes, calming palette, clear progress tracking
- **Strava** — activity-centric, maps as heroes, motivational use of personal data

**E-commerce**

- **GOAT/StockX** — product-as-hero, clean detail views, clear pricing hierarchy
- **Airbnb** — map + list integration, excellent photo galleries, clear booking flow

### Dark Theme Principles

- Never use pure `#000000` as a background — prefer `#0D0D0D` to `#1C1C1E`. Better yet, tint the base with your brand hue at very low saturation (e.g., for a teal accent: `hsl(210, 25%, 5%)` gives a blue-black with soul).
- Create depth through **surface layers**: background → card → elevated card → modal (each ~6-10% lighter).
- Accent colors need higher saturation than on light — muted accents disappear on dark.
- Shadows are invisible on dark — use borders (`borderColor` with low opacity) or elevation to separate layers. **Minimum useful opacity for a border on dark is ~8-15% (`rgba(255,255,255,0.08)` to `rgba(255,255,255,0.15)`)**. Below 8% is invisible to the human eye.
- Text hierarchy: primary `#F1F5F9` or `rgba(255,255,255,0.92)`, secondary `rgba(255,255,255,0.60)`, tertiary `rgba(255,255,255,0.38)`.
- Use subtle gradients on interactive elements to imply light source and dimensionality.

### Light Theme Principles

- Never use pure `#FFFFFF` as the only surface — combine `#FFFFFF` cards on `#F2F2F7` (iOS) or `#F5F5F5` (Android) backgrounds.
- Shadows are visible and essential — use them to establish card hierarchy.
- Color should carry meaning, not decoration — reserve brand color for primary actions.
- Text hierarchy: primary `#000000` or `#1C1C1E`, secondary `#3C3C43` at 60% opacity, tertiary at 30%.
- Borders are optional when shadows are doing the work — don't double up.

### Typography Best Practices

- Choose fonts that have **character and purpose**, not just legibility.
- **Font pairing strategy**: 1 display font (personality) + 1 body font (legibility). Never more than 2 font families.
- Establish a clear type scale (minimum 5 steps: display, title, headline, body, caption).
- **Scale ratio**: use a consistent multiplier between steps. 1.2 (minor third) for compact UIs, 1.25 (major third) for standard, 1.333 (perfect fourth) for spacious.
- Never use uniform font sizes — hierarchy is the foundation of readable mobile UI.
- Avoid Inter, Roboto, Arial, and system-ui as **display/brand** fonts. They are readable but forgettable.

**Recommended expo-google-fonts for brand/display use:**
| Style | Fonts |
|-------|-------|
| Modern geometric | Outfit, Sora, General Sans (not on Google), Plus Jakarta Sans |
| Editorial serif | Playfair Display, DM Serif Display, Libre Baskerville |
| Technical/mono | Space Mono, JetBrains Mono, Fira Code |
| Friendly rounded | Nunito, Comfortaa, Quicksand |
| Bold/impact | Archivo Black, Anton, Bebas Neue |
| Elegant sans | Cormorant, Josefin Sans, Raleway |

### Spacing & Layout

- Use a consistent base unit (4pt or 8pt grid). All spacing, padding, and margin should be multiples of it.
- Screens should breathe — minimum horizontal padding of 16pt on most content.
- Cards benefit from 12–16pt internal padding.
- List items need at minimum 44pt height for touch comfort.
- **Vertical rhythm**: consistent spacing between sections creates scanability. Vary section gaps (larger) vs item gaps (smaller).
- Avoid crowding the edges: respect `SafeAreaInsets` and add extra bottom padding for home-indicator devices (34pt on modern iPhones).

### Motion & Feedback

- **Prefer `react-native-reanimated`** for gesture-heavy UI, shared transitions, scroll-linked effects, and any animation the user will notice repeatedly.
- The built-in `Animated` API is still valid for simple opacity/transform cases, especially when `useNativeDriver` is available. Do not rewrite an existing simple `Animated` flow to `reanimated` unless there is a clear payoff.
- `LayoutAnimation` is fast to implement for simple layout transitions (list reorders, expand/collapse) but runs on the JS thread — avoid for performance-sensitive screens.
- Provide immediate visual feedback on every touch interaction — zero-delay state change on press.
- Reserve animations for transitions that help the user understand spatial relationships (not decoration).
- **Timing guidelines**: 100-200ms for micro-interactions (press, toggle), 250-400ms for page transitions, 300-500ms for modals/sheets.
- **Easing**: use `Easing.bezier(0.25, 0.1, 0.25, 1)` (ease-out) for entrances, `Easing.bezier(0.55, 0, 1, 0.45)` (ease-in) for exits.

---

## State Design — Every Screen Has Multiple States

A screen is not designed until ALL its states are designed. Never deliver only the "happy path":

### Required States

| State                      | Design requirements                                                  |
| -------------------------- | -------------------------------------------------------------------- |
| **Loading (first load)**   | Skeleton shimmer matching content shape. NOT a centered spinner      |
| **Loading (refresh)**      | Pull-to-refresh indicator OR inline indicator. Content stays visible |
| **Empty (first use)**      | Illustration/icon + explanation + primary CTA. Vertically centered   |
| **Empty (after deletion)** | Similar to first-use but contextual ("No favorites yet")             |
| **Error (network)**        | Friendly message + retry button. Not a stack trace                   |
| **Error (validation)**     | Inline per-field, red accent, icon + text                            |
| **Success (transient)**    | Snackbar/toast or animation. Disappears after 2-4s                   |
| **Partial content**        | Graceful degradation — show what you have, indicate what's missing   |
| **Offline**                | Banner at top ("No connection") + cached content still visible       |

### Skeleton Design Rules

- Match the SHAPE of real content — if the card has an image on the left and two lines on the right, the skeleton does too
- Use animated shimmer (left-to-right gradient sweep) — not static gray blocks
- Color: `rgba(255,255,255, 0.05-0.08)` on dark, `rgba(0,0,0, 0.04-0.08)` on light
- Never skeleton an entire screen for more than 2 seconds — after that, show a progress indicator or partial content

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

### NativeWind / Tailwind — Complete Integration Guide

If `nativewind` or `tailwindcss` appears in the project, inspect these files before editing anything:

- `package.json` — dependencies, scripts, and whether the app uses Expo
- `tailwind.config.js|cjs|ts` — content globs, presets, theme extension, plugins
- `global.css` or the configured CSS input — Tailwind directives, theme variables, custom layers
- `babel.config.js|cjs` — `nativewind/babel` and `jsxImportSource`
- `metro.config.js|cjs` — `withNativeWind(...)` or equivalent Metro integration
- `nativewind-env.d.ts` — TypeScript environment support when present

Use NativeWind's conventions, not generic Tailwind-web assumptions:

- `className` works cleanly on core React Native components and on custom components that forward props correctly.
- Do not assume third-party components accept `className`. If they do not pass props through, NativeWind may require `remapProps` or `cssInterop`.
- `cssInterop` is powerful but not free — use it when needed, not by default.
- If the project already uses semantic tokens via `tailwind.config` or CSS variables, stay inside that token model instead of introducing ad-hoc utility colors.
- For dark mode in NativeWind, prefer the project's current approach. Many apps can use `dark:` variants directly; others may rely on `useColorScheme` / `colorScheme` integration.
- Be careful when mixing `className` with inline `style`. Use inline styles for calculated values, runtime measurements, or props that do not map cleanly to utilities.

**Creating a Tailwind theme from scratch** (when user requests):

```javascript
// tailwind.config.js — structure for a complete dark theme
module.exports = {
  content: ['./src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        surface: {
          base: '#070B11',
          main: '#0C1219',
          elevated: '#141C27',
          overlay: 'rgba(0,0,0,0.6)',
        },
        accent: {
          DEFAULT: '#2DD4BF', // primary accent
          container: '#14B8A6',
          subtle: '#0D2D28',
        },
        content: {
          primary: '#F1F5F9',
          secondary: '#94A3B8',
          tertiary: '#64748B',
          disabled: '#475569',
        },
        semantic: {
          success: '#22C55E',
          warning: '#FBBF24',
          error: '#EF4444',
          info: '#3B82F6',
        },
        border: {
          DEFAULT: '#334155',
          subtle: 'rgba(148, 163, 184, 0.06)',
          accent: 'rgba(45, 212, 191, 0.35)',
        },
      },
      fontFamily: {
        display: ['DMSerifDisplay_400Regular'],
        body: ['SpaceMono_400Regular'],
        'body-bold': ['SpaceMono_700Bold'],
      },
      borderRadius: {
        sm: '4px',
        md: '12px',
        lg: '16px',
      },
      spacing: {
        // Extend only if the default 4px scale is insufficient
      },
    },
  },
};
```

### styled-components — Theme Structure

When the project uses styled-components/native:

```typescript
// theme structure — matches Color Token Architecture
export const darkTheme = {
  colors: {
    fill: {
      /* surfaces */
    },
    stroke: {
      /* borders */
    },
    textIcon: {
      /* text hierarchy */
    },
    semantic: {
      /* functional */
    },
    components: {
      /* component-specific overrides */
    },
  },
  typography: {
    /* full scale with fontFamily, fontSize, lineHeight */
  },
  spacing: { xs: 4, sm: 8, md: 16, lg: 24, xl: 32, xxl: 48 },
  borderRadius: { sm: 4, md: 12, lg: 16, full: 9999 },
};
```

---

## Industry-Specific Design Guidance

When the user describes their app's purpose, apply these additional guidelines:

### Streaming / Media Tracker

- Hero images dominate — show the content, not the UI
- Dark theme preferred (cinema feel)
- Status indicators (watching, completed, planned) via color dots or pills
- Rating system should be visually satisfying (stars, hearts, numbers with scale)
- Genre/category as pills or tags

### Fitness / Health

- Progress as the hero (rings, charts, streaks)
- Motivational — celebrate achievements
- High contrast for outdoor/gym use (bright screens)
- Large touch targets (sweaty fingers, gloves)
- Calming palette if wellness-focused; energetic if performance-focused

### Finance / Banking

- Numbers are the hero — strong typographic hierarchy for amounts
- Green/red ONLY for profit/loss — never as accent
- Security-implying design: dark, premium, understated
- Transaction lists: clear grouping by date, running balance
- Charts: clean, labeled, not decorative

### Social / Community

- Content-first — the UI should disappear behind user content
- Avatar-heavy — faces build connection
- Real-time indicators (online status, typing, new messages)
- Easy media sharing — camera/gallery access is primary
- Notification management built into the design

### E-commerce / Marketplace

- Product images are heroes — large, high-quality, zoomable
- Price hierarchy: current price large + bold, original price struck through
- Trust signals: ratings, reviews count, badges
- Cart/checkout must be frictionless — progress indicator, minimal fields
- Filter/sort easily accessible without leaving results

### Education / Learning

- Progress tracking is motivational — streaks, levels, XP
- Content readability above all — generous line height, contrast
- Interactive elements clearly distinguishable from content
- Gamification: badges, celebrations, social comparison (optional)

---

## Iconography & Visual Elements

### Icon Guidelines

| Guideline         | Detail                                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| Library           | Use `@expo/vector-icons` (includes Ionicons, MaterialIcons, Feather, FontAwesome) or `react-native-vector-icons` |
| Style consistency | Pick ONE icon family and stick with it. Mixing outlined and filled, or different weights, creates visual noise   |
| Size              | 20-24pt for inline/navigation. 32-48pt for feature/empty-state icons. 64+ for hero illustrations                 |
| Color             | Icons follow the same text hierarchy as typography (primary, secondary, tertiary)                                |
| Active/inactive   | Active: accent color or primary text. Inactive: tertiary/muted                                                   |
| Touch area        | Icon buttons need the full 44×44 tap target regardless of icon size — use padding or hitSlop                     |

### When to Use Illustrations vs Icons

- **Icons**: navigation, actions, status indicators, inline labels
- **Illustrations**: empty states, onboarding, error pages, feature explanations
- **Neither**: if text alone communicates clearly, don't add visual clutter

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
- ❌ Designing only the happy path — every screen must have loading/empty/error states planned
- ❌ Using more than 2 font families — confusion is not personality
- ❌ Ignoring contrast ratios — pretty is not useful if it's unreadable
- ❌ Flat color systems — always have depth through surface layers
- ❌ Putting destructive actions in easy-to-tap positions — always at the bottom, always with confirmation

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
- Include accessible touch targets, readable contrast ratios (WCAG AA minimum), and sensible support for font scaling unless the project already has a deliberate alternative
- Handle first-run realism: loading, empty, error, and offline-adjacent states when the screen obviously needs them
- Verify the result on at least one small-screen mental model (375pt width — iPhone SE/13 mini) and one large-screen mental model (428pt width — iPhone Pro Max) before considering the work done
- Provide the complete theme token file when creating a new theme — not fragments
- Include font loading instructions when introducing new fonts

### Quality Checklist (verify before delivering)

- [ ] All colors reference tokens (no hardcoded hex in components)
- [ ] Text contrast meets WCAG AA (4.5:1 body, 3:1 large)
- [ ] Touch targets ≥ 44pt
- [ ] Consistent spacing grid (all values are multiples of base unit)
- [ ] All states designed (loading, empty, error — not just happy path)
- [ ] Platform-specific code where iOS/Android differ
- [ ] No web-only CSS properties
- [ ] Safe areas respected
- [ ] Typography hierarchy visible (at least 3 distinct levels on screen)
- [ ] One memorable design decision per screen


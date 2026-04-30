# react-native-design

> A comprehensive VS Code / GitHub Copilot skill that replaces the need for a UX/UI designer when building React Native apps. From color palette generation to screen architecture, this skill handles every design decision with professional-grade output.

## What is this?

A skill file for GitHub Copilot (VS Code agent mode) that provides deep expertise in React Native UI design. Instead of getting generic, bland AI-generated designs, this skill enforces intentional, platform-correct, production-ready UI with strong visual direction.

**It acts as a complete UX/UI replacement** — capable of generating color systems from scratch, designing screens, critiquing and improving existing UI, and making every design decision a professional designer would make.

## Key Capabilities

### 🎨 Theme & Color System

- **Create themes from scratch** — give it an app concept, get a complete token system
- **Generate palettes from user colors** — provide 1-3 colors, get a full professional palette
- **Improve existing palettes** — audits contrast, semantic separation, and surface depth
- **Dual theme support** — dark + light with shared token architecture
- **Color theory built-in** — monochromatic, analogous, complementary, split-complementary, triadic schemes
- **WCAG accessibility** — enforces minimum contrast ratios (4.5:1 body, 3:1 large text)

### 📐 Screen Design & Layout

- **Complete screen pattern catalog** — Auth, Home, Detail, List, Profile, Search, Onboarding, Modals
- **Layout decision framework** — decision tree for choosing structure based on content type
- **Component sizing reference** — minimum heights, recommended dimensions, touch targets
- **Navigation architecture** — when to use tabs, drawers, stacks, or combinations

### 🛠 Stack Agnostic

Works with whatever styling approach your project uses:

- `StyleSheet.create` (vanilla RN)
- **NativeWind / Tailwind** (full config generation included)
- **styled-components/native** (ThemeProvider patterns)
- **@shopify/restyle** (type-safe tokens)

Detects your stack automatically and generates code that matches your conventions.

### 📱 Platform Correctness

Hard rules for React Native constraints:

- Flexbox-only layout (no CSS Grid, no `display: block`)
- Gradient handling via `<LinearGradient>` (not CSS)
- Cross-platform shadows (iOS `shadow*` + Android `elevation`)
- Network images require explicit dimensions
- Safe areas, keyboard avoidance, touch targets

### 🏭 Industry-Specific Guidance

Tailored design patterns for:

- Streaming / Media trackers
- Fitness / Health
- Finance / Banking
- Social / Community
- E-commerce / Marketplace
- Education / Learning

### 🔄 State-First Design

Every screen must handle ALL states — not just the happy path:

- Loading (skeleton shimmer, not spinners)
- Empty (illustration + CTA, not "No items")
- Error (friendly + retry, not stack traces)
- Offline (banner + cached content)
- Success (transient feedback)

## Example Prompts

```
"I'm building a fitness tracking app. Create a dark theme using teal as the accent color with Tailwind/NativeWind."

"Improve my current color palette — the surfaces feel flat and the accent doesn't pop enough."

"Design a detail view for a movie tracker app that shows poster, rating, and episode progress."

"Create a complete theme from these brand colors: #2DD4BF and #0C1219"

"I need a login screen that feels premium and cinematic. Dark theme, styled-components."

"Audit my current UI and tell me what's wrong with the color contrast and hierarchy."

"Design the home screen for a recipe app — light theme, playful aesthetic, lots of food imagery."
```

## What's Included

| Section                   | Description                                                                             |
| ------------------------- | --------------------------------------------------------------------------------------- |
| Core Philosophy           | 5 principles that guide every decision                                                  |
| Design Thinking Framework | Purpose, audience, aesthetic direction, differentiation                                 |
| Color System (Complete)   | Theory, generation process, token architecture, dark/light rules                        |
| Theme Creation Workflows  | 4 step-by-step workflows (scratch, improve, from colors, specific view)                 |
| Screen Pattern Catalog    | 8+ screen types with structure, decisions, and quality marks                            |
| Layout Decision Framework | Content → layout decision tree, spacing rules, component sizes                          |
| Platform Constraints      | All RN hard rules (flexbox, gradients, shadows, images, touch, blur, lists, typography) |
| Visual Aesthetics         | Reference apps by category, dark/light principles, typography, spacing, motion          |
| State Design              | Required states, skeleton rules, empty state patterns                                   |
| Styling (Stack Agnostic)  | Detection + patterns for StyleSheet, NativeWind, styled-components, Restyle             |
| Industry Guidance         | Domain-specific design patterns for 6 industries                                        |
| Iconography               | Icon guidelines, library recommendations, sizing rules                                  |
| Quality Checklist         | 10-point verification before delivering any design                                      |

## Installation

### Option A: Copy into your project

Place the `SKILL.md` file in your project:

```
your-project/
└── .agents/
    └── skills/
        └── react-native-design/
            └── SKILL.md
```

### Option B: Reference from VS Code settings

Add the skill path to your Copilot agent configuration so it's available across projects.

## Requirements

- VS Code with GitHub Copilot (agent mode / chat)
- A React Native project (Expo or bare)
- Any styling stack (the skill adapts automatically)

## Anti-Patterns This Skill Prevents

- ❌ Generic purple-gradient-on-white "AI slop" aesthetics
- ❌ Flat color systems with no surface depth
- ❌ Web CSS assumptions that break on mobile (`linear-gradient`, `display: grid`, `px` units)
- ❌ Designing only the happy path (no loading/empty/error states)
- ❌ Using Inter/Roboto as brand fonts (acceptable for body, not for identity)
- ❌ Hardcoded colors instead of token references
- ❌ Ignoring contrast ratios and accessibility
- ❌ Touch targets smaller than 44pt
- ❌ Network images without explicit dimensions
- ❌ Mixing styling approaches within the same component

## Quality Standards

Every output from this skill is verified against:

- [ ] All colors reference tokens (no hardcoded hex in components)
- [ ] Text contrast meets WCAG AA (4.5:1 body, 3:1 large)
- [ ] Touch targets ≥ 44pt
- [ ] Consistent spacing grid (multiples of base unit)
- [ ] All states designed (loading, empty, error)
- [ ] Platform-specific code where iOS/Android differ
- [ ] No web-only CSS properties
- [ ] Safe areas respected
- [ ] Typography hierarchy visible (≥3 distinct levels)
- [ ] One memorable design decision per screen

## License

MIT


## Repository Note

The main behavior of this skill lives in [`SKILL.md`](./SKILL.md). This README is the human-friendly explanation: what the skill is for, when to use it, and why it improves the quality of React Native UI work.

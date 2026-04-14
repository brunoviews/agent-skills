# React Native Design Skill

Build, redesign, critique, refactor, or debug React Native UI with stronger visual direction and better platform correctness.

This skill is designed for AI coding agents working on mobile interfaces. It helps them produce React Native screens and components that feel intentional, polished, and native to the platform instead of looking like generic web layouts copied into mobile code.

## What This Skill Does

The `react-native-design` skill teaches an agent how to approach React Native UI work with both design judgment and technical accuracy.

It helps the agent:

- Create new mobile screens from scratch
- Redesign existing screens with a clearer visual identity
- Critique weak UI and explain what is not working
- Refactor messy or inconsistent screen implementations
- Debug platform-specific visual issues on iOS and Android
- Translate design ideas or Figma-like direction into working React Native code
- Adapt its output to the styling stack already used by the project

In short: it pushes the agent to make better design decisions and safer React Native implementation decisions at the same time.

## Why This Skill Exists

General-purpose coding agents can usually write React Native code that compiles, but they often miss the details that make mobile UI feel good.

Without guidance, agents tend to:

- Produce generic, forgettable layouts
- Reuse common AI-generated patterns with little personality
- Apply web assumptions that do not map cleanly to React Native
- Ignore platform differences such as shadows, safe areas, or image sizing
- Make small cosmetic tweaks instead of meaningful design improvements
- Introduce a new styling pattern instead of following the project's existing one

This skill exists to prevent that.

It gives the agent a mobile-first design process, a React Native-specific implementation baseline, and a stronger quality bar for visual work.

## With This Skill vs Without It

| Without the skill | With the skill |
| --- | --- |
| The agent may make the UI "clean" but generic | The agent commits to a clear visual direction |
| Web habits may leak into React Native decisions | React Native constraints are treated as first-class rules |
| Design changes may be shallow or token-only | Layout, typography, hierarchy, and interaction are improved intentionally |
| Existing design systems may be ignored or stretched carelessly | The agent inspects the current system first and works within it unless change is justified |
| Styling choices may be inconsistent across files | The agent follows the project's established styling stack |
| UI may technically work but still feel flat | UI is expected to feel more polished, more native, and more memorable |

## Why You Should Use It

Use this skill if you want your AI agent to behave more like a thoughtful mobile product designer and a senior React Native engineer at the same time.

It is especially useful when:

- Visual quality matters, not just functional correctness
- You want stronger screens, not just prettier padding values
- You need the agent to respect mobile platform constraints
- You want design decisions that feel deliberate, not random
- You are working in an existing codebase and want the agent to follow the current stack
- You want fewer "looks fine in code, feels wrong on device" outcomes

## Best Use Cases

This skill is a strong fit for:

- Building a new screen from a product idea
- Improving an existing screen that feels bland or inconsistent
- Auditing a React Native UI and identifying weaknesses
- Fixing styling bugs that appear only on mobile platforms
- Porting a web concept into a proper mobile layout
- Translating a mockup into production-ready React Native code
- Improving empty, loading, or error states
- Working on dark mode, visual hierarchy, spacing systems, or card surfaces

## When Not to Use It

This skill is not meant for every task in a React Native project.

You probably do not need it when the task is mainly about:

- API integration
- State management
- Database work
- Business logic
- Navigation wiring with no UI change
- Simple text or copy edits
- Small fixes that do not involve design, layout, or interaction quality

If the task is mostly backend or app logic, a general coding workflow is usually enough.

## What Makes This Skill Different

This skill does more than list React Native styling tips. It gives the agent a working decision framework.

It teaches the agent to:

- Define a clear design direction before changing code
- Inspect the existing theme, tokens, and layout patterns first
- Decide whether the current design system is enough for meaningful improvement
- Ask before silently extending the design system
- Respect hard React Native platform constraints
- Follow the existing styling stack instead of introducing a new one
- Design for real app states, not just the ideal loaded state
- Avoid "AI slop" aesthetics and default-looking mobile UI

That combination is what makes it practical.

## React Native Areas It Covers

The skill includes guidance for:

- Flexbox and mobile layout behavior
- Safe areas and screen structure
- Gradients in React Native
- Cross-platform shadows
- Image rendering rules
- Typography and font usage
- Dark and light theme design
- Touch targets and interaction feedback
- Blur and visual depth
- Scroll and list patterns
- Platform differences between iOS and Android
- Styling approaches such as `StyleSheet`, NativeWind, styled-components, and Restyle

## How to Use It

Use this skill when asking an AI agent to work on mobile UI.

The most effective requests usually include:

- The screen or component you want to change
- The purpose of that screen
- Whether you want a safe improvement or a bolder redesign
- Your styling stack, if it is not obvious from the repo
- Any platform concerns such as iOS-only or Android-specific issues
- Screenshots, mockups, or references if you have them

### Good Prompt Examples

```txt
Use react-native-design to redesign this home screen using the existing styled-components theme. I want it to feel more premium, but do not introduce a new design system.
```

```txt
Use react-native-design to audit this screen and tell me what is visually weak, what is technically risky in React Native, and then fix it.
```

```txt
Use react-native-design to build a watchlist screen for Expo + React Native. Keep it dark, cinematic, and platform-correct on both iOS and Android.
```

```txt
Use react-native-design to translate this Figma-inspired card layout into React Native using the project's current styling approach.
```

## What You Can Expect From the Output

When used well, this skill should push the agent toward output that is:

- More visually intentional
- More mobile-native
- More faithful to the current codebase
- More careful about iOS and Android behavior
- More complete in handling empty, loading, and edge states
- More accessible in touch targets and readability
- More production-ready and less placeholder-heavy

## Core Principles Behind the Skill

The skill is built around a few clear ideas:

1. Design should be intentional.
2. Mobile UI should respect platform constraints.
3. Existing project conventions matter.
4. Visual change should be noticeable, not cosmetic noise.
5. Strong implementation matters as much as strong aesthetics.

## If You Only Remember One Thing

This skill helps an AI agent stop treating React Native UI like generic frontend work.

It makes the agent think in terms of mobile design, platform reality, and production-quality implementation before it writes code.

## Repository Note

The main behavior of this skill lives in [`SKILL.md`](./SKILL.md). This README is the human-friendly explanation: what the skill is for, when to use it, and why it improves the quality of React Native UI work.

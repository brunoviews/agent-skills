# react-native-design

An agent skill for designing React Native and Expo UI. It makes your coding agent read the project before designing, commit to a visual direction that fits this particular app, build it on design tokens, measure contrast instead of guessing it, and follow the platform rules that web habits get wrong.

It works with any agent that reads `SKILL.md` folders (Claude Code, Cursor, Codex, GitHub Copilot in VS Code and others) and with any styling stack: `StyleSheet`, NativeWind, styled-components or Restyle.

## What it does

- **Themes from scratch.** From an app idea to a complete token system: surfaces, accent, text, borders, semantic colors, type scale, spacing and fonts, with the font loading code.
- **Palettes from your colors.** Give it one to three brand colors and it derives every missing role, for dark, light or both.
- **Audits.** It reads your existing theme and screens, picks the three issues with the most visual impact, and proposes before → after values with their measured contrast.
- **Screens.** It designs a specific view with your existing tokens and components, including the loading, empty, error and offline states.
- **Platform rules.** Checked against React Native 0.82+ and Expo SDK 54+: `boxShadow`, edge-to-edge on Android 16, safe areas, blur on Android, Liquid Glass on iOS 26, keyboard handling, lists and custom fonts.

It respects what's already there: it detects your stack and conventions, and asks before extending your design system.

## Contrast is measured, not guessed

A contrast ratio can't be judged by eye, and a model's estimate is often off. The skill ships a small script that the agent runs on every text/surface pair it adds or changes:

```
$ node scripts/contrast.mjs "rgba(255,255,255,0.38)" "#1C1C1E"
rgba(255,255,255,0.38) on #1C1C1E  (seen as #727274)
3.54:1
  body text   FAIL (needs 4.5:1)
  large text  PASS (needs 3:1)
  UI / icons  PASS (needs 3:1)
```

Semi-transparent colors are blended over their background first, so the ratio is the one users actually see. It accepts hex, `rgb()`, `rgba()`, `hsl()` and `hsla()`, and needs nothing but Node.

## How it's built

`SKILL.md` is the procedure the agent follows: read the project, pick a direction, run the matching workflow, check the result. The detail lives in reference files that the agent loads only when the task needs them, so a small UI fix doesn't pull every guideline into the context.

```
react-native-design/
├── SKILL.md                      the procedure, hard rules and final checklist
├── references/
│   ├── color-system.md           contrast rules, palette generation, token roles, dark and light rules
│   ├── platform-constraints.md   React Native rules that break rendering, with their fixes
│   ├── styling-stacks.md         StyleSheet, NativeWind, styled-components and Restyle patterns
│   ├── screen-patterns.md        auth, home, detail, list, profile, search, onboarding, sheets
│   └── aesthetics.md             aesthetic directions, fonts, motion, icons, domain patterns
└── scripts/
    ├── contrast.mjs              WCAG contrast checker
    └── contrast.test.mjs         its tests: node --test scripts/contrast.test.mjs
```

## Installation

With the [skills CLI](https://github.com/vercel-labs/skills), which detects your agents and installs into the right folder:

```bash
npx skills add brunoviews/agent-skills --skill react-native-design
```

Or copy the whole `react-native-design/` folder by hand. Copy the folder, not only `SKILL.md`: the skill uses `references/` and `scripts/`.

| Agent                    | In the project                          | For all your projects |
| ------------------------ | --------------------------------------- | --------------------- |
| Claude Code              | `.claude/skills/`                       | `~/.claude/skills/`   |
| Cursor                   | `.agents/skills/` or `.cursor/skills/`  | `~/.cursor/skills/`   |
| Codex                    | `.agents/skills/`                       | `~/.agents/skills/`   |
| GitHub Copilot (VS Code) | `.github/skills/` or `.agents/skills/`  | `~/.copilot/skills/`  |

## Example prompts

```
"I'm building a fitness tracking app. Create a dark theme using teal as the accent color with NativeWind."

"Improve my current color palette — the surfaces feel flat and the accent doesn't pop enough."

"Design a detail view for a movie tracker app that shows poster, rating, and episode progress."

"Create a complete theme from these brand colors: #2DD4BF and #0C1219"

"I need a login screen that feels premium and cinematic. Dark theme, styled-components."

"Audit my current UI and tell me what's wrong with the color contrast and hierarchy."

"Design the home screen for a recipe app — light theme, playful aesthetic, lots of food imagery."
```

## Requirements

- A React Native project, Expo or bare. The rules target React Native 0.82+; on older projects the skill checks the version before using newer style props.
- Node.js, for the contrast script.

## What it doesn't do

- It doesn't replace a designer, user research, or testing on real devices. It raises the floor of what your agent produces and catches the mistakes that can be measured.
- It stays out of data fetching, state management, navigation wiring and non-visual bugs on purpose.

## Acknowledgements

The "commit to a direction" step is inspired by Anthropic's [frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) skill (Apache-2.0).

## License

[MIT](./LICENSE)

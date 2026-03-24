# Design System — Toma Design Language

This is the shared design foundation for all apps built in the Toma studio.

**Philosophy:** Minimal luxury. Intentional craft. Every pixel earns its place.
**Reference points:** Linear, Raycast, Arc, iOS 26, Notion, Claude's web product.
**Aesthetic:** Clean surfaces, glass morphism, precise hierarchy, fluid motion.

---

## Structure

```
design/
├── 00-READ-ME.md           ← You are here
├── principles.md            ← Core design values & what NOT to do
├── foundation/
│   ├── colors.css          ← Color tokens: light, dark, semantic
│   ├── typography.css      ← Font scale, weights, line heights
│   ├── spacing.css         ← 4px grid system
│   └── motion.css          ← Animation curves, durations, tokens
├── components/
│   ├── button.css          ← All button variants + states
│   ├── input.css           ← Text inputs, selects, checkboxes
│   ├── card.css            ← Surface cards, glass cards
│   ├── modal.css           ← Overlays, sheets, dialogs
│   └── navigation.css      ← Navbars, tabs, sidebars
└── systems/
    ├── glass.md            ← Glass morphism: when + how to use
    ├── ios-26.md           ← iOS 26 SwiftUI design notes
    └── dark-mode.md        ← Dark mode rules
```

## Design Principles

1. **Clarity over cleverness** — Users should never have to think about the UI.
2. **One primary action per view** — If everything is important, nothing is.
3. **Glass sparingly** — backdrop-filter is powerful. Don't abuse it.
4. **Motion with purpose** — Every animation communicates state, not decoration.
5. **No color for color's sake** — Color is functional or it is noise.
6. **Dark mode first** — Most of our users are power users. They live in dark mode.

---

## For Sub-Agents

When building an app, read these files in order before writing a single line of CSS:
1. Read `principles.md`
2. Read `foundation/colors.css` + `foundation/typography.css`
3. Read `foundation/motion.css`
4. Read `components/button.css` as a template for all components
5. Apply glass.md rules

Each app gets its own folder: `/Dev/<app-name>/`

---

Last updated: 2026-03-24

# Design Principles

## The Look

**Reference:** Linear, Raycast, Arc Browser, iOS 26, Notion, Claude web product.

Think of the design language used by the best software companies in the world right now. Clean. Intentional. Every pixel has a reason. Nothing decorative that doesn't earn its place.

**Aesthetic pillars:**
- **Clarity** — hierarchy is obvious, the eye knows where to go
- **Restraint** — whitespace is a design element, not emptiness
- **Craft** — animations are fluid, interactions are polished, nothing feels cheap
- **Sophistication** — premium feel without being cold or corporate

---

## What Good Looks Like

### Color
- Backgrounds: near-black (#0d0d0e) or clean white (#ffffff) — not mid-gray
- Text: high contrast. Body text never below #6b7280 on light, #9ca3af on dark
- Accent: one primary accent color per app. Use it sparingly — only for CTAs, active states, key UI
- Semantic: error = red, success = green, warning = amber. Consistent across all apps
- No clashing multiple accent colors

### Typography
- Sans-serif primary: Inter (web) or SF Pro (iOS)
- Font weights: 400 (body), 500 (UI labels), 600 (subheadings), 700 (headings)
- Font scale: 12, 13, 14, 16, 18, 20, 24, 30, 36, 48px
- Line heights: 1.2 for headings, 1.5 for body, 1.0 for UI labels
- Letter spacing: -0.02em for large headings, 0 for body, 0.02em for uppercase labels

### Spacing
- 4px base unit: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64px
- Padding inside cards: 16-24px
- Section spacing: 48-96px
- Never use arbitrary pixel values — use the 4px grid

### Motion
- Page transitions: slide from right (or bottom on mobile), 300ms ease-out
- Hover states: 150-200ms
- Modal/sheet open: scale from 0.96 + fade, 250ms spring
- Micro-interactions: 100-150ms
- Never use `linear` easing — always use `ease-out`, `ease-in-out`, or spring curves

### Glass & Surfaces
- Cards: `background: rgba(255,255,255,0.04)` on dark, `rgba(0,0,0,0.03)` on light
- Borders: `1px solid rgba(255,255,255,0.08)` on dark, `rgba(0,0,0,0.08)` on light
- Shadows: subtle — `0 4px 16px rgba(0,0,0,0.15)` max. No harsh drop shadows.
- `backdrop-filter: blur(12px) saturate(180%)` — use for modals, sticky headers, glass panels
- Border radius: 8px for cards, 6px for buttons, 4px for inputs, 12px for modals

---

## What NOT to Do

- ❌ No gradients on large background areas
- ❌ No blue as a default primary color (unless the app is specifically about blue)
- ❌ No `border-radius: 50%` except for avatars
- ❌ No heavy drop shadows — light and subtle only
- ❌ No `linear` easing on any animation
- ❌ No hover states that change more than 2 properties at once
- ❌ No "flat" design — surfaces should have depth (glass, border, shadow — pick one)
- ❌ No full-width inputs or buttons unless specifically a mobile list
- ❌ No color animations — never animate a color from one value to another
- ❌ No scroll-jacking or unusual scroll behavior unless it's a specific interactive feature

---

## Interaction Rules

### Hover
- Elements lift: `transform: translateY(-1px)` + shadow increase, 150ms
- Buttons: background lightens/darkens 8-10%, cursor pointer
- Cards: subtle border brightening

### Click/Tap
- Press: scale(0.98) for 100ms — instant feedback
- Release: scale returns to 1, action fires

### Focus
- Always visible focus ring: `outline: 2px solid accent; outline-offset: 2px`
- Never remove focus styles — keyboard users need them

### Scroll
- Smooth scrolling for anchor links
- No sticky-scroll sections on mobile
- On mobile: momentum scrolling enabled (`-webkit-overflow-scrolling: touch`)

---

## Dark Mode Rules
- Always offer dark mode (and default to dark for power-user apps)
- All colors defined as CSS custom properties with both light and dark values
- Test every view in both modes before considering a feature "done"
- Never use pure black (#000000) in dark mode — use #0d0d0e or similar
- Never use pure white (#ffffff) in dark mode text — use #f5f5f7 or similar

---

## iOS Native Notes
- Use SF Pro as font
- Respect safe areas on all screens
- Use iOS system colors for semantic meaning
- Tab bar: blur glass background
- Navigation bar: large title style where appropriate
- Bottom sheets: use UISheetPresentationController with detents
- Haptic feedback: use on selection change, toggle, drag release

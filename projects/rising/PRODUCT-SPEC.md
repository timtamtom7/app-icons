# Rising — Product Spec

## Concept

Rising is a savings goal tracker that makes saving feel like progress. You set a goal — a number, a purpose, a photo of what you're saving for — and every deposit updates the visual. The photo doesn't just sit there — it gradually reveals itself as you get closer, like a scratch card in reverse. The closer you get, the more real the goal becomes. Watching your money and your motivation grow together is the product.

**Core mechanic:** Set goal → attach photo → deposit money → photo reveals → goal reached → celebrate.

---

## Brand Identity

**Name:** Rising  
**Tagline:** "Watch your goal come into view."  
**Vibe:** Warm, visual, satisfying. The feeling of progress. Like watching the sun rise — slow, inevitable, beautiful. Not a finance app — a motivation app that happens to involve money.

**Aesthetic direction:** Clean surfaces with generous photography. The goal photo is the hero — everything else frames it. Think Headspace meets a high-end fashion editorial. Light-forward (though dark mode works perfectly too). Generous whitespace. The UI recedes; the goal image dominates.

**Reference:** Headspace's warmth + Linear's precision + Instagram's photo-first aesthetic.

**Colors:**
- Background light: `#fafaf9`
- Background dark: `#0f0f0e`
- Surface light: `#ffffff`
- Surface dark: `#1a1a19`
- Border: `rgba(0,0,0,0.06)` (light) / `rgba(255,255,255,0.07)` (dark)
- Text primary: `#18181b`
- Text secondary: `#71717a`
- Accent: `#22c55e` (fresh green — growth, progress, money)
- Accent warm: `#f59e0b` (amber — warmth, gold, reward)

**Typography:**
- Headings: `Inter 600`
- Body: `Inter 400`
- Numbers (amounts): `JetBrains Mono` — financial figures should look precise

---

## Features

### 1. Create a Goal
- **Goal name:** "Trip to Japan" / "New MacBook Pro" / "Emergency Fund"
- **Target amount:** $5,000 (numeric input, formatted as currency)
- **Deadline (optional):** Target date to reach the goal
- **Photo:** Upload a photo of the goal (or choose from Unsplash). This is the emotional anchor.
- **Starting amount:** If you've already saved something toward this goal

### 2. Goal Card (Home)
- Large photo of goal, overlaid with:
  - Goal name
  - Progress bar (filled %)
  - Current amount / Target amount
  - Days remaining (if deadline set)
- Progress bar has a subtle animation when it increases
- The photo gradually reveals more as progress increases (via CSS clip-path or gradient mask)

### 3. Deposit
- Tap goal card → Deposit screen
- Input amount: numeric keyboard, auto-formats as currency
- "What's this deposit for?" (optional): "Paycheck", "Freelance", "Birthday money", "Other"
- Confirm → transaction recorded → progress bar animates up → photo reveals more

### 4. Photo Reveal Mechanic
- At 0%: photo fully masked with a dark overlay
- As % increases: overlay reduces, photo reveals from bottom up
- At 100%: full photo visible, celebration state triggers

**Implementation:** CSS `clip-path: inset(X% 0 0 0)` where X decreases as progress increases.

### 5. Goal History
- Every deposit logged with date, amount, note
- Running total visible
- Milestone markers on progress bar: 25%, 50%, 75%

### 6. Goal States
- **In progress:** Normal state, depositing
- **Completed:** Goal reached → confetti animation (subtle, not overwhelming), goal card shows "🎉 Reached!" badge, option to archive or continue adding
- **Archived:** User can archive a goal (keeps history)
- **Abandoned:** User can mark a goal as abandoned (records it, removes from active view)

### 7. Multiple Goals
- Home shows all active goals as a grid of cards
- Total saved across all goals shown at top
- Can reorder goals (drag to prioritize)

### 8. Optional: Contributions View
- Shows all deposits across all goals in a single feed
- Grouped by month
- Total saved this month / this year

---

## Non-Features
- No bank connections (Plaid, etc.) — manual entry only
- No investment features
- No budgets or spending tracking
- No social sharing of goals
- No AI suggestions

---

## Tech Stack

**Web:** React + Vite, CSS (shared design system), localStorage  
**Images:** Unsplash API (for curated photos) or user upload (stored as base64 in localStorage for MVP)  
**Later:** Supabase for sync across devices  

**localStorage schema:**
```javascript
{
  goals: [
    {
      id: uuid,
      name: string,
      targetAmount: number,
      currentAmount: number,
      deadline: ISO date string | null,
      photoUrl: string (Unsplash URL or base64),
      createdAt: ISO date string,
      completedAt: ISO date string | null,
      archived: boolean,
      deposits: [
        { id: uuid, amount: number, note: string, createdAt: ISO date string }
      ]
    }
  ],
  settings: {
    currency: string, // 'USD', 'EUR', etc.
    theme: 'light' | 'dark'
  }
}
```

---

## Pages

1. **`/`** — Landing. "Watch your goal come into view." Show the photo-reveal mechanic in an animated demo. CTA: Start Saving.
2. **`/app`** — Home: goals grid, total saved counter, "Add Goal" button.
3. **`/app/goals/new`** — Create goal: name, amount, deadline, photo.
4. **`/app/goals/:id`** — Goal detail: large photo, progress, deposit button, deposit history.
5. **`/app/goals/:id/deposit`** — Deposit flow.
6. **`/app/goals/:id/edit`** — Edit goal details.
7. **`/app/history`** — All deposits across all goals, grouped by month.
8. **`/app/settings`** — Currency, theme, export data.

---

## Design Direction (from shared system)

- **Photo first:** Goal image takes up 50-60% of the goal card. The UI is minimal framing.
- **Surfaces:** Light mode default, `--surface-1: #fafaf9`
- **Accent:** `--color-accent: #22c55e` (green)
- **Progress bar:** Accent green, rounded, animated fill
- **Typography:** Inter throughout. JetBrains Mono for currency amounts.
- **Motion:** 
  - Progress bar: smooth width transition, 400ms ease-out
  - Photo reveal: clip-path animation, 600ms ease-out on deposit
  - Card entrance: staggered fade+translate, 250ms
  - Celebration: subtle particle/confetti burst on 100% (CSS only, 2 seconds)
- **Cards:** White (`#ffffff`) with very subtle shadow (`0 2px 8px rgba(0,0,0,0.06)`). No border.
- **Dark mode:** `--surface-1: #0f0f0e`, photo remains prominent
- **Mobile:** Goal card becomes full-width hero image with overlay

---

## Build Roadmap

### Phase 1 — Core MVP
1. Landing page with animated demo
2. Create goal (name, amount, deadline, photo)
3. Goal card with photo reveal
4. Deposit flow
5. Progress bar animation
6. Goal history (deposit log)
7. Goal completion celebration
8. Dark/light mode

### Phase 2 — Multiple Goals + Polish
9. Multiple goals grid
10. Total saved counter
11. Reorder goals
12. Edit/archive goals
13. 25/50/75% milestone markers

### Phase 3 — History + Export
14. Contributions history view
15. Export data as JSON

### Phase 4 — Cloud Sync
16. Supabase auth + sync
17. Cross-device sync

---

## Human Inputs Needed (To-Do for Tommaso)
- [ ] Unsplash API key (or we use free/placeholder images)
- [ ] Custom domain (rising.app?)
- [ ] Logo concept (wordmark + icon)
- [ ] Confetti animation — CSS or Lottie file
- [ ] Supabase project (if going cloud)

# Bunker — Product Spec

## Concept

Bunker is a decision-making app for when the stakes are real. You have a decision to make — something that's been sitting in your head, something you keep pushing off, something where the wrong choice has real consequences. Bunker walks you through articulating it properly before you decide. Not pros-and-cons lists. Not a decision tree. Just structured, honest thinking in a space that feels calm and contained — like you're in a bunker, safe from distraction and urgency, thinking clearly.

**Core mechanic:** Enter decision → articulate the problem, options, tradeoffs, timeline, and what you'd regret → set a decision deadline → Bunker checks in when it's time.

---

## Brand Identity

**Name:** Bunker  
**Tagline:** "Think it through."  
**Vibe:** Deliberate, calm, serious without being cold. A place where important things are taken seriously. Feels like a well-designed private study — everything stripped away except the thinking.

**Aesthetic direction:** Think Linear meets a war room. Dark surfaces, precise typography, structured layouts. Every element earns its place. No decoration. The structure itself communicates care.

**Reference:** Linear's dark UI + Bear App's sense of containment + a blank moleskine page.

**Colors:**
- Background: `#0d0d0e` (deep dark — bunker walls)
- Surface: `#141416`
- Border: `rgba(255,255,255,0.08)`
- Text primary: `#f5f5f7`
- Text secondary: `#8b8b8e`
- Accent: `#5b8def` (calm blue — clear thinking, not anxiety)
- Error: `#e5484d` (only for critical warnings)

**Typography:**
- Headings: `Inter 600`, tight tracking
- Body: `Inter 400`, generous line-height
- Monospace labels: `JetBrains Mono` — for field labels, timestamps

---

## Features

### 1. Decision Entry
- Single focused input: "What decision are you facing?"
- On submit → opens the Decision Workspace
- Can mark as: **Active** (thinking in progress) or **Decided** (already decided, record the outcome)

### 2. Decision Workspace
A structured document for one decision. Sections:

**A. The Decision** — One sentence. The actual decision, stated plainly. Not "what to do about work" — "Accept the job offer at Stripe or stay at the startup."

**B. The Problem** — What's driving this? Why does this decision matter? What happens if you don't make it? (2-4 sentences)

**C. Options** — At least 2 options, max 4.
- Name each option
- For each: "What does this look like in 6 months? 2 years?"
- For each: "What's the worst thing about this?"
- For each: "What's the best-case scenario?"

**D. Tradeoffs** — What are you giving up with each option? (e.g. "Option A: more money but longer commute. Option B: more time but less growth.")

**E. Timeline** — "When do you need to decide by?" — date picker. If no deadline, set one anyway — creates urgency.

**F. Regret Check** — "Which would you regret more: doing X and it going wrong, or NOT doing X and never knowing?"
- This surfaces what actually matters.

**G. Decision Made** — Once decided, record:
  - Which option you chose
  - Why (2 sentences)
  - "Would I make the same decision again?" (Yes/No + why)

### 3. Decision Feed (Home)
- List of all decisions
- Active decisions: highlighted, shows deadline countdown
- Decided decisions: collapsed, shows outcome
- Filter: All / Active / Decided
- Search by decision title

### 4. Check-In System
- On the decision deadline date, Bunker sends a notification (or shows on next open): "You set a deadline for this decision. What's the call?"
- If still undecided: option to extend deadline or mark as decided
- If decided: prompts to record the outcome

### 5. Reflection (Quiet Feature)
- 30 days after a decision is marked "decided," Bunker asks: "How's that decision working out?"
- User can update: Still good / Had doubts / Changed course
- Data stays private, used only for user's own reflection

### 6. Privacy First
- All data encrypted at rest
- No accounts required initially (local storage) — optional Supabase sync later
- No analytics, no tracking

---

## Non-Features
- No AI suggesting what to decide
- No sharing decisions with others
- No collaborative features
- No calendar integration (yet)

---

## Tech Stack

**Web:** React + Vite, CSS (shared design system), localStorage first  
**Optional backend (later):** Supabase for sync + auth  
**No external APIs needed for MVP** — fully local, fully private  

**localStorage schema:**
```javascript
{
  decisions: [
    {
      id: uuid,
      title: string,
      status: 'active' | 'decided',
      problem: string,
      options: [
        { name: string, sixMonths: string, worst: string, best: string }
      ],
      tradeoffs: string,
      deadline: ISO date string | null,
      regretAnswer: string | null,
      decidedAt: ISO date string | null,
      chosenOption: number | null,
      decisionWhy: string | null,
      wouldChooseAgain: boolean | null,
      reflectionAt30Days: { status: string, notes: string } | null,
      createdAt: ISO date string,
      updatedAt: ISO date string
    }
  ],
  settings: {
    theme: 'dark' | 'light',
    checkInEnabled: boolean
  }
}
```

---

## Pages

1. **`/`** — Landing. "The decisions worth making deserve a安静 place to think." CTA: Open App
2. **`/app`** — Decision feed. List of all decisions.
3. **`/app/decisions/new`** — New decision workspace.
4. **`/app/decisions/:id`** — View/edit existing decision workspace.
5. **`/app/decisions/:id/decided`** — Record outcome (after choosing).
6. **`/app/settings`** — Theme, check-in settings, export data.

---

## Design Direction (from shared system)

- **Surfaces:** `--surface-1: #0d0d0e`, `--surface-2: #141416`, `--surface-3: #1c1c1e`
- **Accent:** `--color-accent: #5b8def`
- **Borders:** `1px solid rgba(255,255,255,0.08)` — very subtle
- **Cards:** Glass-light: `background: rgba(255,255,255,0.03)`, subtle border
- **Typography:** Inter throughout. JetBrains Mono for field labels (`font-family: var(--font-mono)`)
- **Section spacing:** generous — `48px` between major sections in workspace
- **Motion:** Slow and deliberate. Entrance: 300ms ease-out. No bounce, no spring. Feels considered.
- **Dark mode first:** Default to dark, light mode must also work perfectly
- **No decoration:** The structure IS the design. No background patterns, no gradients.

---

## Build Roadmap

### Phase 1 — MVP (Local Only)
1. Landing page
2. Decision feed (list view)
3. Decision workspace (all fields)
4. Mark as decided + record outcome
5. Decision timeline (deadline countdown)
6. Edit/delete decisions
7. Dark/light mode

### Phase 2 — Check-Ins + Persistence
8. localStorage persistence
9. Deadline check-in notifications (in-app + browser notification)
10. 30-day reflection prompt

### Phase 3 — Polish
11. Search decisions
12. Export decisions as JSON
13. Keyboard shortcuts
14. Mobile responsive (already designed mobile-first)

### Phase 4 — Optional Cloud Sync
15. Supabase auth + sync
16. Cross-device sync

---

## Human Inputs Needed (To-Do for Tommaso)
- [ ] Logo concept / wordmark
- [ ] Custom domain (bunker.app?)
- [ ] Supabase project (if going cloud)
- [ ] Browser notification permission flow design
- [ ] App Store developer account (if iOS)

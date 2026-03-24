# Ghost Notes — Product Spec

## Concept

Ghost Notes is a read-later app with a difference. It's not a new place to put things — it's a reckoning with the things you've already put off. Save articles with one tap (bookmarklet, share sheet, browser extension). Once a week, Ghost Notes surfaces what's accumulated: "12 articles. Here's what's still here." Clean, honest, a little uncomfortable — because the ghost in the name is the version of you that keeps meaning to read things but doesn't.

**Core mechanic:** One-tap save → weekly surface → read or cull.

---

## Brand Identity

**Name:** Ghost Notes  
**Tagline:** "The things you meant to read."  
**Vibe:** Quiet, slightly melancholic, elegant. Like a well-designed unread folder that actually makes you feel something. Not another productivity app — more like a mirror.

**Aesthetic direction:** Minimal, monochromatic with a single warm accent. Think of the inside of a well-worn Moleskine — cream paper, black ink, one red ribbon. Clean without being cold. Lots of whitespace. Typography-forward.

**Reference:** Instapaper meets Darkroom (the photo app) — editorial restraint with warmth.

**Colors:**
- Background: `#fafaf8` (warm white)
- Surface: `#f3f3f1` (off-white card)
- Text primary: `#1a1a1a`
- Text secondary: `#6b6b6b`
- Accent: `#c8a97e` (warm gold/sand — like aged paper)
- Dark mode: `#111110` bg, `#fafaf8` text, `#c8a97e` accent

**Typography:**
- Headings: `Playfair Display` — editorial, warm serif
- Body: `Inter` — clean, readable
- Accent labels: `Space Mono` — for counts, stats

---

## Features

### 1. Save Anything (One Tap)
- **Browser bookmarklet:** drag to bookmarks bar, tap on any page → saves URL + page title + favicon
- **Share sheet:** on iOS, share to Ghost Notes from any app
- **Keyboard shortcut:** `Cmd+Shift+G` to save current tab (browser extension)
- **Manual add:** paste a URL manually

On save: brief confirmation animation (subtle pulse on the accent color).

### 2. Weekly Surface ("The Haul")
- Every 7 days (configurable: 3, 7, 14 days), user gets a notification or email: "Your Ghost Notes are here."
- The Haul view shows all unread saves since last visit
- Each item shows: favicon, title, domain, time saved, first two lines of meta description
- Reading time estimate if available (via Read Time API or meta tag)
- **Read** — opens article in reading mode (use Mercury/Instapaper parser) or native browser
- **Cull** — swipe or click to archive without reading (sets a "I consciously chose not to read this" record)
- **Save for later** — move to a named list (e.g. "Deep Dives", "Research")

### 3. Reading Mode
- Clean article view: strips ads, navigation, noise
- Typography: Playfair Display body, optimal line length (~65ch)
- Font size + theme (light sepia/dark) controls
- Progress indicator (% read)
- "Done" button marks as read and archives

### 4. Lists / Collections
- User can create named lists
- Items can be moved to lists from the Haul or from individual view
- Default list: "Read Later" (unnamed saves)
- Lists are private — no sharing (yet)

### 5. Archive
- Everything read or culled goes to Archive
- Archive is searchable
- Can re-save from archive (moves back to Ghost Notes)
- Shows: date saved, date read, source domain

### 6. Stats (Subtle)
- "You've saved 234 articles. Read 89. Cull rate: 38%."
- Shown on the Archive page, not prominent. It's a quiet observation.

---

## Non-Features (Out of Scope)
- No social features, no sharing to Twitter/LinkedIn
- No "reading community" or highlights
- No AI summarisation
- No paid tier initially — simple MVP

---

## Tech Stack

**Web:** React + Vite, CSS (use the shared design system), React Router  
**Backend:** Supabase (auth, database, edge functions)  
**Article parsing:** Mercury Web Parser API (or `@postlight/parser`)  
**Email (weekly haul):** Supabase Edge Function + Resend  
**Browser extension:** Manifest V3, vanilla JS  
**iOS share extension:** Swift, App Groups with main app  

**Supabase schema:**
```
profiles       — id, email, created_at, settings_json
saves          — id, user_id, url, title, favicon, description, reading_time_minutes, saved_at, list_id
lists          — id, user_id, name, created_at
archive        — id, user_id, save_id, read_at, culled_at, culled (bool)
settings       — user_id, haul_frequency_days, email_enabled, theme
```

---

## Pages

1. **`/`** — Landing/marketing page. "The things you meant to read." Clean, minimal. CTA: sign up.
2. **`/app`** — Main app. Shows: nav (logo, settings, account), main content area.
   - **Empty state:** "No ghosts yet. Save your first article."
   - **Haul view (default):** Cards of unread saves, sorted by date saved (newest first)
   - **Reading mode:** Full-page article view
   - **Archive:** Searchable list of read/culled items
   - **Lists:** Grid of user's named lists with counts
3. **`/settings`** — Haul frequency, email on/off, dark/light default, export data
4. **`/auth`** — Sign up / sign in. Email magic link (no passwords).

---

## Design Direction (from shared system)

- **Surfaces:** Use `--surface-1` (dark: `#111110`) and `--surface-2` (`#1c1c1c`) from shared tokens
- **Accent:** Override `--color-accent` to `#c8a97e`
- **Typography:** Playfair Display for headings, Inter for body (from Google Fonts)
- **Cards:** Flat with subtle `border: 1px solid var(--border-subtle)`. No shadow.
- **Hover:** `background: var(--surface-3)`, 150ms
- **Motion:** Staggered card entrance: `opacity 0→1, translateY 8px→0`, 250ms, 60ms stagger
- **Reading mode:** Full bleed article, max-width 680px centered, Playfair 20px/1.7
- **Dark mode:** Use `#111110` as base (warmer than pure black)

---

## Build Roadmap

### Phase 1 — Core MVP
1. Landing page (`/`)
2. Auth (Supabase email magic link)
3. Save URL manually (text input)
4. Display saved articles list (Haul view)
5. Read article in reading mode (Mercury parser)
6. Mark as read / cull
7. Archive view
8. Dark/light mode

### Phase 2 — Save Everywhere
9. Browser bookmarklet
10. Browser extension (Manifest V3)
11. iOS Share Extension

### Phase 3 — Lists + Email
12. Named lists (create, move items)
13. Weekly email haul (Supabase Edge Function + Resend)
14. Settings page

### Phase 4 — Polish
15. Search in archive
16. Reading progress indicator
17. Stats page
18. Export data

---

## Human Inputs Needed (To-Do for Tommaso)
- [ ] Supabase project setup
- [ ] Resend account / API key for emails
- [ ] Mercury Parser API key (or Postlight)
- [ ] Custom domain (ghostnotes.app?)
- [ ] Logo design / brand mark
- [ ] App Store developer account (if iOS app)
- [ ] Browser extension icon
- [ ] Email template copy

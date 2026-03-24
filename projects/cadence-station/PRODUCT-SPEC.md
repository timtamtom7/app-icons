# Cadence Station — Product Spec

## Concept

Cadence Station is a focus app that pairs you with accountability partners for timed deep work sessions. The insight: knowing someone else is working right now — without seeing them, talking to them, or any social pressure — changes the game. You're not alone. Someone somewhere is in the zone too. When the session ends, both people get a quiet summary. No leaderboard. No competition. Just the shared signal that focused work happened.

**Core mechanic:** Start session (25/50/90 min) → get matched with a live partner → work in silence together → session ends → brief reflection.

---

## Brand Identity

**Name:** Cadence Station  
**Tagling:** "Find your rhythm. Together."  
**Vibe:** Warm but focused. Like a good jazz station playing quietly in the background of a coffee shop. The name evokes a train station — people coming and going, each on their own journey, sharing the same platform for a moment.

**Aesthetic direction:** Think iOS 26's Maps app meets Spotify's focus playlist UI. Warm dark surfaces, clean hierarchy, the sense that something is happening. Subtle ambient motion — a waveform, a progress ring, a quiet pulse. Never static, never distracting.

**Reference:** iOS 26 dark UI + Raycast's session UI + lo-fi jazz radio aesthetic.

**Colors:**
- Background: `#0d0d0e`
- Surface: `#18181b`
- Surface elevated: `#232326`
- Border: `rgba(255,255,255,0.07)`
- Text primary: `#fafafa`
- Text secondary: `#a1a1aa`
- Accent: `#f59e0b` (warm amber — like a desk lamp, like focused light)
- Partner online indicator: `#22c55e` (green — active, alive)

**Typography:**
- Headings: `Inter 600`, tight tracking
- Body: `Inter 400`
- Monospace (timer): `JetBrains Mono` — the timer is the hero, it should look precise

---

## Features

### 1. Session Setup
- Choose duration: 25 / 50 / 90 minutes (or custom)
- Choose session type: **Solo** (no pairing) or **Paired** (match with a partner)
- If Paired: option to set a topic/goal for the session (optional, private to user)
- Start session → immediately begins countdown

### 2. Solo Session
- Full-screen timer view: large countdown, progress ring
- Background: subtle ambient animation (slow-moving gradient or waveform)
- Optional: ambient sound toggle (brown noise, white noise, cafe sounds — from royalty-free audio)
- End early button (with confirmation)
- On complete: "Session complete. [X] minutes of focused work." Brief reflection: "What did you work on?" (optional, free text)

### 3. Paired Session (The Core Feature)
- Matching: user enters queue → shown "Waiting for a partner..." with live count of others waiting
- When matched: brief intro screen shows partner's first name + their stated goal (if they chose to share) — no photos, no profiles, no social graph
- Session begins for both simultaneously
- During session:
  - Split-screen or mirrored view — each person sees their own timer, and a subtle indicator that their partner is still present ("Alex is working...")
  - No communication — no chat, no video, no sound
  - If one person ends early: the other sees "Your partner ended early. Keep going?" — option to continue solo or end
- On complete (both finished): both see "You both completed the session." — quiet celebration, no leaderboard

### 4. Session History
- Calendar view showing sessions
- Streak counter: consecutive days with at least one session
- Total focus hours
- Weekly summary: "This week: 8 sessions, 6h 40m of focused work"
- Monthly report: same, in a digest view

### 5. Ambient Sounds (Nice-to-Have)
- Built-in sounds: Brown noise, White noise, Cafe, Forest, Rain, Ocean
- Volume control
- Continues if app is backgrounded (with notification showing timer)

### 6. Pairing Logic
- Simple matchmaking: anyone waiting for the same duration gets paired
- No user accounts required for MVP — session is identified by a generated session ID
- Later: optional sign-in to track history across devices

---

## Non-Features
- No video calls
- No chat during sessions
- No social graph or friend system
- No public profiles
- No competition or leaderboards
- No gamification beyond streaks

---

## Tech Stack

**Web:** React + Vite, CSS (shared design system), WebSocket for real-time pairing  
**Backend:** Supabase (presence channels for pairing, session management)  
**Audio:** Howler.js or native HTML5 Audio for ambient sounds  

**Supabase schema (minimal):**
```
sessions:
  id, duration_minutes, started_at, ended_at, completed (bool), type ('solo'|'paired')
pairings:
  session_a_id, session_b_id, matched_at
session_reflections:
  session_id, what_worked_on (text), created_at
```

**WebSocket pairing logic:**
1. User selects duration + Paired
2. Client joins Supabase Realtime "presence" channel for that duration
3. Presence shows count of others waiting
4. First user in channel waits; second user triggers match
5. Both clients receive match event, session begins

---

## Pages

1. **`/`** — Landing. "Find your rhythm. Together." Show the concept in 10 seconds. CTA: Start a Session.
2. **`/app`** — Home: start session widget, today's stats, streak, recent sessions.
3. **`/app/session/:id`** — Active session view (full-screen timer, ambient sounds, partner indicator).
4. **`/app/history`** — Calendar view + session list.
5. **`/app/settings`** — Default duration, sound preferences, account (if signed in).

---

## Design Direction (from shared system)

- **Surfaces:** `--surface-1: #0d0d0e`, `--surface-2: #18181b`, `--surface-3: #232326`
- **Accent:** `--color-accent: #f59e0b` (amber)
- **Timer:** JetBrains Mono, very large (96px+), `--text-6xl`
- **Progress ring:** SVG circle with stroke-dashoffset animation, accent color
- **Ambient waveform:** CSS animation or canvas — subtle, slow (8-12 second loop), low opacity
- **Cards:** `background: rgba(255,255,255,0.04)`, `border: 1px solid rgba(255,255,255,0.07)`
- **Motion:** Smooth, meditative. Timer tick has subtle pulse. Page transitions: fade 200ms.
- **Dark mode first:** Everything designed dark, light mode second
- **Mobile:** Full-screen session view. Timer takes 60% of viewport. Sound controls at bottom.

---

## Build Roadmap

### Phase 1 — Solo Sessions
1. Landing page
2. Session setup (duration picker)
3. Solo session view (timer, ambient sounds)
4. Session complete screen + optional reflection
5. Session history (list view)
6. Streak tracking

### Phase 2 — Paired Sessions
7. Supabase project setup (Realtime)
8. Pairing flow (waiting → matched)
9. Paired session view (partner indicator)
10. Partner ends early handling
11. Both-complete celebration (quiet)

### Phase 3 — Stats + Polish
12. Calendar view
13. Weekly/monthly summary
14. Dark/light mode
15. Mobile PWA (add to home screen)

---

## Human Inputs Needed (To-Do for Tommaso)
- [ ] Supabase project setup + Realtime enabled
- [ ] Ambient sound files (royalty-free, or use URLs from free sources like mynoise.net)
- [ ] Custom domain (cadencestation.com?)
- [ ] Logo concept
- [ ] App Store developer account (if iOS PWA)

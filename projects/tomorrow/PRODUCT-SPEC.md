# Tomorrow — Product Spec

## Concept

Tomorrow is a time capsule for your thoughts, your relationships, and your life. You write a message to someone — yourself, a partner, a friend, a future child — and set a delivery date. It arrives exactly once, on that day, and then it's gone. Not a journal. Not a notes app. A genuine act of trust placed in the future. The emotional weight of knowing this message will only arrive once, on a specific day, changes how you write it.

**Core mechanic:** Write → seal → set date → it arrives → read once → gone.

---

## Brand Identity

**Name:** Tomorrow  
**Tagline:** "A message from the past."  
**Vibe:** Intimate, thoughtful, a little bittersweet. Like finding a letter someone wrote you years ago in an old book. Clean and modern, but with emotional warmth. The kind of app you give as a gift.

**Aesthetic direction:** Think Darkroom (the photo app) meets a handwritten letter. The contrast between the clean digital UI and the emotional weight of the content is the tension. Typography-forward — the words are the product. Generous whitespace. Photography of letters, envelopes, stamps.

**Reference:** Darkroom's elegance + a handwritten letter + Linear's clean dark UI.

**Colors:**
- Background light: `#fafaf9`
- Background dark: `#0d0d0e`
- Surface light: `#ffffff`
- Surface dark: `#141415`
- Border: subtle, `rgba(0,0,0,0.06)` (light) / `rgba(255,255,255,0.07)` (dark)
- Text primary: `#1c1917` (warm black)
- Text secondary: `#78716c`
- Accent: `#e85d04` (warm burnt orange — like a wax seal, like urgency, like warmth)
- Recipient indicator: `#6366f1` (indigo — for "to someone else" letters)

**Typography:**
- Heading: `Inter 600`
- Body/letter: `Lora` — a warm, readable serif. Letters should feel personal. NOT Inter.
- UI labels: `Inter 500`

---

## Features

### 1. Write a Letter
- **Recipient:** "Future Me" / "Someone else"
  - Future Me: no address needed, arrives in your own inbox
  - Someone else: enter their email address
- **Subject (optional):** One line
- **Body:** Rich text area — plain text, comfortable reading width (60ch)
- **Tone selector (optional):** "How do you want this to feel?" — Quiet / Urgent / Playful / Somber (affects the email subject line tone)
- **Delivery date:** Date picker, minimum 1 day in the future, no maximum
- **"Write now, send later"** — letters are stored and dispatched automatically

### 2. Delivery System
- On delivery date: email is sent at 9:00 AM recipient's local time (or immediately on app open for Future Me)
- Email contains:
  - Sender name + "A letter from Tomorrow."
  - The letter body
  - A link to claim/read the letter
- Link is single-use: opens the letter on the Tomorrow website
- After reading: "This letter has been opened and is now sealed forever." That's it.
- Link expires after 7 days if unclaimed

### 3. Read a Letter (Incoming)
- Landing on `/letter/:id`:
  - "You have a letter from [Sender name]."
  - "Once you open this, it will be gone forever. Are you sure?"
  - **Open** / **Maybe later**
- Opening: brief moment (500ms) of anticipation — a closed envelope animation
- Then: the letter fades in, full page, letter body in Lora serif
- After reading: "This letter has been sealed forever." Return to home.

### 4. Your Letters (Sender View)
- **Drafting:** Letters not yet sent (can edit or delete)
- **Sealed:** Letters sent but not yet delivered (read-only, shows delivery countdown)
- **Delivered:** Letters that have been sent (shows delivery date, recipient)
- **Opened:** Letters the recipient has read (shows "Opened on [date]")
- **Expired:** Letters the recipient never claimed (7 days passed)

### 5. For "Someone Else" Letters
- Sender can choose: "Allow recipient to reply"
  - If yes: reply goes to sender's email
- Sender can cancel delivery before it sends (refunds concept — nothing charged, but control exists)

### 6. Notifications
- 7 days before delivery: sender gets "Your letter to [recipient] arrives in a week."
- On delivery: sender gets "Your letter has been delivered to [recipient]."
- If unclaimed after 7 days: "Your letter to [recipient] expired unclaimed."

### 7. Tomorrow Pro (Later)
- Multiple letters in a "Series" (e.g., "Letters to my future child, age by age")
- "Open on condition" (e.g., "Open when I get engaged")
- Photo attachments (encrypted)

---

## Non-Features
- No journaling — this is one-directional, not a diary
- No read receipts for sender beyond "opened/expired"
- No social sharing of letters
- No editing after sending

---

## Tech Stack

**Web:** React + Vite, CSS (shared design system)  
**Backend:** Supabase (auth, database, edge functions for scheduling)  
**Email:** Resend (transactional email with beautiful templates)  
**Scheduling:** Supabase Edge Functions + pg_cron (or Resend's scheduling API)  

**Supabase schema:**
```
profiles       — id, email, name, created_at
letters       — id, sender_id, recipient_email, recipient_name, subject, body, tone, deliver_at, sent_at, status ('draft'|'sealed'|'delivered'|'opened'|'expired'), allow_reply, created_at
letter_claims — id, letter_id, claimed_at
```

**Letter claim flow:**
- `/letter/:id` → checks if letter status is 'delivered' and not claimed
- On open: letter status → 'opened', claimed_at set
- Next visit to same URL: "This letter has been sealed forever."

---

## Pages

1. **`/`** — Landing. "A message from the past." Split: left = emotional copy + demo (letter reveal animation), right = "Write a letter" CTA.
2. **`/write`** — Letter composer: recipient, subject, body, tone, date.
3. **`/app`** — Dashboard: "Your letters" — drafts, sealed, delivered, opened. Stats: total sent, total opened.
4. **`/app/letters/:id`** — View/edit a draft, view a sealed letter's countdown.
5. **`/letter/:id`** — Recipient view (no auth needed to read). Open confirmation → letter reveal.
6. **`/settings`** — Profile, email notifications, delete account.

---

## Design Direction (from shared system)

- **Typography is the hero:** The letter body is set in Lora, 18px, line-height 1.8, max-width 60ch. This is the core experience.
- **Envelope as metaphor:** The "open letter" moment uses an envelope animation — closed → wax seal → opens. CSS animated.
- **Surfaces:** Warm light mode default: `--surface-1: #fafaf9`, `--surface-2: #f5f4f2` (warm tint)
- **Accent:** `--color-accent: #e85d04` (burnt orange)
- **Cards:** Warm white, `border: 1px solid rgba(0,0,0,0.05)`, very subtle shadow
- **Date picker:** Clean calendar, no heavy UI — simple HTML date input styled to match
- **Motion:**
  - Envelope open: 600ms, spring easing
  - Letter reveal: fade in, 400ms
  - Page transitions: fade 200ms
- **Dark mode:** Warm dark (`#0d0d0e`), letter body in Lora remains warm and readable
- **Mobile:** Full-screen letter writing. Reading view: full page, letter centered, minimal chrome.

---

## Build Roadmap

### Phase 1 — Core MVP
1. Landing page with envelope animation
2. Letter composer (write, set date, seal)
3. Email dispatch via Resend
4. `/letter/:id` read view (open confirmation + reveal)
5. Single-use claim (status update on open)
6. Sender dashboard (your letters)
7. Draft/sent/delivered/opened states

### Phase 2 — Notifications + Polish
8. Email notifications (7-day reminder, delivered, expired)
9. Resend email template design (beautiful, letter-style)
10. Dark/light mode

### Phase 3 — Multi-Recipient + Reply
11. "Send to someone else" (with email address)
12. Reply feature (if enabled by sender)
13. Cancel before delivery

### Phase 4 — Series + Condition (Later)
14. Letter Series
15. "Open on condition" (GPS, date, engagement detection via API)

---

## Human Inputs Needed (To-Do for Tommaso)
- [ ] Resend API key
- [ ] Supabase project setup
- [ ] Custom domain (tomorrow.app?)
- [ ] Logo concept + brand mark
- [ ] Email template design (HTML email — Resend supports this natively)
- [ ] Wax seal SVG asset (for envelope animation)
- [ ] App Store developer account (if iOS)

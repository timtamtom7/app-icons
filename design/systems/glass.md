# Glass Morphism — When & How to Use

**Reference:** iOS 26, Linear, Arc Browser

Glass is one of the most powerful visual tools in the current design language. It creates depth and layering without heaviness. But it's also one of the most abused — glass for the sake of it creates visual noise.

---

## When to Use Glass

✅ **Modals / Overlays** — the classic use. Blurred background makes content feel layered.  
✅ **Sticky navigation headers** — sits above content scrolling beneath.  
✅ **Floating panels / sidebars** — elevated above the main surface.  
✅ **Floating action buttons** — make them feel like they're floating above the surface.  
✅ **Cards on textured backgrounds** — when the background is an image or video.

❌ **Don't use glass on page backgrounds** — defeats the purpose, looks cheap.  
❌ **Don't stack glass on glass** — multiple glass layers competing creates confusion.  
❌ **Don't use full-opacity glass on large areas** — only use `rgba()` backgrounds with glass blur.

---

## CSS Recipe

```css
/* Standard glass surface */
.glass {
  background: rgba(20, 20, 21, 0.80);
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

/* Light mode glass */
.glass-light {
  background: rgba(255, 255, 255, 0.82);
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  border: 1px solid rgba(0, 0, 0, 0.06);
}

/* Strong glass (for modals over complex backgrounds) */
.glass-strong {
  background: rgba(20, 20, 21, 0.92);
  backdrop-filter: blur(24px) saturate(200%);
  -webkit-backdrop-filter: blur(24px) saturate(200%);
  border: 1px solid rgba(255, 255, 255, 0.10);
}
```

---

## Glass in Dark vs Light Mode

| Element | Dark Mode | Light Mode |
|---|---|---|
| Modal overlay | `rgba(0,0,0,0.6)` behind glass | `rgba(0,0,0,0.3)` behind glass |
| Glass background | `rgba(20,20,21,0.80)` | `rgba(255,255,255,0.82)` |
| Glass border | `rgba(255,255,255,0.08)` | `rgba(0,0,0,0.06)` |
| backdrop-filter | `blur(12px) saturate(180%)` | `blur(12px) saturate(160%)` |

---

## Linear-Style Glass Card

Linear uses glass with very subtle borders and almost no shadow — the glass *is* the depth.

```css
.card-glass {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: var(--radius-lg);
  backdrop-filter: blur(8px);
  transition: border-color 150ms ease, background 150ms ease;
}

.card-glass:hover {
  background: rgba(255, 255, 255, 0.07);
  border-color: rgba(255, 255, 255, 0.14);
}
```

---

## iOS 26 Liquid Glass

iOS 26's "liquid glass" goes further — elements feel like they're made of actual glass with refraction and reflection. For web, achieve a simplified version:

```css
.liquid-glass {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px) saturate(200%) brightness(1.1);
  -webkit-backdrop-filter: blur(20px) saturate(200%) brightness(1.1);
  border: 0.5px solid rgba(255, 255, 255, 0.12);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.15),
    0 8px 32px rgba(0, 0, 0, 0.3);
}
```

The `brightness(1.1)` adds a subtle luminescence. The `inset` shadow adds a top highlight edge — the key to the "glass edge" look.

---

## Rules

1. **Always include `-webkit-backdrop-filter`** — Safari requires the prefix.
2. **Test on real iOS Safari** — backdrop-filter behaves differently on iOS.
3. **Don't animate backdrop-filter** — it causes repaints. Set it statically.
4. **Performance:** backdrop-filter is GPU-accelerated on modern browsers. It's fine to use on most surfaces.
5. **Fallback:** Always ensure text remains readable if backdrop-filter fails. Use sufficient contrast.

---

## Accessibility

`backdrop-filter: blur()` can reduce text contrast below WCAG thresholds on some backgrounds.

**Rule:** After applying glass, always verify body text has `4.5:1` contrast ratio. If the background behind the glass is variable (e.g. image), use `background: rgba(0,0,0,0.5)` behind the glass to guarantee contrast.

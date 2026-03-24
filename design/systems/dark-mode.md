# Dark Mode — Rules & Patterns

Dark mode is the default for most of our apps. Light mode must work, but design for dark first.

---

## The Dark Mode Rules

### 1. Never Use Pure Black
❌ `#000000` — too harsh, loses all depth  
✅ `#0d0d0e` or `#0a0a0b` — rich dark that still has depth

### 2. Surface Hierarchy in Dark Mode

Use 4 surface levels to create hierarchy without light:

```
Surface 1 (page bg):   #0d0d0e
Surface 2 (cards):     #141415
Surface 3 (inputs/elevated): #1c1c1e
Surface 4 (hover):     #252528
```

### 3. All Text Uses Variable Opacity
```
Primary:   #f5f5f7  (high contrast — body, headings)
Secondary: #a1a1aa  (labels, secondary info)
Tertiary: #6b7280   (placeholders, disabled)
Disabled: #3f3f46   (truly disabled)
```

### 4. Semantic Colors Must Work in Both Modes
```css
/* Error — use these values exactly */
--color-error:       #ef4444;
--color-error-subtle: rgba(239, 68, 68, 0.12);

/* On dark backgrounds, error-subtle is readable */
/* On light backgrounds, error-subtle must also be visible */
```

### 5. Dark Mode Toggle Pattern
```javascript
// Toggle with data-theme attribute on <html>
const toggleDark = () => {
  const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
  document.documentElement.setAttribute('data-theme', isDark ? 'light' : 'dark');
};
```

### 6. System Preference Respect
```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* Dark mode defaults from :root tokens */
  }
}
```

### 7. Transitions
```css
html {
  transition:
    background-color 200ms ease,
    color 200ms ease;
}
```

### 8. What to Animate vs Not Animate
❌ Don't animate `background-color` transitions on large surfaces — it's slow  
✅ Do animate `color` (text color) — fast  
✅ Do animate `border-color` — fast  
✅ Do animate `opacity` — fast  
✅ Do animate `transform` — fast  

---

## Quick Checklist

Before any feature is "done":

- [ ] Tested on dark background (#0d0d0e page)
- [ ] Tested on light background (#ffffff page)
- [ ] All text passes WCAG AA in both modes
- [ ] Borders are visible in both modes (adjust opacity if needed)
- [ ] No pure black or pure white text in dark mode
- [ ] Error/success/warning states work in both modes
- [ ] No jarring flash on page load (set theme before paint)
- [ ] SVG icons use `currentColor` (adapts to text color)
- [ ] Hover states are different in both modes
- [ ] Glass elements tested in both modes

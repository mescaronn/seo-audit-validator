---
name: seo-audit-validator-accessibility
description: "Validates Accessibility rules from SEO audit output. Use when audit contains any of: ARIA Labels, Color Contrast, Form Labels, Heading Order, Landmarks, Link Text, Skip Link, Table Headers, Touch Targets, Video Captions, Zoom Disabled, Focus Visible"
---

# SEO Audit Validator: Accessibility

## Rules

### ARIA Labels (`a11y-aria-labels`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('button, [role="button"], input, select, textarea')].filter(el => {
  const hasLabel = el.getAttribute('aria-label') || el.getAttribute('aria-labelledby') || el.textContent.trim();
  return !hasLabel;
}).map(el => ({tag: el.tagName, type: el.type || '', outerHTML: el.outerHTML.substring(0, 100)}))
```
**Compare:** Count vs tool's count.

---

### Color Contrast (`a11y-color-contrast`) | Severity: warn
**Console (white text elements as proxy):**
```js
[...document.querySelectorAll('*')].filter(el => {
  const color = getComputedStyle(el).color;
  return color === 'rgb(255, 255, 255)';
}).length
```
**Visual check:** DevTools → Elements → select element → Accessibility panel → check contrast ratio.
**Compare:** Tool's count of issues vs what you find. Tool may flag specific elements — verify each one visually.
**Note:** White text on dark backgrounds typically has good contrast. Verify background color before flagging.

---

### Form Labels (`a11y-form-labels`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('input:not([type="hidden"]):not([type="submit"]):not([type="button"]):not([type="reset"])')].filter(input => {
  const id = input.id;
  const hasLabel = id && document.querySelector('label[for="' + id + '"]');
  const hasAria = input.getAttribute('aria-label') || input.getAttribute('aria-labelledby');
  return !hasLabel && !hasAria;
}).map(el => ({type: el.type, id: el.id, name: el.name}))
```
**Compare:** Count vs tool's count.

---

### Heading Order (`a11y-heading-order`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('h1, h2, h3, h4, h5, h6')].slice(0, 10).map(h => ({tag: h.tagName, text: h.textContent.trim().substring(0, 50)}))
```
**Compare:** Check if sequence skips levels (H1→H3 skipping H2). First heading should be H1.
**Note:** Heading Order (a11y) and Heading Hierarchy (content) may overlap — same rule, different category.

---

### Landmarks (`a11y-landmark-regions`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('header, nav, main, footer, aside, [role="navigation"], [role="main"], [role="contentinfo"], [role="banner"]')].map(el => ({tag: el.tagName, role: el.getAttribute('role')}))
```
**Compare:** Tool says "0 landmark regions missing" = pass. If missing → correct flag.

---

### Link Text (`a11y-link-text`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('a')].reduce((acc, a) => {
  const text = a.textContent.trim().toLowerCase();
  if (['click here','read more','learn more','view','more','here'].includes(text)) {
    acc[text] = (acc[text]||0)+1;
  }
  return acc;
}, {})
```
**Also check repeated text to different URLs:**
```js
[...document.querySelectorAll('a')].reduce((acc, a) => {
  const text = a.textContent.trim().toLowerCase();
  if (!acc[text]) acc[text] = new Set();
  acc[text].add(a.href);
  return acc;
}, {})
```
**Compare:** Tool reports issue count. Match generic text counts.

---

### Skip Link (`a11y-skip-link`) | Severity: warn
**Console:**
```js
document.querySelector('a[href="#main"], a[href="#content"], a[href="#maincontent"], .skip-link, .skip-to-content, .sr-only')
```
**Compare:** null → correct flag. Found → false positive.

---

### Table Headers (`a11y-table-headers`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('table')].map((table, i) => ({index: i, hasTh: !!table.querySelector('th'), hasScope: !!table.querySelector('[scope]')}))
```
**Compare:** Tables without th elements → correct flag. All have headers → false positive.

---

### Touch Targets (`a11y-touch-targets`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a, button, [role="button"], input, select')].filter(el => {
  const r = el.getBoundingClientRect();
  return (r.width < 44 || r.height < 44) && r.width > 0;
}).length
```
**Compare:** Count vs tool's count (tool may only check certain element types).
**Note:** Many Shopify theme elements are small by default. Report if count differs significantly.

---

### Video Captions (`a11y-video-captions`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('video, audio')].map((el, i) => ({
  index: i,
  type: el.tagName,
  hasTracks: el.querySelectorAll('track[kind="captions"], track[kind="subtitles"]').length > 0,
  parent: el.parentElement.className
}))
```
**Compare:** Count of elements without tracks vs tool's count.
**Note:** Tool shows "unknown" for video locations — report this as output issue.

---

### Zoom Disabled (`a11y-zoom-disabled`) | Severity: fail
**Console:**
```js
const viewport = document.querySelector('meta[name="viewport"]')?.getAttribute('content');
console.log('Viewport:', viewport);
console.log('Zoom disabled:', viewport?.includes('user-scalable=no') || viewport?.includes('maximum-scale=1'))
```
**Compare:** True → correct flag. False → false positive.

---

### Focus Visible (`a11y-focus-visible`) | Severity: warn/fail
**Check:** DevTools → Elements → select a link → :hov → tick :focus → check if outline/border appears.
**Console:**
```js
document.querySelector('a')?.style.outline || 'check CSS'
```
**Note:** Primarily a visual check. Look for `outline: none` or `outline: 0` on interactive elements in CSS.

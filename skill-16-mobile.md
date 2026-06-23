---
name: seo-audit-validator-mobile
description: "Validates Mobile rules from SEO audit output. Use when audit contains any of: Font Size, Interstitials, Viewport Width, Multiple Viewports, Horizontal Scroll"
---

# SEO Audit Validator: Mobile

## Rules

### Viewport Width (`mobile-viewport-width`) | Severity: warn
```js
document.querySelector('meta[name="viewport"]')?.getAttribute('content')
```
**Compare:** Should contain 'width=device-width'. Fixed pixel width (e.g. width=640) → correct flag.

---

### Multiple Viewports (`mobile-multiple-viewports`) | Severity: fail
```js
document.querySelectorAll('meta[name="viewport"]').length
```
**Compare:** >1 → correct flag. 1 → false positive.

---

### Zoom Disabled (see Accessibility) | Severity: fail
```js
const v = document.querySelector('meta[name="viewport"]')?.getAttribute('content');
console.log('Viewport:', v, '| Zoom disabled:', v?.includes('user-scalable=no') || v?.includes('maximum-scale=1'))
```

---

### Font Size (`mobile-font-size`) | Severity: fail/warn
```js
const bodySize = parseInt(getComputedStyle(document.body).fontSize);
console.log('Body font size:', bodySize + 'px', '| Below 16px:', bodySize < 16)
```
**Also check small text:**
```js
[...document.querySelectorAll('p, span, li, td')].filter(el => parseInt(getComputedStyle(el).fontSize) < 12).length
```
**Compare:** Body <16px → tool may be correct. Count of elements <12px vs tool's count.

---

### Horizontal Scroll (`mobile-horizontal-scroll`) | Severity: fail/warn
```js
console.log('Horizontal scroll:', document.documentElement.scrollWidth > window.innerWidth, '| Page width:', document.documentElement.scrollWidth, '| Window width:', window.innerWidth)
```
**Compare:** scrollWidth > innerWidth → correct flag.

---

### Interstitials (`mobile-interstitials`) | Severity: fail/warn
**Console (total overlays):**
```js
[...document.querySelectorAll('div.overlay, div[class*="overlay"]')].filter(el => {
  const s = getComputedStyle(el);
  return s.display !== 'none' && s.visibility !== 'hidden';
}).length
```
**Console (high z-index = intrusive popups):**
```js
[...document.querySelectorAll('div.overlay, div[class*="overlay"]')].filter(el => {
  const s = getComputedStyle(el);
  return s.display !== 'none' && parseInt(s.zIndex) > 100;
}).length
```
**Compare:** Total overlays vs tool's count. High z-index count = actual intrusive popups.
**Note:** Decorative card overlays (low z-index) ≠ interstitials. Report if tool is counting decorative ones.

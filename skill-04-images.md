---
name: seo-audit-validator-images
description: "Validates Images rules from SEO audit output. Use when audit contains any of: Dimensions, Broken Images, Alt Present, Alt Quality, Background Image SEO, Lazy Loading, Size, Responsive, Modern Format, Figure Captions, Filename Quality, Picture Element, Inline SVG Size, Alt Length"
---

# SEO Audit Validator: Images

## Rules

### Alt Present (`images-alt-present`) | Severity: fail
**Console:**
```js
[...document.querySelectorAll('img:not([alt])')].map(img => img.src.split('/').pop())
```
**Compare:** Count vs tool's count. Empty → false positive.

---

### Alt Quality (`images-alt-quality`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img[alt]')].filter(img => {
  const alt = img.alt.trim().toLowerCase();
  return ['image','photo','picture','img','icon','banner','logo'].includes(alt) || alt.length < 5;
}).map(img => ({src: img.src.split('/').pop(), alt: img.alt}))
```
**Compare:** Count vs tool's count. Check if tool's flagged URLs match results.
**Note:** Tool may flag images not visible in your results — verify each flagged URL exists in DOM.

---

### Alt Length (`images-alt-length`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img[alt]')].filter(img => img.alt.length > 125 || (img.alt.length < 5 && img.alt.length > 0)).map(img => ({src: img.src.split('/').pop(), alt: img.alt, length: img.alt.length}))
```
**Compare:** Count vs tool's count.

---

### Dimensions (`images-dimensions`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img')].filter(img => !img.getAttribute('width') || !img.getAttribute('height')).length
```
**Compare:** Count vs tool's count.

---

### Lazy Loading (`images-lazy-loading`) | Severity: warn
**Console (below-fold without lazy):**
```js
[...document.querySelectorAll('img')].filter(img => !img.getAttribute('loading')).length
```
**Also check visible count:**
```js
[...document.querySelectorAll('img')].filter(img => {
  const r = img.getBoundingClientRect();
  return r.top > window.innerHeight && !img.getAttribute('loading');
}).length
```
**Compare:** Tool flags below-fold images missing lazy. Compare counts.
**Note:** If console shows 0 below-fold, may be false positive or tool is counting differently.

---

### Size (`images-size`) | Severity: warn
**Check:** DevTools → Network tab → filter by Img → sort by Size descending → count images >200KB.
**Compare:** Count vs tool's reported count.
**Note:** Cannot check via Console due to CORS. Must use Network tab.

---

### Responsive (`images-responsive`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img')].filter(img => !img.hasAttribute('srcset') && !img.closest('picture')).length
```
**Compare:** Count vs tool's count.

---

### Modern Format (`images-modern-format`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img')].reduce((acc, img) => {
  const ext = img.src.split('.').pop().split('?')[0].toLowerCase();
  acc[ext] = (acc[ext] || 0) + 1;
  return acc;
}, {})
```
**Calculate:** `webp + avif` count / total (excluding svg) = modern format %.
**Compare:** Tool's % vs your calculated %. Check if SVG is being counted as modern (it shouldn't be).

---

### Broken Images (`images-broken`) | Severity: fail
**Check:** DevTools → Network tab → filter by Img → look for red 404 status.
**Also Console:**
```js
[...document.querySelectorAll('img')].filter(img => !img.complete || img.naturalWidth === 0).map(img => img.src)
```
**Compare:** Count vs tool's count.

---

### Figure Captions (`images-figure-captions`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('figure')].filter(fig => !fig.querySelector('figcaption')).length
```
**Compare:** Count vs tool's count. 0 → false positive.

---

### Filename Quality (`images-filename-quality`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img')].filter(img => {
  const name = img.src.split('/').pop().split('?')[0];
  return /^(img|image|dsc|photo|screenshot|pic|banner|_)[\d_-]/i.test(name);
}).map(img => img.src.split('/').pop().split('?')[0])
```
**Compare:** Count vs tool's count.

---

### Picture Element (`images-picture-element`) | Severity: fail
**Console:**
```js
[...document.querySelectorAll('picture')].map((pic, i) => ({index: i, sources: pic.querySelectorAll('source').length, hasImg: !!pic.querySelector('img')}))
```
**Compare:** Count sources for each picture. Tool flags pictures without source OR without img fallback.
**Note:** Check if tool is flagging "no source" or "no img fallback" — different issues.

---

### Inline SVG Size (`images-inline-svg-size`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('svg')].map(svg => ({
  id: svg.id || 'no-id',
  size: (new XMLSerializer().serializeToString(svg).length / 1024).toFixed(1) + 'KB'
})).filter(s => parseFloat(s.size) > 5)
```
**Compare:** Count vs tool's count. Tool flags SVGs >5KB.

---

### Background Image SEO (`images-background-seo`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('[style*="background-image"]')].map(el => ({tag: el.tagName, class: el.className, style: el.style.backgroundImage}))
```
**Compare:** Count vs tool's count. Content-relevant images in CSS backgrounds are flagged.

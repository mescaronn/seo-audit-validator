---
name: seo-audit-validator-performance
description: "Validates Performance rules from SEO audit output. Use when audit contains any of: LCP, CLS, INP, TTFB, FCP, DOM Size, Preconnect Hints, Render-Blocking, Lazy Above Fold, Page Weight, Video for Animations, LCP Hints, Font Loading, Text Compression, Brotli Compression, Cache Policy, Minify CSS, Minify JS, Response Time, HTTP/2, JS File Size, CSS File Size"
---

# SEO Audit Validator: Performance

## Rules

### LCP / CLS / INP / TTFB / FCP (Core Web Vitals) | Severity: warn/fail
**Note:** Cannot validate via Console. Use Chrome DevTools Lighthouse tab.
**Action:** Run Lighthouse → Performance → compare CWV metrics to tool's reported values.
**Thresholds:**
- LCP: Good <2.5s, Warn 2.5-4s, Fail >4s
- CLS: Good <0.1, Warn 0.1-0.25, Fail >0.25
- INP: Good <200ms, Warn 200-500ms, Fail >500ms
- TTFB: Good <800ms, Warn 800-1800ms, Fail >1800ms
- FCP: Good <1.8s, Warn 1.8-3s, Fail >3s

---

### INP (`cwv-inp`) | Severity: warn/fail
**Console (partial — requires interaction):**
```js
new PerformanceObserver((list) => {
  list.getEntries().forEach(e => console.log(e.name, Math.round(e.duration)+'ms'))
}).observe({type: 'event', buffered: true})
```
**Then:** Click buttons/interact with page for 10-15 seconds. Check console for entries >200ms.
**Note:** Hard to replicate exact conditions. Use Lighthouse for authoritative reading.

---

### DOM Size (`perf-dom-size`) | Severity: warn/fail
**Console:**
```js
console.log('Total nodes:', document.querySelectorAll('*').length, '| Head children:', document.head.children.length)
```
**Compare:** Tool reports total nodes + max children. Compare both numbers.
**Thresholds:** <800 nodes = pass, 800-1500 = warn, >1500 = fail. Head children <60 = pass.

---

### Preconnect Hints (`perf-preconnect`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('link[rel="preconnect"], link[rel="dns-prefetch"]')].map(l => l.href)
```
**Compare:** If tool flags "missing preconnect" but these exist → false positive. If empty → tool may be correct.

---

### Render-Blocking (`perf-render-blocking`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('head script:not([async]):not([defer])')].map(s => s.src || 'inline').filter(s => s !== 'inline')
```
**Compare:** Count matches tool's reported count. Empty = false positive.

---

### Lazy Above Fold (`perf-lazy-above-fold`) | Severity: warn/fail
**Console (Step 1 — check first lazy image):**
```js
document.querySelectorAll('img[loading="lazy"]')[0]?.className
```
**Console (Step 2 — check all above-fold images):**
```js
[...document.querySelectorAll('img')].filter(img => {
  const r = img.getBoundingClientRect();
  return r.top < window.innerHeight && r.width > 200;
}).map(img => ({src: img.src.split('/').pop(), loading: img.getAttribute('loading'), top: Math.round(img.getBoundingClientRect().top)}))
```
**Compare:** If no above-fold images have loading="lazy" → false positive. If any do → tool correct.
**Note:** Tool may flag nav/menu images. Check if flagged image is actually visible above fold.

---

### Page Weight (`perf-page-weight`) | Severity: warn/fail
**Check:** DevTools → Network tab → reload page → check "transferred" total at bottom.
**Compare:** Tool's reported total vs Network tab total.
**Threshold:** <3MB recommended.

---

### Video for Animations (`perf-video-for-animations`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('img')].filter(img => img.src.endsWith('.gif')).map(img => img.src)
```
**Compare:** If GIFs found → tool may be correct. Empty → false positive.

---

### LCP Hints (`perf-lcp-hints`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('link[rel="preload"][as="image"]')].map(l => l.href)
```
**Also check:**
```js
[...document.querySelectorAll('img[fetchpriority="high"]')].map(img => img.src.split('/').pop())
```
**Compare:** If no preload or fetchpriority="high" on LCP image → tool correct.

---

### Font Loading (`perf-font-loading`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('link[rel="preload"][as="font"]')].map(l => l.href)
```
**Check:** DevTools → Elements → search for "font-display" in CSS. Look for "swap".

---

### Text Compression / Brotli Compression (`perf-text-compression` / `perf-brotli`) | Severity: warn
**Check:** DevTools → Network → click HTML document → Headers → look for `content-encoding: gzip` or `br`.
**Compare:** If missing → tool correct. If present → false positive.

---

### Cache Policy (`perf-cache-policy`) | Severity: warn
**Check:** DevTools → Network → click CSS/JS file → Headers → look for `cache-control` header.
**Compare:** If missing or short max-age → tool correct.

---

### Minify CSS / Minify JS (`perf-minify-css` / `perf-minify-js`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('link[rel="stylesheet"]')].map(l => l.href)
```
**Check:** Click a CSS file in Network tab → Preview → check if minified (no whitespace/newlines).

---

### Response Time (`perf-response-time`) | Severity: warn/fail
**Check:** DevTools → Network → reload → click HTML document → Timing → "Waiting (TTFB)".
**Threshold:** <800ms = pass.

---

### HTTP/2 (`perf-http2`) | Severity: warn
**Check:** DevTools → Network → right-click column headers → add "Protocol" column → check if h2 or h3.
**Compare:** If shows http/1.1 → tool correct. If h2/h3 → false positive.

---

### JS File Size / CSS File Size (`perf-js-file-size` / `perf-css-file-size`) | Severity: warn/fail
**Check:** DevTools → Network → filter by JS or CSS → sort by Size descending → check large files.
**Threshold:** Individual files >500KB flagged.

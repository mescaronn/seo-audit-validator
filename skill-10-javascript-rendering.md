---
name: seo-audit-validator-javascript-rendering
description: "Validates JavaScript Rendering rules from SEO audit output. Use when audit contains any of: JS Noindex Mismatch, JS Title Modified, JS Description Modified, JS H1 Modified, JS Rendered Content, JS Rendered Links, JS Blocked Resources, SSR Check, JS Rendered Title, JS Rendered Description, JS Rendered H1, JS Rendered Canonical, JS Canonical Mismatch"
---

# SEO Audit Validator: JavaScript Rendering

## Base Concept
These rules compare the raw HTML source (what crawlers see) vs the rendered DOM (what you see). To check source: right-click page → View Page Source → Ctrl+F to search.

## Rules

### JS Rendered Title (`js-rendered-title`) | Severity: fail
**Source check:** View page source → search for `<title>`. If absent → correct flag.
**Console (rendered):**
```js
document.querySelector('title')?.textContent
```
**Compare:** Title in rendered DOM but missing in source → tool correct. Title in both → false positive.

---

### JS Rendered Description (`js-rendered-description`) | Severity: warn
**Source check:** View page source → search for `meta name="description"`.
**Console (rendered):**
```js
document.querySelector('meta[name="description"]')?.getAttribute('content')
```
**Compare:** Present in rendered but absent in source → correct.

---

### JS Rendered H1 (`js-rendered-h1`) | Severity: fail
**Source check:** View page source → search for `<h1>`.
**Console (rendered):**
```js
document.querySelector('h1')?.textContent.trim()
```
**Compare:** H1 in rendered but absent in source → correct flag.

---

### JS Rendered Canonical (`js-rendered-canonical`) | Severity: fail
**Source check:** View page source → search for `rel="canonical"`.
**Console (rendered):**
```js
document.querySelector('link[rel="canonical"]')?.href
```
**Compare:** Canonical in rendered but absent in source → correct flag.

---

### JS Canonical Mismatch (`js-canonical-mismatch`) | Severity: fail
**Source canonical:** View page source → find canonical.
**Console (rendered canonical):**
```js
document.querySelector('link[rel="canonical"]')?.href
```
**Compare:** If source canonical ≠ rendered canonical → correct flag.

---

### JS Noindex Mismatch (`js-noindex-mismatch`) | Severity: fail
**Source check:** View page source → search for `noindex`.
**Console (rendered):**
```js
document.querySelector('meta[name="robots"]')?.getAttribute('content')
```
**Compare:** Noindex in source but not rendered (or vice versa) → correct flag.

---

### JS Title Modified (`js-title-modified`) | Severity: warn
**Source check:** View page source → note exact title.
**Console (rendered):**
```js
document.querySelector('title')?.textContent
```
**Compare:** If rendered title differs from source title → correct flag. JS is modifying it.

---

### JS Description Modified (`js-description-modified`) | Severity: warn
**Source check:** View page source → note exact description.
**Console (rendered):**
```js
document.querySelector('meta[name="description"]')?.getAttribute('content')
```
**Compare:** Different from source → correct flag.

---

### JS H1 Modified (`js-h1-modified`) | Severity: warn
**Source check:** View page source → find H1 content.
**Console (rendered):**
```js
document.querySelector('h1')?.textContent.trim()
```
**Compare:** Different → correct flag.

---

### JS Rendered Content (`js-rendered-content`) | Severity: warn
**Source check:** View page source → check if main body content exists or is mostly empty `<div>` wrappers.
**Console:**
```js
console.log('Rendered text length:', document.body.innerText.length)
```
**Source:** View source and search for same content. If source is much shorter → JS is rendering main content.

---

### JS Rendered Links (`js-rendered-links`) | Severity: warn
**Console (rendered):**
```js
[...document.querySelectorAll('a[href]')].filter(a => a.hostname === window.location.hostname).length
```
**Source check:** View source → count `<a href` occurrences. If rendered count >> source count → JS is generating links.

---

### JS Blocked Resources (`js-blocked-resources`) | Severity: warn
**Console:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => {
  const blocks = t.match(/Disallow:.+/gi)?.filter(l => l.match(/\.(js|css)/i));
  console.log('Blocked JS/CSS rules:', blocks || 'None');
})
```
**Compare:** No blocking rules → false positive.

---

### SSR Check (`js-ssr-check`) | Severity: warn/fail
**Source check:** View page source → check if meaningful content exists in raw HTML.
**Console:**
```js
console.log('Rendered text length:', document.body.innerText.length)
```
**Source:** Shorter source → less SSR. Compare source text to rendered text.

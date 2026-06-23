---
name: seo-audit-validator-core-seo
description: "Validates Core SEO rules from SEO audit output. Use when audit contains any of: Title Present, Title Length, Description Present, Description Length, Favicon Present, H1 Present, H1 Single, Nosnippet Directive, Robots Meta, Viewport Present, Title Uniqueness, Canonical Present, Canonical Valid, Canonical Header, Conflicting Canonicals, Canonical to Homepage, Canonical HTTP Mismatch, Canonical Loop, Canonical to Noindex"
---

# SEO Audit Validator: Core SEO

## How to Use
1. User pastes SEO audit report with flagged rules
2. For each rule below that appears in the report, provide the console command
3. User runs command in DevTools Console (F12 → Console tab)
4. User reports result back
5. Compare result to tool's output and determine status

## Status Labels
- **Correct** — Tool flagged accurately, counts match
- **Undercounted** — Actual count > tool's count
- **Overcounted** — Actual count < tool's count
- **False Positive** — Tool flagged but issue doesn't exist on page

## Rules

### Title Present (`core-title-present`) | Severity: fail
**Console:**
```js
document.querySelector('title')?.textContent || 'MISSING'
```
**Compare:** If returns 'MISSING' → correct flag. If returns title text → false positive.

---

### Title Length (`core-title-length`) | Severity: warn
**Console:**
```js
const t = document.querySelector('title')?.textContent?.trim();
console.log('Title:', t, '| Length:', t?.length)
```
**Compare:** Tool flags if <30 or >60 chars. Compare actual length to tool's reported length.

---

### Description Present (`core-description-present`) | Severity: fail
**Console:**
```js
document.querySelector('meta[name="description"]')?.getAttribute('content') || 'MISSING'
```
**Compare:** If 'MISSING' → correct. If content exists → false positive.

---

### Description Length (`core-description-length`) | Severity: warn
**Console:**
```js
const d = document.querySelector('meta[name="description"]')?.getAttribute('content');
console.log('Description:', d, '| Length:', d?.length)
```
**Compare:** Tool flags if <120 or >160 chars. Compare actual length to tool's reported value.

---

### Favicon Present (`core-favicon-present`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('link[rel="icon"], link[rel="shortcut icon"], link[rel="apple-touch-icon"]')].map(l => l.href)
```
**Compare:** Empty array → correct flag. Has URLs → false positive.

---

### H1 Present (`core-h1-present`) | Severity: fail
**Console:**
```js
document.querySelectorAll('h1').length + ' H1 tags found'
```
**Compare:** 0 → correct flag. 1+ → false positive.

---

### H1 Single (`core-h1-single`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('h1')].map(h => h.textContent.trim())
```
**Compare:** More than 1 result → tool correct. 1 result → false positive.

---

### Nosnippet Directive (`core-nosnippet`) | Severity: warn
**Console:**
```js
document.querySelector('meta[name="robots"]')?.getAttribute('content')
```
**Compare:** Check if 'nosnippet' or 'max-snippet:0' is present. If yes → correct. If not → false positive.

---

### Robots Meta (`core-robots-meta`) | Severity: warn
**Console:**
```js
document.querySelector('meta[name="robots"]')?.getAttribute('content') || 'No robots meta'
```
**Compare:** Check for noindex, nofollow, noarchive. If present → correct. If not → false positive.

---

### Viewport Present (`core-viewport-present`) | Severity: fail
**Console:**
```js
document.querySelector('meta[name="viewport"]')?.getAttribute('content') || 'MISSING'
```
**Compare:** 'MISSING' → correct flag. Has content → false positive.

---

### Title Uniqueness (`core-title-unique`) | Severity: warn/fail
**Note:** Crawl-mode rule — cannot validate on single page. Skip console validation.
**Action:** Note in report as "crawl-mode rule, requires bulk check."

---

### Canonical Present (`core-canonical-present`) | Severity: fail
**Console:**
```js
document.querySelector('link[rel="canonical"]')?.href || 'MISSING'
```
**Compare:** 'MISSING' → correct flag. Has URL → false positive.

---

### Canonical Valid (`core-canonical-valid`) | Severity: warn
**Console:**
```js
const c = document.querySelector('link[rel="canonical"]')?.href;
console.log('Canonical:', c, '| Is absolute:', c?.startsWith('http'))
```
**Compare:** If not absolute URL or missing → correct. If valid absolute URL → false positive.

---

### Canonical Header (`core-canonical-header`) | Severity: warn
**Console:**
```js
const html = document.querySelector('link[rel="canonical"]')?.href;
fetch(window.location.href, {method:'HEAD'}).then(r => {
  const header = r.headers.get('link');
  console.log('HTML canonical:', html, '| HTTP Link header:', header);
})
```
**Compare:** If both exist and differ → correct. If match or only one exists → false positive.

---

### Conflicting Canonicals (`core-canonical-conflicting`) | Severity: fail
**Console:**
```js
const html = document.querySelector('link[rel="canonical"]')?.href;
console.log('HTML canonical:', html);
fetch(window.location.href, {method:'HEAD'}).then(r => console.log('HTTP canonical header:', r.headers.get('link')));
```
**Compare:** If they point to different URLs → correct. If same → false positive.

---

### Canonical to Homepage (`core-canonical-to-homepage`) | Severity: warn
**Console:**
```js
const canonical = document.querySelector('link[rel="canonical"]')?.href;
const homepage = window.location.origin + '/';
console.log('Canonical:', canonical, '| Is homepage:', canonical === homepage)
```
**Compare:** If canonical equals homepage and current page is not homepage → correct.

---

### Canonical HTTP Mismatch (`core-canonical-http-mismatch`) | Severity: warn
**Console:**
```js
const canonical = document.querySelector('link[rel="canonical"]')?.href;
console.log('Page protocol:', window.location.protocol, '| Canonical protocol:', canonical?.split(':')[0] + ':')
```
**Compare:** If protocols differ → correct. If same → false positive.

---

### Canonical Loop (`core-canonical-loop`) | Severity: fail
**Console:**
```js
document.querySelector('link[rel="canonical"]')?.href
```
**Compare:** If canonical = current page URL (self-referencing) → not a loop. If points elsewhere, requires checking that target's canonical too. Note: hard to fully validate on single page.

---

### Canonical to Noindex (`core-canonical-to-noindex`) | Severity: fail
**Console:**
```js
const canonical = document.querySelector('link[rel="canonical"]')?.href;
const robots = document.querySelector('meta[name="robots"]')?.getAttribute('content');
console.log('Canonical:', canonical, '| Robots:', robots)
```
**Compare:** Requires fetching canonical target. On same page: if page has noindex and canonical points elsewhere → potentially correct.

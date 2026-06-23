---
name: seo-audit-validator-links
description: "Validates Links rules from SEO audit output. Use when audit contains any of: Nofollow Usage, Anchor Text, Link Depth, HTTPS Downgrade, External Count, Invalid Links, Tel & Mailto, Localhost Links, Local File Links, Broken Fragments, Excessive Links, OnClick Navigation, Whitespace Href, Orphan Pages, Redirect Chains, Broken Internal, External Valid, Internal Links, Dead-End Pages"
---

# SEO Audit Validator: Links

## Rules

### Nofollow Usage (`links-nofollow-appropriate`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[rel*="nofollow"]')].map(a => ({href: a.href, text: a.textContent.trim(), visible: a.offsetParent !== null}))
```
**Compare:** Tool reports count + URLs. Match against results. Internal links with nofollow = valid flag. External links (social, ads) = expected nofollow, tool may be over-flagging.

---

### Anchor Text (`links-anchor-text`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a')].filter(a => {
  const text = a.textContent.trim().toLowerCase();
  return ['click here','read more','learn more','more','here','view','→','...'].includes(text) || text === '' || text.length < 3;
}).map(a => ({href: a.href, text: a.textContent.trim()}))
```
**Compare:** Count results vs tool's reported count. Also check for repeated anchor text pointing to different URLs:
```js
[...document.querySelectorAll('a')].reduce((acc, a) => {
  const t = a.textContent.trim().toLowerCase();
  if (!acc[t]) acc[t] = [];
  acc[t].push(a.href);
  return acc;
}, {})
```

---

### Link Depth (`links-depth`) | Severity: warn
**Note:** Crawl-mode rule. Cannot validate on single page.
**Action:** Note as "crawl-mode rule, requires site-wide check."

---

### HTTPS Downgrade (`links-https-downgrade`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href^="http:"]')].map(a => a.href)
```
**Compare:** If page is HTTPS and internal links use HTTP → tool correct. Count vs tool's count.

---

### External Count (`links-external-count`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href]')].filter(a => a.hostname && a.hostname !== window.location.hostname).length
```
**Compare:** Tool flags if >100. Compare actual count.

---

### Invalid Links (`links-invalid`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href=""], a[href="#"], a:not([href])')].map(a => ({text: a.textContent.trim(), tag: a.outerHTML.substring(0,100)}))
```
**Compare:** Count vs tool's reported count. Check if tool listed URL or just "Reason: empty".
**Note:** Tool should show location of invalid link, not just anchor text. Flag if URL missing from report.

---

### Tel & Mailto (`links-tel-mailto`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href^="tel:"], a[href^="mailto:"]')].map(a => ({href: a.href, text: a.textContent.trim()}))
```
**Compare:** Check for malformed tel/mailto (e.g. tel:+1 123 456 instead of tel:+11234567890).

---

### Localhost Links (`links-localhost`) | Severity: fail
**Console:**
```js
[...document.querySelectorAll('a[href]')].filter(a => a.href.includes('localhost') || a.href.includes('127.0.0.1')).map(a => a.href)
```
**Compare:** If results found → tool correct. Empty → false positive.

---

### Local File Links (`links-local-file`) | Severity: fail
**Console:**
```js
[...document.querySelectorAll('a[href^="file://"]')].map(a => a.href)
```
**Compare:** Results found → correct. Empty → false positive.

---

### Broken Fragments (`links-broken-fragment`) | Severity: warn
**Console:**
```js
const ids = new Set([...document.querySelectorAll('[id]')].map(el => el.id));
[...document.querySelectorAll('a[href^="#"]')].filter(a => {
  const anchor = a.getAttribute('href').slice(1);
  return anchor && !ids.has(anchor);
}).map(a => ({href: a.href, text: a.textContent.trim()}))
```
**Compare:** Count vs tool's count. Empty → false positive.

---

### Excessive Links (`links-excessive`) | Severity: warn
**Console:**
```js
console.log('Total links:', document.querySelectorAll('a').length, '| Visible links:', [...document.querySelectorAll('a')].filter(a => a.offsetParent !== null).length)
```
**Compare:** Tool flags if >100 total. Compare total and visible counts.
**Note:** Ask developer if rule counts total or visible links only.

---

### OnClick Navigation (`links-onclick`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[onclick], [onclick]:not(a)')].filter(el => el.getAttribute('onclick')?.includes('location') || el.getAttribute('onclick')?.includes('navigate')).length
```
**Compare:** Count vs tool's count.

---

### Whitespace Href (`links-whitespace-href`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href]')].filter(a => {
  const href = a.getAttribute('href');
  return href !== href.trim();
}).map(a => ({href: JSON.stringify(a.getAttribute('href')), text: a.textContent.trim()}))
```
**Compare:** Results found → correct. Empty → false positive.

---

### Orphan Pages (`links-orphan-pages`) | Severity: info
**Note:** Crawl-mode rule. Cannot validate on single page.

---

### Redirect Chains (`links-redirect-chains`) | Severity: warn/fail
**Note:** Requires fetching URLs. Use Redirect Path browser extension or:
```js
fetch('/some-path').then(r => console.log('Final URL:', r.url, '| Status:', r.status))
```
**Note:** Cannot fully validate via Console due to CORS.

---

### Broken Internal (`links-broken-internal`) | Severity: fail
**Note:** Requires crawl. Use SEO PowerSuite or Screaming Frog for validation.

---

### External Valid (`links-external-valid`) | Severity: warn
**Note:** Requires fetching external URLs. Cannot validate via Console (CORS blocks).

---

### Internal Links (`links-internal-present`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href]')].filter(a => a.hostname === window.location.hostname).length
```
**Compare:** 0 → tool correct. Has internal links → false positive.

---

### Dead-End Pages (`links-dead-end-pages`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[href]')].filter(a => a.hostname === window.location.hostname).length
```
**Compare:** 0 internal links → tool correct (page is dead-end). Has links → false positive.

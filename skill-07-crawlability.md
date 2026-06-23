---
name: seo-audit-validator-crawlability
description: "Validates Crawlability rules from SEO audit output. Use when audit contains any of: Indexability Conflict, Blocked Resources, Crawl Delay, Sitemap in Robots.txt, Pagination Canonical, Pagination Broken, Pagination Loop, Pagination Sequence, Pagination Noindex, Pagination Orphaned, Schema + Noindex, Sitemap Size Limit, Sitemap Duplicates, Sitemap Orphan URLs, Sitemap URL Limit, Sitemap Domain, Noindex in Sitemap, Canonical Redirect"
---

# SEO Audit Validator: Crawlability

## Rules

### Blocked Resources (`crawl-blocked-resources`) | Severity: warn
**Console:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => console.log(t))
```
**Compare:** Look for any Disallow rules targeting *.css, *.js, /assets/, /cdn/. If none found → false positive.

---

### Crawl Delay (`crawl-crawl-delay`) | Severity: info
**Console:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => {
  const delay = t.match(/crawl-delay:\s*(\d+)/i);
  console.log('Crawl-delay:', delay ? delay[1] + ' seconds' : 'Not set');
})
```
**Compare:** Present → correct info. Not set → false positive.

---

### Sitemap in Robots.txt (`crawl-sitemap-in-robotstxt`) | Severity: warn
**Console:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => {
  const sitemaps = t.match(/Sitemap:.+/gi);
  console.log('Sitemaps in robots.txt:', sitemaps || 'NONE FOUND');
})
```
**Compare:** None found → correct flag. Present → false positive.

---

### Indexability Conflict (`crawl-indexability-conflict`) | Severity: warn
**Console:**
```js
const robotsMeta = document.querySelector('meta[name="robots"]')?.getAttribute('content');
console.log('Robots meta:', robotsMeta);
fetch('/robots.txt').then(r => r.text()).then(t => {
  const path = window.location.pathname;
  const isDisallowed = t.includes('Disallow: ' + path) || t.includes('Disallow: /');
  console.log('Disallowed in robots.txt:', isDisallowed);
})
```
**Compare:** Page has noindex AND is blocked in robots.txt → correct (conflict). Only one → no conflict.

---

### Schema + Noindex (`crawl-schema-noindex-conflict`) | Severity: fail
**Console:**
```js
const noindex = document.querySelector('meta[name="robots"]')?.getAttribute('content')?.includes('noindex');
const hasSchema = document.querySelectorAll('script[type="application/ld+json"]').length > 0;
console.log('Has noindex:', noindex, '| Has schema:', hasSchema)
```
**Compare:** Both true → tool correct. Either false → false positive.

---

### Canonical Redirect (`crawl-canonical-redirect`) | Severity: warn/fail
**Console:**
```js
const canonical = document.querySelector('link[rel="canonical"]')?.href;
if (canonical) {
  fetch(canonical, {method: 'HEAD'}).then(r => {
    console.log('Canonical URL:', canonical);
    console.log('Final URL:', r.url);
    console.log('Redirects:', canonical !== r.url);
  });
}
```
**Compare:** If canonical URL redirects → correct flag.

---

### Pagination Canonical (`crawl-pagination-canonical`) | Severity: warn/fail
**Console:**
```js
const canonical = document.querySelector('link[rel="canonical"]')?.href;
const current = window.location.href;
console.log('Current URL:', current);
console.log('Canonical:', canonical);
console.log('Is self-referencing:', canonical === current || canonical === current.split('?')[0]);
```
**Compare:** Paginated page canonicalizing to page 1 (not self) → correct flag.

---

### Pagination Broken / Loop / Sequence / Noindex / Orphaned | Severity: varies
**Console (check pagination links):**
```js
[...document.querySelectorAll('a[rel="next"], a[rel="prev"], .pagination a, [class*="paginat"] a')].map(a => ({href: a.href, rel: a.getAttribute('rel'), text: a.textContent.trim()}))
```
**Note:** These rules require crawl-mode validation. Console only shows current page's pagination links.

---

### Sitemap Size Limit / URL Limit / Duplicates / Domain / Orphan URLs / Noindex in Sitemap | Severity: varies
**Note:** All require sitemap crawl. Cannot validate via Console on page.
**Action:** Mark as "sitemap-level rules, requires XML crawl." Use Screaming Frog or SEO PowerSuite.

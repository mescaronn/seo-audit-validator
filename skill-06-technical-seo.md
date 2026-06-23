---
name: seo-audit-validator-technical-seo
description: "Validates Technical SEO rules from SEO audit output. Use when audit contains any of: URL Structure, Trailing Slash, Timeout, Robots.txt Exists, Robots.txt Valid, Bad Content-Type, Sitemap Exists, Sitemap Valid, WWW Redirect, 404 Page, Non-404 Client Error, Soft 404, Server Error"
---

# SEO Audit Validator: Technical SEO

## Rules

### Robots.txt Exists (`technical-robots-txt-exists`) | Severity: warn
**Console:**
```js
fetch('/robots.txt').then(r => console.log('Status:', r.status))
```
**Compare:** 200 → false positive. 404 → correct flag.

---

### Robots.txt Valid (`technical-robots-txt-valid`) | Severity: warn
**Console:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => console.log(t))
```
**Compare:** Check for valid syntax (User-agent, Allow/Disallow, Sitemap). Look for typos or conflicting rules.

---

### Sitemap Exists (`technical-sitemap-exists`) | Severity: warn
**Console:**
```js
fetch('/sitemap.xml').then(r => console.log('Status:', r.status))
```
**Also try:**
```js
fetch('/robots.txt').then(r => r.text()).then(t => console.log('Sitemap in robots:', t.match(/Sitemap:.+/g)))
```
**Compare:** 200 on sitemap.xml → false positive. 404 → correct flag.

---

### Sitemap Valid (`technical-sitemap-valid`) | Severity: warn
**Console:**
```js
fetch('/sitemap.xml').then(r => r.text()).then(t => console.log(t.substring(0, 500)))
```
**Compare:** Check if valid XML with <urlset> or <sitemapindex> structure.

---

### URL Structure (`technical-url-structure`) | Severity: warn
**Console:**
```js
const path = window.location.pathname;
console.log('Path:', path);
console.log('Has uppercase:', path !== path.toLowerCase());
console.log('Has underscores:', path.includes('_'));
console.log('Has spaces (%20):', path.includes('%20'));
```
**Compare:** Any true values → tool correct.

---

### Trailing Slash (`technical-trailing-slash`) | Severity: warn
**Console:**
```js
window.location.pathname
```
**Check:** Consistent trailing slash usage. Also check if both /page and /page/ are accessible.

---

### WWW Redirect (`technical-www-redirect`) | Severity: warn
**Check:** Try both www and non-www versions of site. Use Redirect Path extension.
**Note:** Cannot validate via Console on live page.

---

### 404 Page (`technical-404-page`) | Severity: warn
**Console:**
```js
fetch('/this-page-does-not-exist-12345').then(r => {
  console.log('Status:', r.status, '| URL:', r.url);
  return r.text();
}).then(html => console.log('Has content:', html.length > 500))
```
**Compare:** Status 404 + has content → custom 404 exists. Returns 200 or no content → tool correct.

---

### Soft 404 (`technical-soft-404`) | Severity: warn
**Console:**
```js
fetch('/this-page-does-not-exist-12345').then(r => console.log('Status for non-existent page:', r.status))
```
**Compare:** Returns 200 for non-existent page → tool correct (soft 404). Returns 404 → false positive.

---

### Server Error (`technical-server-error`) | Severity: fail
**Check:** DevTools → Network tab → look for any 5xx responses.
**Console:**
```js
const responses = window.performance.getEntriesByType('resource').filter(r => r.responseStatus >= 500);
console.log('5xx errors:', responses.map(r => r.name))
```

---

### Non-404 Client Error (`technical-4xx-non-404`) | Severity: warn
**Check:** DevTools → Network tab → look for 403, 410, 451 responses.

---

### Timeout (`technical-timeout`) | Severity: fail
**Note:** If page loaded normally, no timeout issue. Check if specific resources timed out in Network tab.

---

### Bad Content-Type (`technical-bad-content-type`) | Severity: warn/fail
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('Content-Type:', r.headers.get('content-type')))
```
**Compare:** Should be 'text/html; charset=utf-8'. Missing charset or wrong type → correct flag.

---
name: seo-audit-validator-security
description: "Validates Security rules from SEO audit output. Use when audit contains any of: HTTPS, HTTPS Redirect, External Link Security, Form HTTPS, Mixed Content, Leaked Secrets, Password over HTTP, Protocol-Relative URLs, SSL Expiry, SSL Protocol, HSTS, CSP, X-Frame-Options, X-Content-Type, Permissions-Policy, Referrer-Policy"
---

# SEO Audit Validator: Security

## Rules

### HTTPS (`security-https`) | Severity: fail
**Console:**
```js
window.location.protocol
```
**Compare:** 'https:' → false positive. 'http:' → correct.

---

### HTTPS Redirect (`security-https-redirect`) | Severity: warn
**Check:** Navigate to http://[domain] and see if it redirects to https://. Use Redirect Path extension.
**Note:** Cannot validate via Console on HTTPS page.

---

### External Link Security (`security-external-links`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('a[target="_blank"]')].filter(a => {
  const rel = a.getAttribute('rel') || '';
  return !rel.includes('noopener') && !rel.includes('noreferrer');
}).map(a => a.href)
```
**Compare:** Count vs tool's count. Match flagged URLs.

---

### Form HTTPS (`security-form-https`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('form')].map(f => ({action: f.action, method: f.method, isHTTPS: f.action.startsWith('https')}))
```
**Compare:** Any form with action starting with 'http:' → correct flag.

---

### Mixed Content (`security-mixed-content`) | Severity: warn/fail
**Console:**
```js
[...document.querySelectorAll('[src^="http:"], [href^="http:"], [action^="http:"]')].map(el => ({tag: el.tagName, url: el.src || el.href || el.action}))
```
**Compare:** Count vs tool's count. Empty on HTTPS page → false positive.

---

### Leaked Secrets (`security-leaked-secrets`) | Severity: fail
**Console:**
```js
const html = document.documentElement.outerHTML;
const patterns = [/(?:api[_-]?key|apikey|secret|password|token)\s*[:=]\s*["'][^"']{10,}/gi];
patterns.flatMap(p => [...html.matchAll(p)]).map(m => m[0].substring(0, 50))
```
**Compare:** Results found → correct (critical). Empty → false positive.
**Note:** Basic check only. Tool may use more sophisticated detection.

---

### Password over HTTP (`security-password-http`) | Severity: fail
**Console:**
```js
window.location.protocol === 'http:' ? document.querySelectorAll('input[type="password"]').length + ' password fields on HTTP' : 'Page is HTTPS - no issue'
```

---

### Protocol-Relative URLs (`security-protocol-relative`) | Severity: warn
**Console:**
```js
document.documentElement.outerHTML.match(/['"]\/\/[^'"]+/g)?.length || 0
```
**Compare:** Count vs tool's reported count. Tool may undercount.

---

### SSL Expiry (`security-ssl-expiry`) | Severity: warn/fail
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => {
  const hsts = r.headers.get('strict-transport-security');
  console.log('HSTS:', hsts);
  const maxAge = hsts?.match(/max-age=(\d+)/)?.[1];
  if (maxAge) console.log('Days:', Math.round(parseInt(maxAge) / 86400));
})
```
**Compare:** Tool reports HSTS max-age in days. Convert seconds to days (÷86400).

---

### SSL Protocol (`security-ssl-protocol`) | Severity: warn/fail
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => {
  const hsts = r.headers.get('strict-transport-security');
  console.log('HSTS:', hsts);
  console.log('Has includeSubDomains:', hsts?.includes('includeSubDomains'));
  console.log('Has preload:', hsts?.includes('preload'));
})
```
**Compare:** If missing includeSubDomains or preload → correct flag.

---

### HSTS (`security-hsts`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('HSTS:', r.headers.get('strict-transport-security')))
```
**Compare:** null or missing → correct flag. Present → false positive.

---

### CSP (`security-csp`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('CSP:', r.headers.get('content-security-policy')))
```
**Compare:** null → correct flag. Present → false positive.

---

### X-Frame-Options (`security-x-frame-options`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('X-Frame-Options:', r.headers.get('x-frame-options')))
```
**Compare:** null → correct flag. DENY or SAMEORIGIN → false positive.

---

### X-Content-Type (`security-x-content-type-options`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('X-Content-Type-Options:', r.headers.get('x-content-type-options')))
```
**Compare:** null or not 'nosniff' → correct flag.

---

### Permissions-Policy (`security-permissions-policy`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('Permissions-Policy:', r.headers.get('permissions-policy')))
```
**Compare:** null → correct flag. Present → false positive.

---

### Referrer-Policy (`security-referrer-policy`) | Severity: warn
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('Referrer-Policy:', r.headers.get('referrer-policy')))
```
**Also check meta:**
```js
document.querySelector('meta[name="referrer"]')?.getAttribute('content')
```
**Compare:** Both null → correct flag.

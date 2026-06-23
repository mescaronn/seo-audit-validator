---
name: seo-audit-validator-redirects
description: "Validates Redirects rules from SEO audit output. Use when audit contains any of: Meta Refresh, JavaScript Redirect, HTTP Refresh Header, Resource Redirect, Case Normalization, Redirect Loop, Redirect Type, Broken Redirect"
---

# SEO Audit Validator: Redirects

## Rules

### Meta Refresh (`redirect-meta-refresh`) | Severity: warn
```js
document.querySelector('meta[http-equiv="refresh"]')?.getAttribute('content') || 'Not found'
```
**Compare:** Found → correct flag. Not found → false positive.

---

### JavaScript Redirect (`redirect-javascript`) | Severity: warn
```js
const scripts = [...document.querySelectorAll('script')].map(s => s.textContent).join(' ');
console.log('window.location redirect:', /window\.location\s*[=.]/.test(scripts));
console.log('location.replace redirect:', /location\.replace\s*\(/.test(scripts));
```
**Compare:** Either true → correct flag.

---

### HTTP Refresh Header (`redirect-http-refresh`) | Severity: warn
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('Refresh header:', r.headers.get('refresh')))
```
**Compare:** Not null → correct flag.

---

### Redirect Loop (`redirect-loop`) | Severity: fail
**Note:** Cannot detect via Console. Use Redirect Path extension.

---

### Redirect Type (`redirect-type`) | Severity: warn
**Note:** Requires checking HTTP response codes. Use Redirect Path extension or Network tab.

---

### Broken Redirect (`redirect-broken`) | Severity: fail
**Note:** Requires crawl-mode to detect broken redirect targets.

---

### Resource Redirect (`redirect-resource`) | Severity: warn
**Check:** DevTools → Network tab → look for CSS/JS/image resources with 301/302 status.

---

### Case Normalization (`redirect-case-normalization`) | Severity: warn
```js
fetch(window.location.origin + window.location.pathname.toUpperCase()).then(r => {
  console.log('Uppercase URL status:', r.status, '| Final URL:', r.url);
  console.log('Redirected to lowercase:', r.url === window.location.href);
})
```

---
name: seo-audit-validator-html-validation
description: "Validates HTML Validation rules from SEO audit output. Use when audit contains any of: Missing DOCTYPE, Missing Charset, Invalid Head, Noscript in Head, Multiple Heads, Size Limit, Lorem Ipsum, Multiple Titles, Multiple Descriptions"
---

# SEO Audit Validator: HTML Validation

## Rules

### Missing DOCTYPE (`htmlval-missing-doctype`) | Severity: warn
**Check:** View page source → first line should be `<!DOCTYPE html>`.
```js
document.doctype?.name === 'html' ? 'DOCTYPE present' : 'DOCTYPE MISSING'
```

---

### Missing Charset (`htmlval-missing-charset`) | Severity: warn
```js
document.querySelector('meta[charset]')?.getAttribute('charset') ||
document.querySelector('meta[http-equiv="Content-Type"]')?.getAttribute('content') ||
'Charset MISSING'
```

---

### Invalid Head (`htmlval-invalid-head`) | Severity: warn
```js
const validTags = ['META','TITLE','LINK','SCRIPT','STYLE','BASE','NOSCRIPT'];
[...document.head.children].filter(el => !validTags.includes(el.tagName)).map(el => el.tagName)
```
**Compare:** Any non-head tags found → correct flag.

---

### Noscript in Head (`htmlval-noscript-in-head`) | Severity: warn
```js
document.head.querySelector('noscript') ? 'Noscript FOUND in head' : 'No noscript in head'
```
**Compare:** Found → correct flag.

---

### Multiple Heads (`htmlval-multiple-heads`) | Severity: fail
```js
document.querySelectorAll('head').length
```
**Compare:** >1 → correct flag. 1 → false positive.

---

### Size Limit (`htmlval-size-limit`) | Severity: warn/fail
```js
console.log('HTML size:', (new Blob([document.documentElement.outerHTML]).size / 1024).toFixed(1) + ' KB')
```
**Compare:** Tool's reported size vs your measurement. Threshold: >500KB warn, >5MB fail.
**Note:** Your measurement via Blob includes rendered DOM (dynamic content). May differ from tool's raw HTML measurement.

---

### Lorem Ipsum (`htmlval-lorem-ipsum`) | Severity: warn
```js
document.body.innerText.includes('Lorem ipsum') ? 'Lorem ipsum FOUND' : 'No Lorem ipsum'
```
**Compare:** Found → correct flag.

---

### Multiple Titles (`htmlval-multiple-titles`) | Severity: fail
```js
document.querySelectorAll('title').length
```
**Compare:** >1 → correct flag. 1 → false positive.

---

### Multiple Descriptions (`htmlval-multiple-descriptions`) | Severity: fail
```js
document.querySelectorAll('meta[name="description"]').length
```
**Compare:** >1 → correct flag. 1 → false positive.

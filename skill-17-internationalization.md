---
name: seo-audit-validator-internationalization
description: "Validates Internationalization rules from SEO audit output. Use when audit contains any of: Lang Attribute, Hreflang Tags, Hreflang Return Links, Hreflang to Noindex, Hreflang to Non-Canonical, Hreflang to Broken, Hreflang to Redirect, Hreflang Conflicting, Hreflang Lang Mismatch, Hreflang Multiple Methods"
---

# SEO Audit Validator: Internationalization

## Rules

### Lang Attribute (`i18n-lang-attribute`) | Severity: fail
```js
document.documentElement.getAttribute('lang')
```
**Compare:** null or empty → correct flag. Valid BCP 47 code (e.g. 'en', 'en-US') → false positive.

---

### Hreflang Tags (`i18n-hreflang`) | Severity: warn/fail
```js
[...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l => ({lang: l.getAttribute('hreflang'), href: l.href}))
```
**Compare:** Empty on multi-language site → correct flag. Present → false positive.

---

### Hreflang Return Links (`i18n-hreflang-return-links`) | Severity: fail
**Note:** Requires fetching each hreflang target and checking if they link back. Cannot fully validate via Console.
**Partial check:**
```js
[...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l => l.href)
```
Then manually check each target page for reciprocal hreflang.

---

### Hreflang to Noindex (`i18n-hreflang-to-noindex`) | Severity: fail
**Note:** Requires fetching each hreflang target. Cannot validate via Console (CORS).

---

### Hreflang to Non-Canonical (`i18n-hreflang-to-non-canonical`) | Severity: warn
**Note:** Requires fetching each target's canonical. Cannot validate via Console.

---

### Hreflang to Broken / Redirect (`i18n-hreflang-to-broken` / `i18n-hreflang-to-redirect`) | Severity: fail/warn
**Note:** Requires fetching each hreflang target. Check manually or via crawl tool.

---

### Hreflang Conflicting (`i18n-hreflang-conflicting`) | Severity: fail
```js
const hreflangs = [...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l => l.getAttribute('hreflang'));
const dupLangs = hreflangs.filter((l, i) => hreflangs.indexOf(l) !== i);
console.log('Duplicate hreflang codes:', dupLangs)
```
**Compare:** Duplicate language codes → correct flag.

---

### Hreflang Lang Mismatch (`i18n-hreflang-lang-mismatch`) | Severity: warn
```js
const pageLang = document.documentElement.getAttribute('lang');
const hreflangs = [...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l => l.getAttribute('hreflang'));
console.log('Page lang:', pageLang, '| Hreflang includes page lang:', hreflangs.includes(pageLang))
```

---

### Hreflang Multiple Methods (`i18n-hreflang-multiple-methods`) | Severity: warn
```js
const htmlHreflang = [...document.querySelectorAll('link[rel="alternate"][hreflang]')].length;
console.log('HTML hreflang tags:', htmlHreflang);
fetch(window.location.href, {method:'HEAD'}).then(r => console.log('HTTP Link header:', r.headers.get('link')));
```
**Compare:** If both HTML tags AND HTTP header exist → correct flag (multiple methods).

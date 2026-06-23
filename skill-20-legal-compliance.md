---
name: seo-audit-validator-legal-compliance
description: "Validates Legal Compliance rules from SEO audit output. Use when audit contains: Cookie Consent"
---

# SEO Audit Validator: Legal Compliance

## Rules

### Cookie Consent (`legal-cookie-consent`) | Severity: pass/warn
```js
const cmpPlatforms = ['cookieyes','onetrust','cookiebot','termly','quantcast','usercentrics','didomi','iubenda'];
const html = document.documentElement.outerHTML.toLowerCase();
const found = cmpPlatforms.filter(p => html.includes(p));
console.log('CMP platforms found:', found.length ? found : 'None detected');
```
**Also check scripts:**
```js
[...document.querySelectorAll('script[src]')].filter(s => /cookie|consent|gdpr|privacy/i.test(s.src)).map(s => s.src)
```
**Compare:** CMP found → pass/false positive. No CMP but tracking scripts exist → correct warn.
**Also check for cookie banner visibility:**
```js
document.querySelector('[class*="cookie"], [class*="consent"], [id*="cookie"], [id*="consent"]')?.className
```

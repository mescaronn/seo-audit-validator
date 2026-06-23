---
name: seo-audit-validator-eeat
description: "Validates E-E-A-T rules from SEO audit output. Use when audit contains any of: About Page, Affiliate Disclosure, Author Byline, Author Expertise, Citations, Contact Page, Content Dates, Disclaimers, Editorial Policy, Physical Address, Privacy Policy, Terms of Service, Trust Signals, YMYL Detection"
---

# SEO Audit Validator: E-E-A-T

## Rules

### About Page (`eeat-about-page`) | Severity: warn
```js
[...document.querySelectorAll('a')].filter(a => /about/i.test(a.href) || /about/i.test(a.textContent)).map(a => ({href: a.href, text: a.textContent.trim()}))
```
**Compare:** Empty → correct flag. Link to about page found → false positive.

---

### Contact Page (`eeat-contact-page`) | Severity: warn
```js
[...document.querySelectorAll('a')].filter(a => /contact/i.test(a.href) || /contact/i.test(a.textContent)).map(a => ({href: a.href, text: a.textContent.trim()}))
```

---

### Privacy Policy (`eeat-privacy-policy`) | Severity: warn
```js
[...document.querySelectorAll('a')].filter(a => /privacy/i.test(a.href) || /privacy/i.test(a.textContent)).map(a => ({href: a.href, text: a.textContent.trim()}))
```

---

### Terms of Service (`eeat-terms-of-service`) | Severity: warn
```js
[...document.querySelectorAll('a')].filter(a => /terms|tos/i.test(a.href) || /terms|tos/i.test(a.textContent)).map(a => ({href: a.href, text: a.textContent.trim()}))
```

---

### Author Byline (`eeat-author-byline`) | Severity: warn
```js
document.querySelector('[class*="author"], [rel="author"], .byline, [itemprop="author"]')?.textContent.trim()
```
**Note:** Mainly relevant for blog/article pages.

---

### Author Expertise (`eeat-author-expertise`) | Severity: warn
**Check:** Look for author bio section with credentials. Elements → search for "bio" or "author" classes.

---

### Citations (`eeat-citations`) | Severity: warn
```js
[...document.querySelectorAll('a[href]')].filter(a => /\.gov|\.edu|ncbi\.nlm|pubmed/i.test(a.href)).map(a => ({href: a.href, text: a.textContent.trim()}))
```

---

### Content Dates (`eeat-content-dates`) | Severity: warn
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
const dates = schemas.filter(s => s.datePublished || s.dateModified).map(s => ({published: s.datePublished, modified: s.dateModified}));
console.log('Schema dates:', dates);
console.log('Time elements:', [...document.querySelectorAll('time')].map(t => t.getAttribute('datetime')));
```

---

### Physical Address (`eeat-physical-address`) | Severity: warn
```js
[...document.querySelectorAll('[itemprop="address"], address, [class*="address"]')].map(el => el.textContent.trim().substring(0, 100))
```

---

### Trust Signals (`eeat-trust-signals`) | Severity: warn
```js
[...document.querySelectorAll('[class*="review"], [class*="rating"], [class*="badge"], [class*="certif"], [class*="trust"]')].length
```

---

### Affiliate Disclosure (`eeat-affiliate-disclosure`) | Severity: warn
```js
document.body.innerText.match(/affiliate|sponsored|paid|commission/i) ? 'Disclosure text found' : 'No disclosure found'
```

---

### Editorial Policy (`eeat-editorial-policy`) | Severity: warn
```js
[...document.querySelectorAll('a')].filter(a => /editorial|policy/i.test(a.href) || /editorial.policy/i.test(a.textContent)).map(a => a.href)
```

---

### Disclaimers (`eeat-disclaimers`) | Severity: warn
```js
document.body.innerText.match(/disclaimer|not financial advice|not medical advice|consult a professional/i) ? 'Disclaimer found' : 'No disclaimer found'
```

---

### YMYL Detection (`eeat-ymyl-detection`) | Severity: info
**Note:** Tool auto-detects YMYL keywords (health, finance, legal). Info only — no action needed.

---
name: seo-audit-validator-structured-data
description: "Validates Structured Data rules from SEO audit output. Use when audit contains any of: Required Fields, Article Schema, Breadcrumb Schema, Schema Present, Schema Valid, Schema Type, Product Schema, Review Schema, Video Schema, LocalBusiness, Organization, WebSite Search, FAQ Schema"
---

# SEO Audit Validator: Structured Data

## Base Command (use first for all schema rules)
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const json = JSON.parse(s.textContent);
  return json['@graph'] ? json['@graph'] : [json];
});
console.log('Schema types found:', schemas.map(s => s['@type']));
```

## Rules

### Schema Present (`schema-present`) | Severity: warn
**Console:**
```js
document.querySelectorAll('script[type="application/ld+json"]').length
```
**Compare:** 0 → correct flag. >0 → false positive.

---

### Schema Valid (`schema-valid`) | Severity: fail
**Console:**
```js
[...document.querySelectorAll('script[type="application/ld+json"]')].map(s => {
  try { JSON.parse(s.textContent); return 'valid'; }
  catch(e) { return 'INVALID: ' + e.message; }
})
```
**Compare:** Any 'INVALID' → correct flag. All 'valid' → false positive.

---

### Schema Type (`schema-type`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'].map(n => n['@type']) : [j['@type']];
})
```
**Compare:** Any undefined/@type missing → correct flag.

---

### Required Fields (`schema-required-fields`) | Severity: warn
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.forEach(s => console.log(JSON.stringify(s, null, 2)));
```
**Compare:** Tool lists specific missing fields. Check each schema object for those fields.
**Note:** If tool doesn't list WHICH fields are missing → report as output issue (tool should specify).

---

### WebSite Search (`schema-website-search`) | Severity: info
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'WebSite').map(s => ({name: s.name, url: s.url, hasSearch: !!s.potentialAction}))
```
**Compare:** WebSite schema with name + url → false positive. Missing → correct flag.
**Note:** Tool may not detect @graph structure. Verify manually in Elements if console shows empty.

---

### Product Schema (`schema-product`) | Severity: fail
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'Product').map(s => ({name: s.name, hasOffers: !!s.offers, hasPrice: !!s.offers?.price}))
```
**Compare:** For product pages, missing Product schema → correct. Has schema with all required fields → false positive.

---

### Article Schema (`schema-article`) | Severity: warn
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => ['Article','BlogPosting','NewsArticle'].includes(s['@type'])).map(s => ({type: s['@type'], hasHeadline: !!s.headline, hasAuthor: !!s.author, hasDate: !!s.datePublished}))
```
**Compare:** For blog/article pages, check required fields.

---

### Breadcrumb Schema (`schema-breadcrumb`) | Severity: info
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'BreadcrumbList').map(s => s.itemListElement?.length + ' items')
```
**Compare:** Missing on non-homepage → correct flag. Present → false positive.

---

### FAQ Schema (`schema-faq`) | Severity: fail
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'FAQPage').map(s => ({questions: s.mainEntity?.length, hasAnswers: s.mainEntity?.every(q => q.acceptedAnswer?.text)}))
```
**Compare:** Missing on FAQ page → correct flag. Present with valid structure → false positive.

---

### LocalBusiness (`schema-local-business`) | Severity: warn
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'LocalBusiness' || s['@type']?.includes('Business')).map(s => ({name: s.name, hasAddress: !!s.address, hasPhone: !!s.telephone}))
```

---

### Organization (`schema-organization`) | Severity: info
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'Organization').map(s => ({name: s.name, hasLogo: !!s.logo, hasSameAs: !!s.sameAs}))
```

---

### Review Schema (`schema-review`) | Severity: warn
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => ['Review','AggregateRating'].includes(s['@type'])).map(s => s['@type'])
```

---

### Video Schema (`schema-video`) | Severity: warn
**Console:**
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.filter(s => s['@type'] === 'VideoObject').map(s => ({name: s.name, hasThumb: !!s.thumbnailUrl, hasDate: !!s.uploadDate}))
```

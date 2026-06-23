---
name: seo-audit-validator-ai-geo-readiness
description: "Validates AI/GEO Readiness rules from SEO audit output. Use when audit contains any of: Schema Drift, Semantic HTML, Content Structure, AI Bot Access, llms.txt"
---

# SEO Audit Validator: AI/GEO Readiness

## Rules

### llms.txt (`geo-llms-txt`) | Severity: info
```js
fetch('/llms.txt').then(r => console.log('llms.txt status:', r.status, '| URL:', r.url))
```
**Compare:** 200 → file exists → false positive. 404 → correct flag.

---

### AI Bot Access (`geo-ai-bot-access`) | Severity: warn
```js
fetch('/robots.txt').then(r => r.text()).then(t => {
  const blockedBots = ['GPTBot','Claude-Web','Anthropic','Google-Extended','CCBot','PerplexityBot'].filter(bot => t.includes('Disallow') && t.toLowerCase().includes(bot.toLowerCase()));
  console.log('Blocked AI bots:', blockedBots.length ? blockedBots : 'None blocked');
  console.log('Full robots.txt:', t);
})
```
**Compare:** Any AI bot blocked → correct flag. None blocked → false positive.

---

### Semantic HTML (`geo-semantic-html`) | Severity: warn
```js
const semanticTags = ['article','section','aside','nav','main','header','footer','figure','figcaption','time','mark','details','summary'];
const found = semanticTags.filter(tag => document.querySelector(tag));
console.log('Semantic tags found:', found, '| Missing:', semanticTags.filter(t => !found.includes(t)))
```
**Compare:** Few or no semantic tags → correct flag. Comprehensive use → false positive.

---

### Content Structure (`geo-content-structure`) | Severity: warn
```js
console.log('H1:', document.querySelector('h1')?.textContent.trim());
console.log('H2 count:', document.querySelectorAll('h2').length);
console.log('Has lists:', !!document.querySelector('ul, ol'));
console.log('Has tables:', !!document.querySelector('table'));
console.log('Paragraphs:', document.querySelectorAll('p').length);
```
**Compare:** Tool checks if content is well-structured for AI extraction. Missing H1, no headings, no lists = poorly structured.

---

### Schema Drift (`geo-schema-drift`) | Severity: warn
```js
const schemas = [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s => {
  const j = JSON.parse(s.textContent);
  return j['@graph'] ? j['@graph'] : [j];
});
schemas.forEach(s => {
  if (s['@type'] === 'Product') console.log('Product schema name:', s.name, '| Page H1:', document.querySelector('h1')?.textContent.trim());
  if (s['@type'] === 'Article') console.log('Article headline:', s.headline, '| Page H1:', document.querySelector('h1')?.textContent.trim());
});
```
**Compare:** Schema name/headline doesn't match page H1 or visible content → correct flag.

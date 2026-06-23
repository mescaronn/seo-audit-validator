---
name: seo-audit-validator-content
description: "Validates Content rules from SEO audit output. Use when audit contains any of: Word Count, Reading Level, Keyword Stuffing, Article Link Density, Broken HTML, Meta in Body, Heading Hierarchy, Heading Length, Heading Unique, Title Same as H1, Title Pixel Width, Description Pixel Width, Duplicate Description, MIME Type, Text/HTML Ratio, Exact Duplicate, Near Duplicate"
---

# SEO Audit Validator: Content

## Rules

### Word Count (`content-word-count`) | Severity: warn/fail
**Console:**
```js
const words = document.body.innerText.trim().split(/\s+/).filter(w => w.length > 0);
console.log('Word count:', words.length)
```
**Compare:** Tool flags <300 words (warn) or <100 (fail). Compare actual count.

---

### Reading Level (`content-reading-level`) | Severity: warn
**Note:** Flesch-Kincaid requires calculation. Console approximation:
```js
const text = document.body.innerText;
const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 0).length;
const words = text.trim().split(/\s+/).length;
const avgWordsPerSentence = words / sentences;
console.log('Avg words/sentence:', avgWordsPerSentence.toFixed(1), '| Target: <20 words/sentence')
```
**Compare:** >20 words/sentence on average → complex content. Tool's score needs Lighthouse or specific tool.

---

### Keyword Stuffing (`content-keyword-stuffing`) | Severity: warn/fail
**Console:**
```js
const words = document.body.innerText.toLowerCase().match(/\b\w+\b/g) || [];
const total = words.length;
const freq = {};
words.forEach(w => freq[w] = (freq[w]||0)+1);
Object.entries(freq).filter(([w,c]) => (c/total*100) > 2 && w.length > 3).sort((a,b)=>b[1]-a[1]).slice(0,10).map(([w,c]) => ({word: w, count: c, density: (c/total*100).toFixed(2)+'%'}))
```
**Compare:** Count matches, density percentages. Tool may use different total word count method.

---

### Article Link Density (`content-article-links`) | Severity: warn
**Console:**
```js
const textLength = document.body.innerText.length;
const links = document.querySelectorAll('a').length;
console.log('Links:', links, '| Text chars:', textLength, '| Ratio:', (links / textLength * 100).toFixed(2) + '%')
```

---

### Broken HTML (`content-broken-html`) | Severity: warn/fail
**Console (run all three):**
```js
// Duplicate IDs
const ids = [...document.querySelectorAll('[id]')].map(el => el.id).filter(id => id);
const dupIds = ids.filter((id, i) => ids.indexOf(id) !== i);
console.log('Duplicate IDs:', [...new Set(dupIds)]);
```
```js
// Invalid nesting (a > div)
console.log('a > div elements:', document.querySelectorAll('a > div').length);
```
```js
// Links missing href
console.log('Links missing href:', document.querySelectorAll('a:not([href])').length);
```
**Compare:** Sum up all issues. Tool reports total "problems" count. Verify each issue type.

---

### Meta in Body (`content-meta-in-body`) | Severity: fail
**Console:**
```js
[...document.body.querySelectorAll('meta, title, link[rel="canonical"]')].map(el => el.outerHTML.substring(0, 100))
```
**Compare:** Any results → correct flag. Empty → false positive.

---

### Heading Hierarchy (`content-heading-hierarchy`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('h1, h2, h3, h4, h5, h6')].slice(0, 10).map(h => ({tag: h.tagName, text: h.textContent.trim().substring(0, 50)}))
```
**Compare:** Check sequence — should go H1→H2→H3 without skipping. H2 before H1 → correct flag.

---

### Heading Length (`content-heading-length`) | Severity: warn
**Console:**
```js
[...document.querySelectorAll('h1, h2, h3, h4, h5, h6')].filter(h => h.textContent.trim().length < 10 || h.textContent.trim().length > 100).map(h => ({tag: h.tagName, text: h.textContent.trim(), length: h.textContent.trim().length}))
```
**Compare:** Count vs tool's count. Tool may use <10 chars as threshold.

---

### Heading Unique (`content-heading-unique`) | Severity: warn
**Console:**
```js
const hs = [...document.querySelectorAll('h1, h2, h3, h4, h5, h6')].map(h => h.textContent.trim().toLowerCase());
const dups = hs.filter((h,i) => hs.indexOf(h) !== i);
console.log('Duplicate heading instances:', dups.length, '| Unique duplicated headings:', [...new Set(dups)].length)
```
**Compare:** Tool reports count + list. Verify unique headings count matches.

---

### Title Same as H1 (`content-title-same-as-h1`) | Severity: warn
**Console:**
```js
const title = document.querySelector('title')?.textContent.trim().toLowerCase();
const h1 = document.querySelector('h1')?.textContent.trim().toLowerCase();
console.log('Title:', title, '| H1:', h1, '| Same:', title === h1)
```
**Compare:** Same → correct flag. Different → false positive.

---

### Title Pixel Width (`content-title-pixel-width`) | Severity: warn
**Console:**
```js
const title = document.querySelector('title')?.textContent.trim();
console.log('Title:', title, '| Length:', title?.length, '| Approx px:', Math.round(title?.length * 7))
```
**Compare:** Tool reports pixel width. >580px → flag. Check tool's calculation vs your estimate.

---

### Description Pixel Width (`content-description-pixel-width`) | Severity: warn
**Console:**
```js
const desc = document.querySelector('meta[name="description"]')?.getAttribute('content');
console.log('Description:', desc, '| Length:', desc?.length, '| Approx px:', Math.round(desc?.length * 7.5))
```
**Compare:** Tool reports pixel width. >920px → flag. Compare values.

---

### Duplicate Description (`content-duplicate-description`) | Severity: warn/fail
**Note:** Crawl-mode rule. Cannot validate on single page.

---

### MIME Type (`content-mime-type`) | Severity: warn/fail
**Console:**
```js
fetch(window.location.href, {method: 'HEAD'}).then(r => console.log('Content-Type:', r.headers.get('content-type')))
```
**Compare:** Should be 'text/html; charset=utf-8'. Missing charset → correct flag.

---

### Text/HTML Ratio (`content-text-html-ratio`) | Severity: warn
**Console:**
```js
const textLen = document.body.innerText.length;
const htmlLen = document.body.innerHTML.length;
console.log('Text:', textLen, '| HTML:', htmlLen, '| Ratio:', (textLen/htmlLen*100).toFixed(1)+'%')
```
**Compare:** Tool's ratio vs your calculation. <15% text is typically flagged.

---

### Exact Duplicate / Near Duplicate (`content-duplicate-exact` / `content-duplicate-near`) | Severity: fail/warn
**Note:** Crawl-mode rules. Cannot validate on single page.

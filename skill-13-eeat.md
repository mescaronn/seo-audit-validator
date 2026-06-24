# Skill 13: E-E-A-T

### About Page | `eeat-about-page` | warn
```js
console.log('AboutLink:', [...document.querySelectorAll('a')].filter(a=>/about/i.test(a.href)||/about/i.test(a.textContent)).length)
```

### Contact Page | `eeat-contact-page` | warn
```js
console.log('ContactLink:', [...document.querySelectorAll('a')].filter(a=>/contact/i.test(a.href)||/contact/i.test(a.textContent)).length)
```

### Privacy Policy | `eeat-privacy-policy` | warn
```js
console.log('PrivacyLink:', [...document.querySelectorAll('a')].filter(a=>/privacy/i.test(a.href)||/privacy/i.test(a.textContent)).length)
```

### Terms of Service | `eeat-terms-of-service` | warn
```js
console.log('TermsLink:', [...document.querySelectorAll('a')].filter(a=>/terms|tos/i.test(a.href)||/terms/i.test(a.textContent)).length)
```

### Author Byline | `eeat-author-byline` | warn
```js
console.log('AuthorByline:', document.querySelector('[class*="author"],[rel="author"],.byline,[itemprop="author"]')?1:0)
```

### Physical Address | `eeat-physical-address` | warn
```js
console.log('AddressElement:', document.querySelector('[itemprop="address"],address,[class*="address"]')?1:0)
```

### Trust Signals | `eeat-trust-signals` | warn
```js
console.log('TrustElements:', document.querySelectorAll('[class*="review"],[class*="rating"],[class*="badge"],[class*="certif"],[class*="trust"]').length)
```

### Content Dates | `eeat-content-dates` | warn
```js
console.log('TimeElements:', document.querySelectorAll('time[datetime]').length)
```

### Citations | `eeat-citations` | warn
```js
console.log('CitationLinks:', [...document.querySelectorAll('a[href]')].filter(a=>/\.gov|\.edu|ncbi\.nlm|pubmed/i.test(a.href)).length)
```

### Affiliate Disclosure | `eeat-affiliate-disclosure` | warn
```js
console.log('AffiliateText:', /affiliate|sponsored|paid|commission/i.test(document.body.innerText)?1:0)
```

### Disclaimers | `eeat-disclaimers` | warn
```js
console.log('DisclaimerText:', /disclaimer|not financial advice|not medical advice|consult a professional/i.test(document.body.innerText)?1:0)
```

### Editorial Policy | `eeat-editorial-policy` | warn
```js
console.log('EditorialLink:', [...document.querySelectorAll('a')].filter(a=>/editorial|policy/i.test(a.href)).length)
```

### YMYL Detection | `eeat-ymyl-detection` | info
```js
console.log('YMYLKeywords:', /health|medical|financial|legal|safety/i.test(document.body.innerText)?1:0)
```

### Author Expertise | `eeat-author-expertise` | warn
**CANNOT VALIDATE** - requires manual review of author bio content

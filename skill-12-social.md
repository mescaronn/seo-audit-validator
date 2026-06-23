---
name: seo-audit-validator-social
description: "Validates Social rules from SEO audit output. Use when audit contains any of: OG Title, OG Description, OG Image, OG Image Size, OG URL, Twitter Card, Share Buttons, Social Profiles, OG URL Canonical"
---

# SEO Audit Validator: Social

## Rules

### OG Title (`social-og-title`) | Severity: warn
```js
document.querySelector('meta[property="og:title"]')?.getAttribute('content') || 'MISSING'
```
**Compare:** MISSING → correct flag.

---

### OG Description (`social-og-description`) | Severity: warn
```js
document.querySelector('meta[property="og:description"]')?.getAttribute('content') || 'MISSING'
```

---

### OG Image (`social-og-image`) | Severity: warn
```js
document.querySelector('meta[property="og:image"]')?.getAttribute('content') || 'MISSING'
```

---

### OG Image Size (`social-og-image-size`) | Severity: warn
```js
const w = document.querySelector('meta[property="og:image:width"]')?.getAttribute('content');
const h = document.querySelector('meta[property="og:image:height"]')?.getAttribute('content');
console.log('og:image dimensions:', w + 'x' + h, '| Recommended: 1200x630')
```
**Compare:** Tool flags if below 1200x630. Even 1200x628 (2px off) gets flagged.

---

### OG URL (`social-og-url`) | Severity: warn
```js
document.querySelector('meta[property="og:url"]')?.getAttribute('content') || 'MISSING'
```

---

### OG URL Canonical (`social-og-url-canonical`) | Severity: fail
```js
const ogUrl = document.querySelector('meta[property="og:url"]')?.getAttribute('content');
const canonical = document.querySelector('link[rel="canonical"]')?.href;
console.log('og:url:', ogUrl, '| canonical:', canonical, '| Match:', ogUrl === canonical)
```

---

### Twitter Card (`social-twitter-card`) | Severity: warn
```js
document.querySelector('meta[name="twitter:card"]')?.getAttribute('content') || 'MISSING'
```

---

### Share Buttons (`social-share-buttons`) | Severity: warn
```js
[...document.querySelectorAll('a[href*="facebook.com/sharer"], a[href*="twitter.com/intent"], a[href*="linkedin.com/sharing"], a[href*="pinterest.com/pin"]')].map(a => a.href)
```
**Also check:** Look for social sharing widget class names in Elements.

---

### Social Profiles (`social-profiles`) | Severity: warn
```js
[...document.querySelectorAll('a[href*="facebook.com/"], a[href*="twitter.com/"], a[href*="instagram.com/"], a[href*="linkedin.com/"], a[href*="youtube.com/"], a[href*="threads.net/"]')].map(a => a.href)
```
**Compare:** 0-2 profiles → may flag. 3+ → false positive.

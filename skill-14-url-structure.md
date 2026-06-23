---
name: seo-audit-validator-url-structure
description: "Validates URL Structure rules from SEO audit output. Use when audit contains any of: Slug Keywords, Stop Words, Uppercase URLs, Underscores, Double Slashes, HTTP/HTTPS Duplicate, Spaces in URL, Non-ASCII, URL Length, Session IDs, Tracking Params, Internal Search, Repetitive Path, URL Parameters"
---

# SEO Audit Validator: URL Structure

## Base Command
```js
console.log('Full URL:', window.location.href, '| Path:', window.location.pathname, '| Query:', window.location.search)
```

## Rules

### Uppercase URLs (`url-uppercase`) | Severity: warn
```js
const path = window.location.pathname;
console.log('Has uppercase:', path !== path.toLowerCase(), '| Path:', path)
```

---

### Underscores (`url-underscores`) | Severity: warn
```js
console.log('Has underscores:', window.location.pathname.includes('_'), '| Path:', window.location.pathname)
```

---

### Double Slashes (`url-double-slash`) | Severity: warn
```js
console.log('Has double slash:', /\/\/(?!$)/.test(window.location.pathname))
```

---

### Spaces in URL (`url-spaces`) | Severity: fail
```js
console.log('Has %20:', window.location.href.includes('%20') || window.location.href.includes('+'))
```

---

### Non-ASCII (`url-non-ascii`) | Severity: warn
```js
console.log('Has non-ASCII:', /[^\x00-\x7F]/.test(window.location.href))
```

---

### URL Length (`url-length`) | Severity: warn
```js
console.log('URL length:', window.location.href.length, '| Path length:', window.location.pathname.length, '| Recommended path: <75 chars')
```

---

### Session IDs (`url-session-ids`) | Severity: fail
```js
const params = window.location.search;
console.log('Session params:', params.match(/(?:PHPSESSID|JSESSIONID|sessionid|sessid|sid=)[a-z0-9]+/i) || 'None found')
```

---

### Tracking Params (`url-tracking-params`) | Severity: warn
```js
const params = window.location.search;
console.log('Tracking params:', params.match(/utm_|fbclid|gclid|msclkid|mc_eid/i) ? 'FOUND: ' + params : 'None')
```

---

### Internal Search (`url-internal-search`) | Severity: warn
```js
console.log('Is search URL:', window.location.search.match(/[?&](?:q|query|search|s)=/i) ? 'Yes — should be noindexed' : 'No')
```

---

### Repetitive Path (`url-repetitive-path`) | Severity: warn
```js
const segments = window.location.pathname.split('/').filter(s => s);
const dupSegments = segments.filter((s, i) => segments.indexOf(s) !== i);
console.log('Duplicate path segments:', dupSegments)
```

---

### URL Parameters (`url-parameters`) | Severity: warn
```js
const params = new URLSearchParams(window.location.search);
console.log('Parameter count:', [...params.keys()].length, '| Params:', [...params.keys()])
```
**Compare:** >3 parameters typically flagged.

---

### Stop Words (`url-stop-words`) | Severity: warn
```js
const stopWords = ['the','a','an','and','or','but','in','on','at','to','for','of','with','by'];
const segments = window.location.pathname.split(/[-/]/).filter(s => s);
const found = segments.filter(s => stopWords.includes(s.toLowerCase()));
console.log('Stop words in URL:', found)
```

---

### Slug Keywords (`url-slug-keywords`) | Severity: fail/warn
**Check:** Is the URL path descriptive? Numeric IDs like `/p=123` or `/product-12345` → tool correct. Descriptive slugs → false positive.
```js
console.log('Path:', window.location.pathname, '| Is numeric:', /^\/[\d]+$/.test(window.location.pathname))
```

---

### HTTP/HTTPS Duplicate (`url-http-https-duplicate`) | Severity: warn
**Check:** Try http:// version of page. If both return 200 without redirect → tool correct.
**Note:** Requires Redirect Path extension or manual check.

# Skill 15: Redirects

### Meta Refresh | `redirect-meta-refresh` | warn
```js
console.log('MetaRefresh:', document.querySelector('meta[http-equiv="refresh"]')?1:0)
```

### JavaScript Redirect | `redirect-javascript` | warn
```js
const s=[...document.querySelectorAll('script')].map(s=>s.textContent).join(' ');console.log('WindowLocationRedirect:', /window\.location\s*[=.]/.test(s)?1:0, '| LocationReplace:', /location\.replace\s*\(/.test(s)?1:0)
```

### HTTP Refresh Header | `redirect-http-refresh` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > refresh

### Redirect Loop | `redirect-loop` | fail
**CANNOT VALIDATE** - use Redirect Path extension

### Redirect Type | `redirect-type` | warn
**CANNOT VALIDATE** - check Network tab > status codes (301 vs 302)

### Broken Redirect | `redirect-broken` | fail
**CANNOT VALIDATE** - requires crawl tool

### Resource Redirect | `redirect-resource` | warn
**CANNOT VALIDATE** - check Network tab > filter CSS/JS/images > look for 301/302

### Case Normalization | `redirect-case-normalization` | warn
**CANNOT VALIDATE** - navigate to uppercase version of URL and check if redirects

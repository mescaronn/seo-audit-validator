# Skill 07: Crawlability

### Blocked Resources | `crawl-blocked-resources` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t)) — look for Disallow rules on .css/.js

### Indexability Conflict | `crawl-indexability-conflict` | warn
```js
console.log('Noindex:', document.querySelector('meta[name="robots"]')?.getAttribute('content')?.includes('noindex')?1:0)
```

### Canonical Redirect | `crawl-canonical-redirect` | warn/fail
**CANNOT VALIDATE** - requires fetching canonical URL to check for redirect

### Crawl Delay | `crawl-crawl-delay` | info
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t)) — look for Crawl-delay

### Sitemap in Robots.txt | `crawl-sitemap-in-robotstxt` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t)) — look for Sitemap:

### Schema + Noindex | `crawl-schema-noindex-conflict` | fail
```js
console.log('HasSchema:', document.querySelectorAll('script[type="application/ld+json"]').length, '| HasNoindex:', document.querySelector('meta[name="robots"]')?.getAttribute('content')?.includes('noindex')?1:0)
```

### Pagination Canonical | `crawl-pagination-canonical` | warn/fail
```js
console.log('Canonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none', '| CurrentURL:', window.location.href)
```

### Sitemap Size Limit | `crawl-sitemap-size-limit` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Sitemap URL Limit | `crawl-sitemap-url-limit` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Sitemap Duplicates | `crawl-sitemap-duplicates` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Sitemap Domain | `crawl-sitemap-domain` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Noindex in Sitemap | `crawl-noindex-in-sitemap` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Sitemap Orphan URLs | `crawl-sitemap-orphan-urls` | warn
**CANNOT VALIDATE** - requires sitemap crawl

### Pagination Broken | `crawl-pagination-broken` | fail
**CANNOT VALIDATE** - requires crawl

### Pagination Loop | `crawl-pagination-loop` | fail
**CANNOT VALIDATE** - requires crawl

### Pagination Sequence | `crawl-pagination-sequence` | warn
**CANNOT VALIDATE** - requires crawl

### Pagination Noindex | `crawl-pagination-noindex` | warn
**CANNOT VALIDATE** - requires crawl

### Pagination Orphaned | `crawl-pagination-orphaned` | warn
**CANNOT VALIDATE** - requires crawl

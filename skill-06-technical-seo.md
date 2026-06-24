# Skill 06: Technical SEO

### Robots.txt Exists | `technical-robots-txt-exists` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>console.log('RobotsTxt:',r.status))

### Robots.txt Valid | `technical-robots-txt-valid` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t))

### Sitemap Exists | `technical-sitemap-exists` | warn
**CANNOT VALIDATE** - run: fetch('/sitemap.xml').then(r=>console.log('Sitemap:',r.status))

### Sitemap Valid | `technical-sitemap-valid` | warn
**CANNOT VALIDATE** - requires crawl tool

### URL Structure | `technical-url-structure` | warn
```js
console.log('HasUppercase:', window.location.pathname!==window.location.pathname.toLowerCase()?1:0, '| HasUnderscore:', window.location.pathname.includes('_')?1:0, '| HasSpaces:', window.location.href.includes('%20')?1:0)
```

### Trailing Slash | `technical-trailing-slash` | warn
```js
console.log('Path:', window.location.pathname)
```

### Bad Content-Type | `technical-bad-content-type` | warn/fail
**CANNOT VALIDATE** - check Network tab > Response Headers > content-type

### 404 Page | `technical-404-page` | warn
**CANNOT VALIDATE** - navigate to a non-existent URL and check status code

### Soft 404 | `technical-soft-404` | warn
**CANNOT VALIDATE** - run: fetch('/this-page-does-not-exist-xyz').then(r=>console.log('Status:',r.status))

### Server Error | `technical-server-error` | fail
**CANNOT VALIDATE** - check Network tab for any 5xx responses

### Non-404 Client Error | `technical-4xx-non-404` | warn
**CANNOT VALIDATE** - check Network tab for 403/410 responses

### WWW Redirect | `technical-www-redirect` | warn
**CANNOT VALIDATE** - navigate to www/non-www version manually

### Timeout | `technical-timeout` | fail
**CANNOT VALIDATE** - check Network tab for timed out requests

# Skill 03: Links

### Nofollow Usage | `links-nofollow-appropriate` | warn
```js
console.log('NofollowLinks:', document.querySelectorAll('a[rel*="nofollow"]').length)
```

### Anchor Text | `links-anchor-text` | warn
```js
console.log('NonDescriptiveAnchors:', [...document.querySelectorAll('a')].filter(a=>{const t=a.textContent.trim().toLowerCase();return['click here','read more','learn more','more','here','view','→','...'].includes(t)||t===''||t.length<3;}).length)
```

### Invalid Links | `links-invalid` | warn
```js
console.log('InvalidLinks:', document.querySelectorAll('a[href=""],a[href="#"],a:not([href])').length)
```

### Excessive Links | `links-excessive` | warn
```js
console.log('TotalLinks:', document.querySelectorAll('a').length, '| VisibleLinks:', [...document.querySelectorAll('a')].filter(a=>a.offsetParent!==null).length)
```

### External Link Security | `security-external-links` | warn
```js
console.log('UnsafeBlankLinks:', [...document.querySelectorAll('a[target="_blank"]')].filter(a=>{const r=a.getAttribute('rel')||'';return!r.includes('noopener')&&!r.includes('noreferrer');}).length)
```

### HTTPS Downgrade | `links-https-downgrade` | warn
```js
console.log('HTTPLinks:', [...document.querySelectorAll('a[href]')].filter(a=>a.getAttribute('href')?.startsWith('http:')).length)
```

### External Count | `links-external-count` | warn
```js
console.log('ExternalLinks:', [...document.querySelectorAll('a[href]')].filter(a=>a.hostname&&a.hostname!==window.location.hostname).length)
```

### Tel & Mailto | `links-tel-mailto` | warn
```js
console.log('TelLinks:', document.querySelectorAll('a[href^="tel:"]').length, '| MailtoLinks:', document.querySelectorAll('a[href^="mailto:"]').length)
```

### Localhost Links | `links-localhost` | fail
```js
console.log('LocalhostLinks:', [...document.querySelectorAll('a[href]')].filter(a=>a.href.includes('localhost')||a.href.includes('127.0.0.1')).length)
```

### Local File Links | `links-local-file` | fail
```js
console.log('FileLinks:', document.querySelectorAll('a[href^="file://"]').length)
```

### Broken Fragments | `links-broken-fragment` | warn
```js
const ids=new Set([...document.querySelectorAll('[id]')].map(el=>el.id));console.log('BrokenFragments:',[...document.querySelectorAll('a[href^="#"]')].filter(a=>{const anchor=a.getAttribute('href').slice(1);return anchor&&!ids.has(anchor);}).length)
```

### OnClick Navigation | `links-onclick` | warn
```js
console.log('OnClickNav:', [...document.querySelectorAll('a[onclick]')].filter(a=>(a.getAttribute('onclick')||'').includes('location')).length)
```

### Whitespace Href | `links-whitespace-href` | warn
```js
console.log('WhitespaceHref:', [...document.querySelectorAll('a[href]')].filter(a=>{const h=a.getAttribute('href');return h!==h.trim();}).length)
```

### Internal Links | `links-internal-present` | warn
```js
console.log('InternalLinks:', [...document.querySelectorAll('a[href]')].filter(a=>a.hostname===window.location.hostname).length)
```

### Orphan Pages | `links-orphan-pages` | warn
**CANNOT VALIDATE** - requires site crawl

### Redirect Chains | `links-redirect-chains` | warn/fail
**CANNOT VALIDATE** - requires Redirect Path extension or crawl tool

### Broken Internal | `links-broken-internal` | fail
**CANNOT VALIDATE** - requires crawl tool

### External Valid | `links-external-valid` | warn
**CANNOT VALIDATE** - requires fetching external URLs (CORS blocks console)

### Dead-End Pages | `links-dead-end-pages` | warn
**CANNOT VALIDATE** - requires site crawl

### Link Depth | `links-depth` | warn
**CANNOT VALIDATE** - requires site crawl

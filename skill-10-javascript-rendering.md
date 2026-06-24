# Skill 10: JavaScript Rendering

### JS Blocked Resources | `js-blocked-resources` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t)) — look for Disallow on .js files

### SSR Check | `js-ssr-check` | warn/fail
```js
console.log('RenderedTextLen:', document.body.innerText.length)
```
**Also check** - View Page Source (Ctrl+U) and compare text length to rendered

### JS Rendered Title | `js-rendered-title` | fail
```js
console.log('RenderedTitle:', document.querySelector('title')?.textContent ?? 'none')
```
**Also check** - View Page Source > search for <title>

### JS Rendered Description | `js-rendered-description` | warn
```js
console.log('RenderedDesc:', document.querySelector('meta[name="description"]')?.getAttribute('content') ?? 'none')
```

### JS Rendered H1 | `js-rendered-h1` | fail
```js
console.log('RenderedH1:', document.querySelector('h1')?.textContent.trim() ?? 'none')
```

### JS Rendered Canonical | `js-rendered-canonical` | fail
```js
console.log('RenderedCanonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none')
```

### JS Canonical Mismatch | `js-canonical-mismatch` | fail
```js
console.log('RenderedCanonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none')
```
**Also check** - View Page Source > search for rel="canonical"

### JS Noindex Mismatch | `js-noindex-mismatch` | fail
```js
console.log('RenderedRobots:', document.querySelector('meta[name="robots"]')?.getAttribute('content') ?? 'none')
```
**Also check** - View Page Source > search for meta name="robots"

### JS Title Modified | `js-title-modified` | warn
```js
console.log('RenderedTitle:', document.querySelector('title')?.textContent ?? 'none')
```
**Also check** - View Page Source > compare title

### JS Description Modified | `js-description-modified` | warn
```js
console.log('RenderedDesc:', document.querySelector('meta[name="description"]')?.getAttribute('content') ?? 'none')
```

### JS H1 Modified | `js-h1-modified` | warn
```js
console.log('RenderedH1:', document.querySelector('h1')?.textContent.trim() ?? 'none')
```

### JS Rendered Content | `js-rendered-content` | warn
```js
console.log('RenderedTextLen:', document.body.innerText.length)
```
**Also check** - View Page Source > compare content length

### JS Rendered Links | `js-rendered-links` | warn
```js
console.log('RenderedInternalLinks:', [...document.querySelectorAll('a[href]')].filter(a=>a.hostname===window.location.hostname).length)
```

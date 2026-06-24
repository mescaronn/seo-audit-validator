# Skill 01: Core SEO

### Title Present | `core-title-present` | fail
```js
console.log('TitlePresent:', document.querySelector('title') ? 1 : 0)
```

### Title Length | `core-title-length` | warn
```js
console.log('TitleLength:', document.querySelector('title')?.textContent?.trim().length ?? 0)
```

### Description Present | `core-description-present` | fail
```js
console.log('DescPresent:', document.querySelector('meta[name="description"]') ? 1 : 0)
```

### Description Length | `core-description-length` | warn
```js
console.log('DescLength:', document.querySelector('meta[name="description"]')?.getAttribute('content')?.length ?? 0)
```

### Favicon Present | `core-favicon-present` | warn
```js
console.log('FaviconPresent:', document.querySelectorAll('link[rel="icon"],link[rel="shortcut icon"],link[rel="apple-touch-icon"]').length)
```

### H1 Present | `core-h1-present` | fail
```js
console.log('H1Present:', document.querySelectorAll('h1').length)
```

### H1 Single | `core-h1-single` | warn
```js
console.log('H1Count:', document.querySelectorAll('h1').length)
```

### Nosnippet Directive | `core-nosnippet` | warn
```js
console.log('RobotsMeta:', document.querySelector('meta[name="robots"]')?.getAttribute('content') ?? 'none')
```

### Robots Meta | `core-robots-meta` | warn
```js
console.log('RobotsMeta:', document.querySelector('meta[name="robots"]')?.getAttribute('content') ?? 'none')
```

### Viewport Present | `core-viewport-present` | fail
```js
console.log('ViewportPresent:', document.querySelector('meta[name="viewport"]') ? 1 : 0)
```

### Canonical Present | `core-canonical-present` | fail
```js
console.log('CanonicalPresent:', document.querySelector('link[rel="canonical"]') ? 1 : 0)
```

### Canonical Valid | `core-canonical-valid` | warn
```js
console.log('CanonicalURL:', document.querySelector('link[rel="canonical"]')?.href ?? 'none')
```

### Conflicting Canonicals | `core-canonical-conflicting` | fail
```js
console.log('CanonicalURL:', document.querySelector('link[rel="canonical"]')?.href ?? 'none')
```

### Canonical to Homepage | `core-canonical-to-homepage` | warn
```js
console.log('Canonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none', '| Homepage:', window.location.origin + '/')
```

### Canonical HTTP Mismatch | `core-canonical-http-mismatch` | warn
```js
console.log('PageProtocol:', window.location.protocol, '| CanonicalProtocol:', (document.querySelector('link[rel="canonical"]')?.href ?? '').split(':')[0] + ':')
```

### Canonical Loop | `core-canonical-loop` | fail
```js
console.log('Canonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none', '| CurrentURL:', window.location.href)
```

### Canonical to Noindex | `core-canonical-to-noindex` | fail
```js
console.log('Canonical:', document.querySelector('link[rel="canonical"]')?.href ?? 'none', '| Noindex:', document.querySelector('meta[name="robots"]')?.getAttribute('content')?.includes('noindex') ? 1 : 0)
```

### Canonical Header | `core-canonical-header` | warn
**CANNOT VALIDATE** - requires HTTP header check via Network tab

### Title Uniqueness | `core-title-unique` | warn
**CANNOT VALIDATE** - requires site crawl

# Skill 02: Performance

### DOM Size | `perf-dom-size` | warn/fail
```js
console.log('DOMNodes:', document.querySelectorAll('*').length, '| HeadChildren:', document.head.children.length)
```

### Lazy Above Fold | `perf-lazy-above-fold` | warn/fail
```js
console.log('AboveFoldLazy:', [...document.querySelectorAll('img')].filter(img=>{const r=img.getBoundingClientRect();return r.top<window.innerHeight&&r.width>100&&img.getAttribute('loading')==='lazy';}).length)
```

### LCP | `cwv-lcp` | warn/fail
**CANNOT VALIDATE** - use Lighthouse

### CLS | `cwv-cls` | warn/fail
**CANNOT VALIDATE** - use Lighthouse

### INP | `cwv-inp` | warn/fail
**CANNOT VALIDATE** - use Lighthouse

### TTFB | `cwv-ttfb` | warn/fail
**CANNOT VALIDATE** - use Lighthouse

### FCP | `cwv-fcp` | warn/fail
**CANNOT VALIDATE** - use Lighthouse

### Render-Blocking | `perf-render-blocking` | warn/fail
```js
console.log('RenderBlocking:', [...document.querySelectorAll('head script:not([async]):not([defer])')].filter(s=>s.src).length)
```

### Preconnect Hints | `perf-preconnect` | warn
```js
console.log('PreconnectCount:', document.querySelectorAll('link[rel="preconnect"],link[rel="dns-prefetch"]').length)
```

### LCP Hints | `perf-lcp-hints` | warn/fail
```js
console.log('PreloadImages:', document.querySelectorAll('link[rel="preload"][as="image"]').length, '| FetchPriorityHigh:', document.querySelectorAll('img[fetchpriority="high"]').length)
```

### Font Loading | `perf-font-loading` | warn/fail
```js
console.log('PreloadFonts:', document.querySelectorAll('link[rel="preload"][as="font"]').length)
```

### Video for Animations | `perf-video-for-animations` | warn
```js
console.log('GIFCount:', [...document.querySelectorAll('img')].filter(img=>img.src.toLowerCase().includes('.gif')).length)
```

### Text Compression | `perf-text-compression` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > content-encoding

### Brotli Compression | `perf-brotli` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > content-encoding: br

### Cache Policy | `perf-cache-policy` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > cache-control

### Minify CSS | `perf-minify-css` | warn
**CANNOT VALIDATE** - check Network tab > CSS files > Preview

### Minify JS | `perf-minify-js` | warn
**CANNOT VALIDATE** - check Network tab > JS files > Preview

### Response Time | `perf-response-time` | warn/fail
**CANNOT VALIDATE** - check Network tab > HTML document > Timing > TTFB

### HTTP/2 | `perf-http2` | warn
**CANNOT VALIDATE** - check Network tab > Protocol column

### Page Weight | `perf-page-weight` | warn/fail
**CANNOT VALIDATE** - check Network tab > Total transferred size

### JS File Size | `perf-js-file-size` | warn/fail
**CANNOT VALIDATE** - check Network tab > JS filter > Size column

### CSS File Size | `perf-css-file-size` | warn/fail
**CANNOT VALIDATE** - check Network tab > CSS filter > Size column

# Skill 16: Mobile

### Viewport Width | `mobile-viewport-width` | warn
```js
console.log('Viewport:', document.querySelector('meta[name="viewport"]')?.getAttribute('content')??'none')
```

### Multiple Viewports | `mobile-multiple-viewports` | fail
```js
console.log('ViewportCount:', document.querySelectorAll('meta[name="viewport"]').length)
```

### Font Size | `mobile-font-size` | warn/fail
```js
console.log('BodyFontSize:', parseInt(getComputedStyle(document.body).fontSize), '| SmallTextCount:', [...document.querySelectorAll('p,span,li,td')].filter(el=>parseInt(getComputedStyle(el).fontSize)<12).length)
```

### Interstitials | `mobile-interstitials` | warn/fail
```js
console.log('VisibleOverlays:', [...document.querySelectorAll('div[class*="overlay"],div[class*="modal"],div[class*="popup"]')].filter(el=>{const s=getComputedStyle(el);return s.display!=='none'&&s.visibility!=='hidden';}).length, '| HighZIndex:', [...document.querySelectorAll('div[class*="overlay"],div[class*="modal"],div[class*="popup"]')].filter(el=>{const s=getComputedStyle(el);return s.display!=='none'&&parseInt(s.zIndex)>100;}).length)
```

### Horizontal Scroll | `mobile-horizontal-scroll` | fail/warn
```js
console.log('HorizontalScroll:', document.documentElement.scrollWidth>window.innerWidth?1:0, '| PageWidth:', document.documentElement.scrollWidth, '| WindowWidth:', window.innerWidth)
```

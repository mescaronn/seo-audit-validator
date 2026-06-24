# Skill 11: Accessibility

### Heading Order | `a11y-heading-order` | warn/fail
```js
const hs=[...document.querySelectorAll('h1,h2,h3,h4,h5,h6')];console.log('FirstHeading:', hs[0]?.tagName??'none', '| H1Count:', document.querySelectorAll('h1').length)
```

### Color Contrast | `a11y-color-contrast` | warn
```js
console.log('WhiteTextElements:', [...document.querySelectorAll('*')].filter(el=>getComputedStyle(el).color==='rgb(255, 255, 255)').length)
```

### Video Captions | `a11y-video-captions` | warn/fail
```js
console.log('VideoCount:', document.querySelectorAll('video,audio').length, '| WithoutTracks:', [...document.querySelectorAll('video,audio')].filter(el=>el.querySelectorAll('track[kind="captions"],track[kind="subtitles"]').length===0).length)
```

### Landmarks | `a11y-landmark-regions` | warn
```js
console.log('Header:', document.querySelectorAll('header,[role="banner"]').length, '| Nav:', document.querySelectorAll('nav,[role="navigation"]').length, '| Main:', document.querySelectorAll('main,[role="main"]').length, '| Footer:', document.querySelectorAll('footer,[role="contentinfo"]').length)
```

### Link Text | `a11y-link-text` | warn/fail
```js
const counts={};[...document.querySelectorAll('a')].forEach(a=>{const t=a.textContent.trim().toLowerCase();if(['click here','read more','learn more','view','more','here'].includes(t))counts[t]=(counts[t]||0)+1;});console.log('GenericLinkText:', JSON.stringify(counts))
```

### Touch Targets | `a11y-touch-targets` | warn
```js
console.log('SmallTargets:', [...document.querySelectorAll('a,button,[role="button"]')].filter(el=>{const r=el.getBoundingClientRect();return(r.width<44||r.height<44)&&r.width>0;}).length)
```

### ARIA Labels | `a11y-aria-labels` | warn/fail
```js
console.log('MissingAriaLabel:', [...document.querySelectorAll('button,[role="button"],input,select,textarea')].filter(el=>!el.getAttribute('aria-label')&&!el.getAttribute('aria-labelledby')&&!el.textContent.trim()).length)
```

### Form Labels | `a11y-form-labels` | warn/fail
```js
console.log('UnlabeledInputs:', [...document.querySelectorAll('input:not([type="hidden"]):not([type="submit"]):not([type="button"]):not([type="reset"])')].filter(input=>!input.getAttribute('aria-label')&&!input.getAttribute('aria-labelledby')&&!(input.id&&document.querySelector('label[for="'+input.id+'"]'))).length)
```

### Skip Link | `a11y-skip-link` | warn
```js
console.log('SkipLink:', document.querySelector('a[href="#main"],a[href="#content"],a[href="#maincontent"],.skip-link')?1:0)
```

### Table Headers | `a11y-table-headers` | warn/fail
```js
console.log('TablesTotal:', document.querySelectorAll('table').length, '| TablesNoTH:', [...document.querySelectorAll('table')].filter(t=>!t.querySelector('th')).length)
```

### Zoom Disabled | `a11y-zoom-disabled` | fail
```js
const v=document.querySelector('meta[name="viewport"]')?.getAttribute('content')??'';console.log('ZoomDisabled:', (v.includes('user-scalable=no')||v.includes('maximum-scale=1'))?1:0)
```

### Focus Visible | `a11y-focus-visible` | warn/fail
**CANNOT VALIDATE** - DevTools > Elements > :hov > :focus > check outline on links/buttons

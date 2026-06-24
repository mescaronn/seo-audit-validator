# Skill 04: Images

### Alt Present | `images-alt-present` | fail
```js
console.log('ImgsWithoutAlt:', document.querySelectorAll('img:not([alt])').length)
```

### Alt Quality | `images-alt-quality` | warn
```js
console.log('PoorAltCount:', [...document.querySelectorAll('img[alt]')].filter(img=>{const a=img.alt.trim().toLowerCase();return['image','photo','picture','img','icon','banner','logo'].includes(a)||a.length<5;}).length)
```

### Alt Length | `images-alt-length` | warn
```js
console.log('LongAlt:', [...document.querySelectorAll('img[alt]')].filter(img=>img.alt.length>125).length)
```

### Dimensions | `images-dimensions` | warn
```js
console.log('MissingDimensions:', [...document.querySelectorAll('img')].filter(img=>!img.getAttribute('width')||!img.getAttribute('height')).length)
```

### Lazy Loading | `images-lazy-loading` | warn
```js
console.log('TotalImgs:', document.querySelectorAll('img').length, '| NoLazy:', [...document.querySelectorAll('img')].filter(img=>!img.getAttribute('loading')).length, '| BelowFoldNoLazy:', [...document.querySelectorAll('img')].filter(img=>{const r=img.getBoundingClientRect();return r.top>window.innerHeight&&!img.getAttribute('loading');}).length)
```

### Modern Format | `images-modern-format` | warn
```js
console.log('TotalImgs:', document.querySelectorAll('img').length, '| WebP:', [...document.querySelectorAll('img')].filter(img=>img.src.includes('.webp')).length, '| AVIF:', [...document.querySelectorAll('img')].filter(img=>img.src.includes('.avif')).length, '| JPG:', [...document.querySelectorAll('img')].filter(img=>img.src.match(/\.jpe?g/i)).length, '| PNG:', [...document.querySelectorAll('img')].filter(img=>img.src.includes('.png')).length, '| SVG:', [...document.querySelectorAll('img')].filter(img=>img.src.includes('.svg')).length)
```

### Responsive | `images-responsive` | warn
```js
console.log('NonResponsive:', [...document.querySelectorAll('img')].filter(img=>!img.hasAttribute('srcset')&&!img.closest('picture')).length)
```

### Picture Element | `images-picture-element` | fail
```js
console.log('PictureTotal:', document.querySelectorAll('picture').length, '| PictureNoSource:', [...document.querySelectorAll('picture')].filter(pic=>pic.querySelectorAll('source').length===0).length, '| PictureNoImg:', [...document.querySelectorAll('picture')].filter(pic=>!pic.querySelector('img')).length)
```

### Inline SVG Size | `images-inline-svg-size` | warn
```js
console.log('LargeSVGs:', [...document.querySelectorAll('svg')].filter(svg=>new XMLSerializer().serializeToString(svg).length/1024>5).length)
```

### Broken Images | `images-broken` | fail
```js
console.log('BrokenImgs:', [...document.querySelectorAll('img')].filter(img=>!img.complete||img.naturalWidth===0).length)
```

### Figure Captions | `images-figure-captions` | warn
```js
console.log('FiguresWithoutCaption:', [...document.querySelectorAll('figure')].filter(fig=>!fig.querySelector('figcaption')).length)
```

### Filename Quality | `images-filename-quality` | warn
```js
console.log('BadFilenames:', [...document.querySelectorAll('img')].filter(img=>/^(img|image|dsc|photo|screenshot|pic|banner|_)[\d_-]/i.test(img.src.split('/').pop().split('?')[0])).length)
```

### Background Image SEO | `images-background-seo` | warn
```js
console.log('CSSBackgroundImgs:', [...document.querySelectorAll('[style*="background-image"]')].length)
```

### Size | `images-size` | warn
**CANNOT VALIDATE** - check Network tab > Img filter > Size column > count rows >200KB

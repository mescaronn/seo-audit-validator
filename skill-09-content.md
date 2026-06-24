# Skill 09: Content

### Word Count | `content-word-count` | warn/fail
```js
console.log('WordCount:', document.body.innerText.trim().split(/\s+/).filter(w=>w.length>0).length)
```

### Keyword Stuffing | `content-keyword-stuffing` | warn/fail
```js
const words=document.body.innerText.toLowerCase().match(/\b\w+\b/g)||[];const total=words.length;const freq={};words.forEach(w=>freq[w]=(freq[w]||0)+1);const over=Object.entries(freq).filter(([w,c])=>c/total*100>2&&w.length>3);console.log('TotalWords:', total, '| KeywordsOver2pct:', over.length)
```

### Broken HTML | `content-broken-html` | warn/fail
```js
const ids=[...document.querySelectorAll('[id]')].map(el=>el.id).filter(id=>id);const dupIds=ids.filter((id,i)=>ids.indexOf(id)!==i);console.log('DuplicateIDs:', [...new Set(dupIds)].length, '| AinDiv:', document.querySelectorAll('a > div').length, '| LinksNoHref:', document.querySelectorAll('a:not([href])').length)
```

### Meta in Body | `content-meta-in-body` | fail
```js
console.log('MetaInBody:', document.body.querySelectorAll('meta,title,link[rel="canonical"]').length)
```

### Heading Hierarchy | `content-heading-hierarchy` | warn
```js
const hs=[...document.querySelectorAll('h1,h2,h3,h4,h5,h6')].map(h=>parseInt(h.tagName[1]));let skips=0;for(let i=1;i<hs.length;i++){if(hs[i]-hs[i-1]>1)skips++;}console.log('HeadingSkips:', skips, '| FirstHeading:', 'H'+hs[0])
```

### Heading Length | `content-heading-length` | warn
```js
console.log('ShortHeadings:', [...document.querySelectorAll('h1,h2,h3,h4,h5,h6')].filter(h=>h.textContent.trim().length<10).length, '| LongHeadings:', [...document.querySelectorAll('h1,h2,h3,h4,h5,h6')].filter(h=>h.textContent.trim().length>100).length)
```

### Heading Unique | `content-heading-unique` | warn
```js
const hs=[...document.querySelectorAll('h1,h2,h3,h4,h5,h6')].map(h=>h.textContent.trim().toLowerCase());const dups=hs.filter((h,i)=>hs.indexOf(h)!==i);console.log('DupInstances:', dups.length, '| UniqueDups:', [...new Set(dups)].length)
```

### Title Same as H1 | `content-title-same-as-h1` | warn
```js
console.log('TitleSameAsH1:', document.querySelector('title')?.textContent.trim().toLowerCase()===document.querySelector('h1')?.textContent.trim().toLowerCase()?1:0)
```

### Title Pixel Width | `content-title-pixel-width` | warn
```js
console.log('TitleLength:', document.querySelector('title')?.textContent.trim().length??0, '| ApproxPx:', Math.round((document.querySelector('title')?.textContent.trim().length??0)*7))
```

### Description Pixel Width | `content-description-pixel-width` | warn
```js
console.log('DescLength:', document.querySelector('meta[name="description"]')?.getAttribute('content')?.length??0, '| ApproxPx:', Math.round((document.querySelector('meta[name="description"]')?.getAttribute('content')?.length??0)*7.5))
```

### Text/HTML Ratio | `content-text-html-ratio` | warn
```js
console.log('TextLen:', document.body.innerText.length, '| HTMLLen:', document.body.innerHTML.length, '| RatioPct:', Math.round(document.body.innerText.length/document.body.innerHTML.length*100))
```

### Lorem Ipsum | `content-lorem-ipsum` | warn
```js
console.log('LoremIpsum:', document.body.innerText.includes('Lorem ipsum')?1:0)
```

### Article Link Density | `content-article-links` | warn
```js
console.log('LinkCount:', document.querySelectorAll('a').length, '| TextLength:', document.body.innerText.length)
```

### MIME Type | `content-mime-type` | warn/fail
**CANNOT VALIDATE** - check Network tab > Response Headers > content-type

### Reading Level | `content-reading-level` | warn
**CANNOT VALIDATE** - requires text analysis tool

### Duplicate Description | `content-duplicate-description` | warn/fail
**CANNOT VALIDATE** - requires site crawl

### Exact Duplicate | `content-duplicate-exact` | fail
**CANNOT VALIDATE** - requires site crawl

### Near Duplicate | `content-duplicate-near` | warn
**CANNOT VALIDATE** - requires site crawl

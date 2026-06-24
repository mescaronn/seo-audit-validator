# Skill 08: Structured Data

### Schema Present | `schema-present` | warn
```js
console.log('SchemaCount:', document.querySelectorAll('script[type="application/ld+json"]').length)
```

### Schema Valid | `schema-valid` | fail
```js
console.log('InvalidSchemas:', [...document.querySelectorAll('script[type="application/ld+json"]')].filter(s=>{try{JSON.parse(s.textContent);return false;}catch(e){return true;}}).length)
```

### Schema Type | `schema-type` | warn
```js
console.log('SchemaTypes:', [...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph'].map(n=>n['@type']):[j['@type']];}).join(','))
```

### Required Fields | `schema-required-fields` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});console.log('SchemaTypes:',s.map(x=>x['@type']).join(','),'| Names:',s.map(x=>x.name||x.headline||'no-name').join(','))
```

### WebSite Search | `schema-website-search` | info
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const ws=s.filter(x=>x['@type']==='WebSite');console.log('WebSiteSchema:', ws.length, '| HasName:', ws[0]?.name?1:0, '| HasURL:', ws[0]?.url?1:0)
```

### Product Schema | `schema-product` | fail
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const p=s.filter(x=>x['@type']==='Product');console.log('ProductSchema:', p.length, '| HasOffers:', p[0]?.offers?1:0, '| HasPrice:', p[0]?.offers?.price?1:0)
```

### Article Schema | `schema-article` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const a=s.filter(x=>['Article','BlogPosting','NewsArticle'].includes(x['@type']));console.log('ArticleSchema:', a.length, '| HasHeadline:', a[0]?.headline?1:0, '| HasAuthor:', a[0]?.author?1:0, '| HasDate:', a[0]?.datePublished?1:0)
```

### Breadcrumb Schema | `schema-breadcrumb` | info
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const b=s.filter(x=>x['@type']==='BreadcrumbList');console.log('BreadcrumbSchema:', b.length, '| Items:', b[0]?.itemListElement?.length??0)
```

### FAQ Schema | `schema-faq` | fail
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const f=s.filter(x=>x['@type']==='FAQPage');console.log('FAQSchema:', f.length, '| Questions:', f[0]?.mainEntity?.length??0)
```

### LocalBusiness | `schema-local-business` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const lb=s.filter(x=>x['@type']==='LocalBusiness'||String(x['@type']).includes('Business'));console.log('LocalBusinessSchema:', lb.length, '| HasAddress:', lb[0]?.address?1:0)
```

### Organization | `schema-organization` | info
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const org=s.filter(x=>x['@type']==='Organization');console.log('OrgSchema:', org.length, '| HasLogo:', org[0]?.logo?1:0)
```

### Review Schema | `schema-review` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});console.log('ReviewSchema:', s.filter(x=>x['@type']==='Review').length, '| AggRating:', s.filter(x=>x['@type']==='AggregateRating').length)
```

### Video Schema | `schema-video` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const v=s.filter(x=>x['@type']==='VideoObject');console.log('VideoSchema:', v.length, '| HasThumb:', v[0]?.thumbnailUrl?1:0, '| HasDate:', v[0]?.uploadDate?1:0)
```

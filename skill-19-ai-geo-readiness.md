# Skill 19: AI/GEO Readiness

### llms.txt | `geo-llms-txt` | info
**CANNOT VALIDATE** - run: fetch('/llms.txt').then(r=>console.log('LLMsTxt:',r.status))

### AI Bot Access | `geo-ai-bot-access` | warn
**CANNOT VALIDATE** - run: fetch('/robots.txt').then(r=>r.text()).then(t=>console.log(t)) — look for GPTBot, Claude-Web, Anthropic, Google-Extended blocks

### Semantic HTML | `geo-semantic-html` | warn
```js
const tags=['article','section','aside','nav','main','header','footer','figure','time'];console.log('SemanticTagsUsed:', tags.filter(t=>document.querySelector(t)).length, '| Of:', tags.length)
```

### Content Structure | `geo-content-structure` | warn
```js
console.log('H1Count:', document.querySelectorAll('h1').length, '| H2Count:', document.querySelectorAll('h2').length, '| HasLists:', document.querySelector('ul,ol')?1:0, '| ParagraphCount:', document.querySelectorAll('p').length)
```

### Schema Drift | `geo-schema-drift` | warn
```js
const s=[...document.querySelectorAll('script[type="application/ld+json"]')].flatMap(s=>{const j=JSON.parse(s.textContent);return j['@graph']?j['@graph']:[j];});const p=s.find(x=>x['@type']==='Product');console.log('SchemaName:', p?.name??'none', '| H1:', document.querySelector('h1')?.textContent.trim()??'none')
```

# Skill 18: HTML Validation

### Missing DOCTYPE | `htmlval-missing-doctype` | warn
```js
console.log('DOCTYPE:', document.doctype?.name==='html'?1:0)
```

### Missing Charset | `htmlval-missing-charset` | warn
```js
console.log('Charset:', document.querySelector('meta[charset]')?.getAttribute('charset')??document.querySelector('meta[http-equiv="Content-Type"]')?.getAttribute('content')??'none')
```

### Multiple Titles | `htmlval-multiple-titles` | fail
```js
console.log('TitleCount:', document.querySelectorAll('title').length)
```

### Multiple Descriptions | `htmlval-multiple-descriptions` | fail
```js
console.log('DescCount:', document.querySelectorAll('meta[name="description"]').length)
```

### Invalid Head | `htmlval-invalid-head` | warn
```js
const valid=['META','TITLE','LINK','SCRIPT','STYLE','BASE','NOSCRIPT'];console.log('InvalidHeadTags:', [...document.head.children].filter(el=>!valid.includes(el.tagName)).length)
```

### Noscript in Head | `htmlval-noscript-in-head` | warn
```js
console.log('NoscriptInHead:', document.head.querySelector('noscript')?1:0)
```

### Multiple Heads | `htmlval-multiple-heads` | fail
```js
console.log('HeadCount:', document.querySelectorAll('head').length)
```

### Size Limit | `htmlval-size-limit` | warn/fail
```js
console.log('HTMLSizeKB:', Math.round(new Blob([document.documentElement.outerHTML]).size/1024))
```

### Lorem Ipsum | `htmlval-lorem-ipsum` | warn
```js
console.log('LoremIpsum:', document.body.innerText.includes('Lorem ipsum')?1:0)
```

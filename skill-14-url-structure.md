# Skill 14: URL Structure

### Uppercase URLs | `url-uppercase` | warn
```js
console.log('HasUppercase:', window.location.pathname!==window.location.pathname.toLowerCase()?1:0)
```

### Underscores | `url-underscores` | warn
```js
console.log('HasUnderscores:', window.location.pathname.includes('_')?1:0)
```

### Double Slashes | `url-double-slash` | warn
```js
console.log('HasDoubleSlash:', /\/\/(?!$)/.test(window.location.pathname)?1:0)
```

### Spaces in URL | `url-spaces` | fail
```js
console.log('HasSpaces:', (window.location.href.includes('%20')||window.location.href.includes('+'))?1:0)
```

### Non-ASCII | `url-non-ascii` | warn
```js
console.log('HasNonASCII:', /[^\x00-\x7F]/.test(window.location.href)?1:0)
```

### URL Length | `url-length` | warn
```js
console.log('URLLength:', window.location.href.length, '| PathLength:', window.location.pathname.length)
```

### Session IDs | `url-session-ids` | fail
```js
console.log('SessionIDs:', /PHPSESSID|JSESSIONID|sessionid|sessid/i.test(window.location.search)?1:0)
```

### Tracking Params | `url-tracking-params` | warn
```js
console.log('TrackingParams:', /utm_|fbclid|gclid|msclkid/i.test(window.location.search)?1:0)
```

### Internal Search | `url-internal-search` | warn
```js
console.log('IsSearchURL:', /[?&](?:q|query|search|s)=/i.test(window.location.search)?1:0)
```

### Repetitive Path | `url-repetitive-path` | warn
```js
const segs=window.location.pathname.split('/').filter(s=>s);const dups=segs.filter((s,i)=>segs.indexOf(s)!==i);console.log('RepetitiveSegments:', dups.length)
```

### URL Parameters | `url-parameters` | warn
```js
console.log('ParamCount:', [...new URLSearchParams(window.location.search).keys()].length)
```

### Stop Words | `url-stop-words` | warn
```js
const sw=['the','a','an','and','or','but','in','on','at','to','for','of','with','by'];const segs=window.location.pathname.split(/[-\/]/).filter(s=>s);console.log('StopWords:', segs.filter(s=>sw.includes(s.toLowerCase())).length)
```

### Slug Keywords | `url-slug-keywords` | warn/fail
```js
console.log('PathLength:', window.location.pathname.length, '| IsNumeric:', /^\/[\d]+$/.test(window.location.pathname)?1:0)
```

### HTTP/HTTPS Duplicate | `url-http-https-duplicate` | warn
**CANNOT VALIDATE** - navigate to http:// version and check if redirects to https://

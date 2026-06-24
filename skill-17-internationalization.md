# Skill 17: Internationalization

### Lang Attribute | `i18n-lang-attribute` | fail
```js
console.log('LangAttr:', document.documentElement.getAttribute('lang')??'none')
```

### Hreflang Tags | `i18n-hreflang` | warn/fail
```js
console.log('HreflangCount:', document.querySelectorAll('link[rel="alternate"][hreflang]').length)
```

### Hreflang Conflicting | `i18n-hreflang-conflicting` | fail
```js
const langs=[...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l=>l.getAttribute('hreflang'));const dups=langs.filter((l,i)=>langs.indexOf(l)!==i);console.log('DuplicateLangs:', [...new Set(dups)].length)
```

### Hreflang Lang Mismatch | `i18n-hreflang-lang-mismatch` | warn
```js
const pageLang=document.documentElement.getAttribute('lang');const hreflangs=[...document.querySelectorAll('link[rel="alternate"][hreflang]')].map(l=>l.getAttribute('hreflang'));console.log('PageLang:', pageLang??'none', '| HreflangIncludesPage:', hreflangs.includes(pageLang)?1:0)
```

### Hreflang Multiple Methods | `i18n-hreflang-multiple-methods` | warn
```js
console.log('HTMLHreflang:', document.querySelectorAll('link[rel="alternate"][hreflang]').length)
```
**Also check** - Network tab > Response Headers > link (for HTTP header hreflang)

### Hreflang Return Links | `i18n-hreflang-return-links` | fail
**CANNOT VALIDATE** - requires fetching each hreflang target URL

### Hreflang to Noindex | `i18n-hreflang-to-noindex` | fail
**CANNOT VALIDATE** - requires fetching each hreflang target URL

### Hreflang to Non-Canonical | `i18n-hreflang-to-non-canonical` | warn
**CANNOT VALIDATE** - requires fetching each hreflang target URL

### Hreflang to Broken | `i18n-hreflang-to-broken` | fail
**CANNOT VALIDATE** - requires fetching each hreflang target URL

### Hreflang to Redirect | `i18n-hreflang-to-redirect` | warn
**CANNOT VALIDATE** - requires fetching each hreflang target URL

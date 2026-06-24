# Skill 12: Social

### OG Title | `social-og-title` | warn
```js
console.log('OGTitle:', document.querySelector('meta[property="og:title"]')?.getAttribute('content')?.length??0)
```

### OG Description | `social-og-description` | warn
```js
console.log('OGDesc:', document.querySelector('meta[property="og:description"]')?.getAttribute('content')?.length??0)
```

### OG Image | `social-og-image` | warn
```js
console.log('OGImage:', document.querySelector('meta[property="og:image"]')?1:0)
```

### OG Image Size | `social-og-image-size` | warn
```js
console.log('OGWidth:', document.querySelector('meta[property="og:image:width"]')?.getAttribute('content')??'none', '| OGHeight:', document.querySelector('meta[property="og:image:height"]')?.getAttribute('content')??'none')
```

### OG URL | `social-og-url` | warn
```js
console.log('OGURL:', document.querySelector('meta[property="og:url"]')?1:0)
```

### OG URL Canonical | `social-og-url-canonical` | fail
```js
console.log('OGURL:', document.querySelector('meta[property="og:url"]')?.getAttribute('content')??'none', '| Canonical:', document.querySelector('link[rel="canonical"]')?.href??'none')
```

### Twitter Card | `social-twitter-card` | warn
```js
console.log('TwitterCard:', document.querySelector('meta[name="twitter:card"]')?.getAttribute('content')??'none')
```

### Share Buttons | `social-share-buttons` | warn
```js
console.log('ShareButtons:', document.querySelectorAll('a[href*="facebook.com/sharer"],a[href*="twitter.com/intent"],a[href*="linkedin.com/sharing"],a[href*="pinterest.com/pin"]').length)
```

### Social Profiles | `social-profiles` | warn
```js
console.log('SocialLinks:', document.querySelectorAll('a[href*="facebook.com/"],a[href*="twitter.com/"],a[href*="instagram.com/"],a[href*="linkedin.com/"],a[href*="youtube.com/"],a[href*="threads.net/"]').length)
```

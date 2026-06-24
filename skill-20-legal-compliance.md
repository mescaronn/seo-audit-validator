# Skill 20: Legal Compliance

### Cookie Consent | `legal-cookie-consent` | warn
```js
const cmps=['cookieyes','onetrust','cookiebot','termly','quantcast','usercentrics','didomi','iubenda'];const html=document.documentElement.outerHTML.toLowerCase();console.log('CMPDetected:', cmps.filter(p=>html.includes(p)).length, '| CookieBanner:', document.querySelector('[class*="cookie"],[class*="consent"],[id*="cookie"],[id*="consent"]')?1:0)
```

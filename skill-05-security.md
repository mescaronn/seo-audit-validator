# Skill 05: Security

### HTTPS | `security-https` | fail
```js
console.log('Protocol:', window.location.protocol)
```

### External Link Security | `security-external-links` | warn
```js
console.log('UnsafeBlankLinks:', [...document.querySelectorAll('a[target="_blank"]')].filter(a=>{const r=a.getAttribute('rel')||'';return!r.includes('noopener')&&!r.includes('noreferrer');}).length)
```

### Protocol-Relative URLs | `security-protocol-relative` | warn
```js
console.log('ProtocolRelative:', (document.documentElement.outerHTML.match(/['"]\/\/[^\'"]+/g)||[]).length)
```

### Mixed Content | `security-mixed-content` | warn/fail
```js
console.log('MixedContent:', document.querySelectorAll('[src^="http:"],[href^="http:"],[action^="http:"]').length)
```

### Leaked Secrets | `security-leaked-secrets` | fail
```js
console.log('LeakedSecrets:', (document.documentElement.outerHTML.match(/(?:api[_-]?key|apikey|secret|password|token)\s*[:=]\s*["'][^"']{10,}/gi)||[]).length)
```

### Form HTTPS | `security-form-https` | warn/fail
```js
console.log('HTTPForms:', [...document.querySelectorAll('form')].filter(f=>f.action.startsWith('http:')).length)
```

### SSL Expiry | `security-ssl-expiry` | warn/fail
**CANNOT VALIDATE** - check Network tab > Response Headers > strict-transport-security

### SSL Protocol | `security-ssl-protocol` | warn/fail
**CANNOT VALIDATE** - check Network tab > Response Headers > strict-transport-security (look for includeSubDomains, preload)

### HSTS | `security-hsts` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > strict-transport-security

### CSP | `security-csp` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > content-security-policy

### X-Frame-Options | `security-x-frame-options` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > x-frame-options

### X-Content-Type | `security-x-content-type-options` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > x-content-type-options

### Permissions-Policy | `security-permissions-policy` | warn
**CANNOT VALIDATE** - check Network tab > Response Headers > permissions-policy

### Referrer-Policy | `security-referrer-policy` | warn
```js
console.log('ReferrerMeta:', document.querySelector('meta[name="referrer"]')?.getAttribute('content') ?? 'none')
```

### HTTPS Redirect | `security-https-redirect` | warn
**CANNOT VALIDATE** - navigate to http:// version manually

### Password over HTTP | `security-password-http` | fail
```js
console.log('PasswordFields:', document.querySelectorAll('input[type="password"]').length, '| IsHTTP:', window.location.protocol==='http:'?1:0)
```

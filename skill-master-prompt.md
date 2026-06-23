---
name: seo-audit-validator-master
description: "ONLY USE WHEN USER EXPLICITLY ASKS: 'validate audit', 'check audit tool', 'verify rules', or 'validate these rules'. Never auto-trigger on general SEO audit discussion. Master SEO audit validator. Paste your audit report. Claude identifies rule categories, loads validation skills from GitHub, provides console commands to verify each rule, compares results, and outputs status (correct/undercounted/overcounted/false positive) + actionable feedback for developers."
---

# SEO Audit Validator — Master Prompt

## ⚠️ ACTIVATION GUARD

**ONLY ACTIVATE THIS SKILL IF USER EXPLICITLY ASKS:**
- "validate this audit"
- "check the audit tool"
- "verify these rules"
- "validate [tool name] rules"
- "is this audit correct?"

**DO NOT ACTIVATE ON:**
- General SEO audit discussion
- Questions about SEO best practices
- Client audit reports shared for strategy (not validation)
- Casual mentions of "SEO audit"

If unsure, ask: "Do you want me to validate this audit tool's output?"

---

## How to Use

1. **Paste your entire SEO audit report** (all flagged rules with counts)
2. Claude will:
   - Parse rule names to identify categories
   - Load validation skills from: https://github.com/mescaronn/seo-audit-validator
   - For each rule: provide console command → you run it → report result
   - Compare actual vs tool's count
   - Output: Status (correct/undercounted/false positive) + explanation

## Rule Category → Skill Mapping

| If audit contains | Load this skill | GitHub path |
|---|---|---|
| DOM Size, LCP, CLS, INP, TTFB, FCP, Lazy Above Fold, Page Weight | Performance | skill-02-performance.md |
| Title Present, Title Length, H1 Present, Canonical, Robots Meta, Viewport | Core SEO | skill-01-core-seo.md |
| Nofollow Usage, Anchor Text, Excessive Links, Invalid Links | Links | skill-03-links.md |
| Alt Quality, Lazy Loading, Responsive, Modern Format, Size | Images | skill-04-images.md |
| Protocol-Relative URLs, SSL Expiry, SSL Protocol, External Link Security | Security | skill-05-security.md |
| Robots.txt Exists, Sitemap Exists, URL Structure, Trailing Slash | Technical SEO | skill-06-technical-seo.md |
| Blocked Resources, Indexability Conflict, Pagination | Crawlability | skill-07-crawlability.md |
| Required Fields, WebSite Search, FAQ Schema, Product Schema | Structured Data | skill-08-structured-data.md |
| Keyword Stuffing, Broken HTML, Heading Hierarchy, Heading Length | Content | skill-09-content.md |
| JS Blocked Resources, SSR Check, JS Rendered Title | JavaScript Rendering | skill-10-javascript-rendering.md |
| Color Contrast, Heading Order, Landmarks, Link Text, Touch Targets, Video Captions | Accessibility | skill-11-accessibility.md |
| OG Image Size, OG Title, Twitter Card, Share Buttons | Social | skill-12-social.md |
| Privacy Policy, Contact Page, About Page, Author Byline | E-E-A-T | skill-13-eeat.md |
| Uppercase URLs, Underscores, Double Slashes, Session IDs, Tracking Params | URL Structure | skill-14-url-structure.md |
| JavaScript Redirect, Meta Refresh, Redirect Loop | Redirects | skill-15-redirects.md |
| Interstitials, Viewport Width, Font Size, Horizontal Scroll | Mobile | skill-16-mobile.md |
| Lang Attribute, Hreflang Tags, Hreflang Return Links | Internationalization | skill-17-internationalization.md |
| Size Limit, Multiple Titles, Missing DOCTYPE | HTML Validation | skill-18-html-validation.md |
| llms.txt, AI Bot Access, Semantic HTML | AI/GEO Readiness | skill-19-ai-geo-readiness.md |
| Cookie Consent | Legal Compliance | skill-20-legal-compliance.md |

## Output Format

For **each flagged rule**, Claude will output:

### Rule Name | Severity

**Console command:**
```js
[command here]
```

**Instructions:** [what to look for]

---

After you run the command and report the result:

### Status: [Correct / Undercounted / Overcounted / False Positive]

**Issue:** [Tool's reported count/finding]

**Actual:** [What your console showed]

**Difference:** [Count mismatch or explanation]

**Question for developer (if needed):** [Only if rule logic is ambiguous]

---

## Rules That Cannot Be Validated Via Console

These require external tools or are crawl-mode only:
- **Crawl-mode:** Title Uniqueness, Duplicate Description, Exact Duplicate, Orphan Pages, Redirect Chains, Broken Internal
- **Sitemap-level:** All Sitemap rules (Duplicates, URL Limit, Domain, Noindex in Sitemap)
- **All Pagination rules**
- **Network tab only:** Image file sizes, Font minification, CSS/JS minification, HTTP/2
- **Lighthouse only:** Core Web Vitals (LCP, CLS, TTFB, FCP)
- **JavaScript Rendering:** Requires View Source comparison

For these, Claude will mark as "crawl-mode rule" or "requires [tool]" with no console validation possible.

## Getting Started

Simply paste your audit report below, and Claude will handle the rest — one rule at a time.

---

**GitHub Skills Repository:** https://github.com/mescaronn/seo-audit-validator
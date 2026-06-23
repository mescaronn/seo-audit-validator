---
name: seo-audit-validator-index
description: "Master index for SEO audit validation. Use this first when user pastes any SEO audit report. Routes to the correct skill based on rule names found in the report."
---

# SEO Audit Validator: Master Index

## How to Use

1. User pastes SEO audit report (all flagged rules)
2. Parse the report — identify which categories are present
3. Load the corresponding skill file for each category
4. For each flagged rule: give the console command → user runs it → compare result

## Category → Skill Mapping

| Category | Skill File | Rule IDs |
|----------|-----------|----------|
| Core SEO | skill-01-core-seo.md | core-* |
| Performance | skill-02-performance.md | cwv-*, perf-* |
| Links | skill-03-links.md | links-* |
| Images | skill-04-images.md | images-* |
| Security | skill-05-security.md | security-* |
| Technical SEO | skill-06-technical-seo.md | technical-* |
| Crawlability | skill-07-crawlability.md | crawl-* |
| Structured Data | skill-08-structured-data.md | schema-* |
| Content | skill-09-content.md | content-* |
| JavaScript Rendering | skill-10-javascript-rendering.md | js-* |
| Accessibility | skill-11-accessibility.md | a11y-* |
| Social | skill-12-social.md | social-* |
| E-E-A-T | skill-13-eeat.md | eeat-* |
| URL Structure | skill-14-url-structure.md | url-* |
| Redirects | skill-15-redirects.md | redirect-* |
| Mobile | skill-16-mobile.md | mobile-* |
| Internationalization | skill-17-internationalization.md | i18n-* |
| HTML Validation | skill-18-html-validation.md | htmlval-* |
| AI/GEO Readiness | skill-19-ai-geo-readiness.md | geo-* |
| Legal Compliance | skill-20-legal-compliance.md | legal-* |

## Rule Name → Category Quick Reference

DOM Size, LCP, CLS, INP, TTFB, FCP, Lazy Above Fold, Page Weight, Render-Blocking, Font Loading → Performance
Title Present, Title Length, H1 Present, Canonical Present, Robots Meta, Viewport Present → Core SEO
Nofollow Usage, Anchor Text, Excessive Links, Invalid Links, External Link Security → Links
Alt Quality, Lazy Loading, Responsive, Modern Format, Size, Picture Element, Inline SVG Size → Images
Protocol-Relative URLs, SSL Expiry, SSL Protocol, External Link Security, Mixed Content → Security
Robots.txt Exists, Sitemap Exists, URL Structure → Technical SEO
Blocked Resources, Indexability Conflict → Crawlability
Required Fields, WebSite Search, FAQ Schema, Product Schema → Structured Data
Keyword Stuffing, Broken HTML, Heading Hierarchy, Heading Length, Heading Unique, Description Pixel Width → Content
JS Blocked Resources, SSR Check → JavaScript Rendering
Color Contrast, Heading Order, Landmarks, Link Text, Touch Targets, Video Captions → Accessibility
OG Image Size, OG Title, Twitter Card → Social
Privacy Policy, Contact Page, About Page → E-E-A-T
JavaScript Redirect, Meta Refresh → Redirects
Interstitials, Viewport Width, Font Size → Mobile
Lang Attribute, Hreflang Tags → Internationalization
Size Limit, Multiple Titles → HTML Validation
llms.txt, AI Bot Access → AI/GEO Readiness
Cookie Consent → Legal Compliance

## Output Format

For each rule, after user provides console result, output:

**If tool is correct:** [No report needed]

**If tool is wrong:**
> **Issue:** [Rule Name] ([severity])
>   [Tool's exact output]
>
> **Correct:** [What was actually found + count discrepancy]
>
> **What I used to check:** [Console command used]
>
> **Question for developer:** [Only if rule logic is ambiguous]

## Notes on Rules That Require Crawl Mode
These cannot be validated via Console (single-page check):
- Title Uniqueness, Duplicate Description, Exact Duplicate, Near Duplicate
- Orphan Pages, Link Depth, Redirect Chains, Broken Internal, External Valid
- All Sitemap-level rules (Sitemap Duplicates, URL Limit, Domain, Noindex in Sitemap)
- All Pagination rules
Mark these as "crawl-mode rule — requires site-wide check."

## Rules That Cannot Be Checked Via Console
These require external tools or visual inspection:
- Core Web Vitals (LCP, CLS, TTFB, FCP) → Use Lighthouse
- Image file sizes (Size rule) → Use DevTools Network tab
- Font minification, CSS/JS minification → Use Network tab
- HTTP/2 → Use Network tab + Protocol column
- Security headers (CSP, X-Frame-Options, etc.) → Use fetch HEAD
- All JS Rendering rules → Requires View Source comparison

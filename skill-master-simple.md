---
name: seo-audit-validator-master-simple
description: "ONLY USE WHEN USER EXPLICITLY ASKS: 'validate audit from my repo'. Load skills from GitHub. Validate rules. Ask for console only when needed. Wait for 'make final table' before outputting anything."
---

# SEO Audit Validator

## INSTRUCTIONS:
1. Load skill files from GitHub repo
2. For each rule: validate using the skill file
3. If skill can validate without console → validate it directly, store result internally
4. If skill REQUIRES console → combine ALL console commands into ONE block wrapped in (function(){...})();
5. Console commands must return NUMBERS ONLY using console.log('Label:', value) — NO arrays, NO objects, NO URLs, NO .map()
6. Wait for my console results (I paste numbers only)
7. Store all results internally — DO NOT output anything yet
8. ONLY when I say "make final table" → output ONE table: RULE | FINDING | CORRECT
9. Only rows where tool is WRONG
10. No analysis, no extra columns, no notes, no badges
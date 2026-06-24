---
name: seo-audit-validator
description: "ONLY ACTIVATE when user says 'validate audit from my repo'. Read skill files from GitHub. For each rule find the console command. Merge all console commands into ONE block. Output ONLY numbers. Store results. Wait for 'make final csv'."
---

# SEO Audit Validator

## INSTRUCTIONS

1. Read skill files from https://github.com/mescaronn/seo-audit-validator
2. For each rule in the audit: find the matching console command in the skill file
3. Merge ALL console commands into ONE block wrapped in `(function(){...})();`
4. ALL commands must use `console.log('Label:', value)` returning NUMBERS ONLY — NO arrays, NO objects, NO URLs, NO .map() returning objects
5. List rules that CANNOT VALIDATE separately in chat (no console needed)
6. Wait for user to paste numbers back
7. Store all results internally — output NOTHING yet
8. ONLY when user says **"make final csv"** → output CSV with ONLY rules where tool is WRONG

## Final CSV Format

```
RULE,RULE ID,FINDING,CORRECT
Description Length (warning),core-description-length,"Meta description is too long (164 characters)","Actual: 141 characters. Tool overcounted by 23."
```

**Columns:**
- A: RULE (name + severity)
- B: RULE ID (from skill file)
- C: FINDING (exact quote from audit report — what tool said)
- D: CORRECT (what is actually true — brief, only if tool is wrong)

**Only rows where tool is WRONG. No other rows.**

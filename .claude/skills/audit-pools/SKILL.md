---
name: audit-pools
description: Audit existing PROMPT_POOLS entries in data/pools.js against the AGENTS.md quality bar and report violations for approval before fixing. Use when asked to review, clean up, or check the quality of existing prompt pool entries.
---

# Audit PROMPT_POOLS entries

Read `AGENTS.md` first if it isn't already in context — the rules enforced
here are defined there.

## Procedure

1. **Mechanical pass** — scan every entry in `data/pools.js` for:
   - trailing periods or uppercase first letter in `en`
   - a subject noun (patterns like `a woman`, `a man`, `girl`, `woman aged`)
   - banned filler words (`beautiful`, `stunning`, `amazing`, `gorgeous`,
     `masterpiece`, `best quality`, or a bare `8K` not already grandfathered)
   - template-suffix duplication (`85mm`, `shallow depth of field`,
     `photorealistic`, `high detail skin`)
   - embedded aspect ratios (`9:16`, `4:5`, etc.) or negative prompts
     (`no illustration`, `no anime`, `no blur`, etc.)
   - `ko` labels longer than ~16 characters
   - exact or near-duplicate `en` strings within the same pool
2. **Judgment pass** — for entries that pass the mechanical check, look for:
   - category bleed (e.g. makeup/skin description filed under `lighting`,
     pose description filed under `location`)
   - density too low (fewer than 2 concrete specifics) or too high
     (kitchen-sink entries stacking many unrelated details)
   - `ko`/`en` register mismatch (label promises something the fragment
     doesn't deliver, or vice versa)
3. **Report findings as a table**: `pool | ko | violated rule | suggested fix`.
   Known existing debt as of the last audit (don't re-report unless asked to
   fix it): `outfit` entries with "168cm tall" / "East Asian woman aged
   20-23"; `lighting`'s "Douyin 뷰티" (makeup, wrong pool); several entries
   with embedded aspect ratios or negative prompts; `action`'s "85mm full
   body shot" (duplicates template lens spec).
4. **Apply fixes only on approval** — do not edit `data/pools.js` until the
   user confirms which findings to act on.

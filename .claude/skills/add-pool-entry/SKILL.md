---
name: add-pool-entry
description: Add one or more entries to PROMPT_POOLS in data/pools.js, enforcing the quality bar in AGENTS.md. Use when asked to add a new location/mood/outfit/action/lighting prompt entry.
---

# Add a PROMPT_POOLS entry

Read `AGENTS.md` first if it isn't already in context — this skill enforces
the rules defined there, it doesn't restate them.

## Procedure

1. **Identify the target pool** (`location`, `mood`, `outfit`, `action`, or
   `lighting`). If ambiguous, ask.
2. **Read the last ~10 entries** of that pool in `data/pools.js` under
   `// 학습 추가` to calibrate register and density before drafting.
3. **Draft the `en` fragment** against the AGENTS.md checklist:
   - lowercase start, no trailing period
   - no subject noun (no "a woman…", "she is…")
   - 2–4 concrete sensory specifics (place/posture/light/texture), no more
   - light named by source + temperature, not bare adjectives
   - no banned filler ("beautiful", "stunning", "high quality", etc.)
   - doesn't duplicate the template suffix (85mm, shallow depth of field,
     photorealistic, high detail skin)
   - stays in its pool's lane (see Category purity in AGENTS.md)
4. **Draft the `ko` label**: a short chip (~2–14 chars), `+` for compounds,
   not a translation of the full `en` sentence.
5. **Run the composition test**: pick one existing entry from each of the
   other four pools and assemble the full template order —
   `action, location, outfit, lighting, mood` — with the draft substituted
   into its slot. Read the resulting sentence. If it produces a hard
   contradiction or breaks grammatically, revise the draft.
6. **Check for near-duplicates**: scan the target pool for entries covering
   the same place/garment/light source. If one exists, either differentiate
   the new entry clearly or skip adding it.
7. **Show the proposed `{ ko, en }` pair and the composed sample prompt** for
   approval before editing the file, unless told to commit directly.
8. **Append** the approved entry at the end of the pool's `// 학습 추가`
   section in `data/pools.js`. Do not reorder or renumber existing entries.

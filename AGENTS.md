# AGENTS.md — aiprompt

Operative rules for any coding agent (Codex, Claude Code, etc.) working in this
repo. Claude Code loads this same file via an import in CLAUDE.md — edit here,
not there, so the two agents never enforce different standards.

## What this repo is

A single-page, no-build-step web app: `index.html` contains a Korean-language
"랜덤 프롬프트 생성기" (random prompt generator) for AI image generation. React 18
UMD + Babel standalone from CDN, inline styles, dark theme (`#0D0D12`
background, Pretendard font). There is no bundler, no package.json, no test
suite. Keep it that way unless explicitly asked otherwise.

The heart of the app is `PROMPT_POOLS` (in `data/pools.js`): five arrays of
`{ ko, en }` entries (`location`, `mood`, `outfit`, `action`, `lighting`).
`generatePrompt()` picks one entry from each pool and concatenates them in
this fixed order:

    action.en, location.en, outfit.en, lighting.en, mood.en,
    photorealistic portrait photography, 85mm lens, shallow depth of field,
    high detail skin texture. [FACE_LOCK]

Every rule about the `en` field below exists because of this concatenation.
An entry is never rendered alone — it is a fragment spliced into a longer
prompt whose subject (the character) is supplied by a face reference, and
whose camera/quality suffix is supplied by the template.

## Structural rules (repo-specific)

1. **No build tooling, no framework migration.** Do not introduce npm, Vite,
   JSX build steps, TypeScript, or external state libraries. The app must keep
   working when `index.html` is opened directly from disk (`file://`), which
   means: no ES module imports of local files, no `fetch()` of local JSON.
   Plain `<script src>` tags defining globals are the only safe way to split
   files.
2. **Data edits go in `data/pools.js`; logic edits go in `index.html`.** Never
   mix a pool-content change and a UI/logic change in the same commit unless
   the task requires both.
3. **Preserve the section comments** inside each pool (`// 기존` for original
   entries, `// 학습 추가` for later additions). New entries go at the end of
   the appropriate pool under `// 학습 추가` (or a new dated comment section if
   a batch is large).
4. **UI text stays Korean; prompt text stays English.** The app's interface
   language is Korean (labels, buttons, headings). The `en` field is always
   English. Do not localize either direction.
5. After any pool change, the POOL SIZE panel updates automatically
   (it reads `.length`) — do not hand-maintain counts anywhere.

## Quality bar for `en` entries (repo-specific)

These rules are derived from the strongest existing entries (e.g. 빨래방,
바이닐 레코드샵, 심야 도서관, 이자카야) and from how the template consumes
fragments. Match them exactly.

### Grammar of a fragment
- Lowercase first letter, **no trailing period**, no leading/trailing spaces.
  The template joins fragments with commas; a period mid-prompt breaks the
  flow and confuses some image models.
- **No subject noun.** Never write "a woman standing…" — the subject comes
  from the face reference. Start with a participle, preposition, or noun
  phrase: "leaning on…", "inside a…", "wearing…", "warm amber…".
- Internal commas are fine and encouraged; em dashes appear in one existing
  entry and are acceptable sparingly.
- Must read naturally when concatenated in the fixed order
  action → location → outfit → lighting → mood. Before finalizing, do the
  **composition test**: splice the new entry into the template with one
  randomly chosen existing entry from each other pool and read the result
  aloud. If it produces a hard contradiction ("wearing an oversized coat,
  wearing a bikini" is not), rewrite.

### Density and specificity
- Target 2–4 concrete, sensory specifics per entry. The strong existing
  pattern is: one spatial/physical anchor + one light quality + one texture
  or object detail. Example from the repo:
  "inside a late-night laundromat, leaning against running washing machines,
  fluorescent lighting, condensation on windows" — place, posture-context,
  light, texture. Aim for that density, not more.
- Name light by source and temperature ("warm amber desk lamp", "cool
  daylight-tone street lamp"), not by adjective alone ("nice lighting").
- **Banned filler**: "beautiful", "stunning", "amazing", "gorgeous",
  "high quality", "masterpiece", "best quality", "8K" as a bare quality tag.
  If a word could be pasted into any entry without changing the image, it is
  filler. (Exception: existing entries already containing "8K" are
  grandfathered until an audit pass; do not add new ones.)
- **Do not duplicate the template suffix.** The template already appends
  "photorealistic portrait photography, 85mm lens, shallow depth of field,
  high detail skin texture". New entries must not repeat lens focal lengths,
  "shallow depth of field", "photorealistic", or "high detail skin" unless
  the entry deliberately overrides the default — and then say the override
  explicitly.

### Category purity
Each pool answers one question. Keep entries in their lane:
- `location` — where, and the ambient light/objects of that place.
- `mood` — overall grade/aesthetic (film stock, editorial style, color story).
- `outfit` — garments, fabrics, accessories. No body measurements, no age
  ranges, no facial descriptions (the face is locked by FACE_LOCK).
- `action` — pose, gesture, gaze, expression.
- `lighting` — light sources, direction, contrast, flash character. Makeup
  and skin-finish descriptions do not belong here.
- Aspect ratios ("9:16", "4:5") and negative prompts ("no illustration no
  anime") should not be added inside new pool entries — if framing control is
  needed, raise it as a template-level feature instead.

### The `ko` label
- Short Korean noun-phrase tag, roughly 2–14 characters; compounds joined
  with `+` ("크롭탑+버건디 부츠" style) are the established convention.
- It renders as a small chip (10–11px) in the UI, so it must scan at a
  glance. It is a label for the `en` payload, not a translation of it —
  "빨래방" for the full laundromat scene is correct; a sentence-length ko
  is wrong.
- The ko must not promise something the en doesn't deliver, and vice versa.

## Which of these rules are general vs. repo-specific (honesty note)

- **General good creative-writing/prompt-writing principles** (they'd apply to
  any prompt library): concrete sensory detail over adjectives; ban generic
  filler; name light by source and temperature; specificity budget of a few
  strong details rather than many weak ones.
- **Specific to this repo's mechanics** (they exist because of how
  `generatePrompt()` concatenates): fragment grammar (lowercase, no period,
  no subject noun), the composition test, the fixed concatenation order, the
  no-duplicate-suffix rule, category purity, ko-as-chip length limits, the
  file:// constraint, and the 기존/학습 추가 comment convention.

Rules in the second group are non-negotiable; rules in the first group are
strong defaults you may consciously trade off if the repo owner asks.

## A note on tone/quality expectations

The bar here is scene fragments that feel observed rather than assembled from
stock phrases — condensation on the window, dust in the light beam, the
charger cable left in frame. That is aspirational guidance, not a verified
spec of any particular model; the concrete rules above are the actual
definition of quality in this repo.

## Known content debt (do not copy these patterns)

An audit of the existing pools found entries that violate the rules above.
They're left in place pending an explicit cleanup pass — don't use them as
precedent when writing new entries:
- `outfit`: "168cm tall" and "East Asian woman aged 20-23" — body/age
  description leaking past FACE_LOCK's job.
- `lighting`: "Douyin 뷰티" — makeup/skin-finish description, wrong pool.
- Several `location`/`action`/`lighting` entries embed aspect ratios (`9:16`,
  `4:5`) or a negative prompt ("no illustration no anime") — framing/negative
  concerns belong at the template level, not inside a pool entry.
- `action`: "85mm full body shot" — duplicates the template's own lens spec.

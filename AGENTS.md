# AGENTS.md — Boss Battle: TPAT3 + English

## 1. What it is

A single-file static arcade quiz game for Thai students practising TPAT3-style content
(physics, engineering, logic, math) and English. 180 questions. Correct answers deal damage to a
boss; wrong answers cost a heart and heal the boss. It ships as **one** `index.html` with no build
step, no dependencies and no network requests. It is a study toy, not an LMS and not an official
exam product.

Questions are original items written against the exam blueprint, not reproduced TGAT/TPAT papers.

## 2. Commands

```bash
# run
open index.html                       # file:// works, everything is inline
python3 -m http.server 8080           # optional local server

# syntax-check the inline script (the only "build")
node -e 'const fs=require("fs");const h=fs.readFileSync("index.html","utf8");fs.writeFileSync("/tmp/bb.js",h.match(/<script>([\s\S]*?)<\/script>/)[1])'
node --check /tmp/bb.js

# deploy: push to main; GitHub Pages serves the repo root (no workflow)
git push origin main
```

There is no package.json, bundler or test runner. Verification during development was done with
headless Chrome (puppeteer-core) at 360×640, 390×844 and 1440×900.

## 3. Architecture

Everything is in `index.html`, in this order:

1. `<style>` — design tokens, menu, boss ring, question card, skills, overlays, reduced-motion overrides.
2. Markup — `#vMenu`, `#vBattle`, `#vEnd`, `#vStats`; plus `#ov` (dialog), `#banner` (rage),
   `#fxLayer`, `#cf` (particle canvas), `#live` (screen reader).
3. `<script>` IIFE, sections 0–16:
   helpers → audio → storage → i18n → subjects/bank → DOM refs → particles → state → menu →
   intro → battle → timer → skills → HUD → finish/paintEnd → stats → events → boot.

The files that control behaviour:

| Location | Controls |
|---|---|
| `BANK` | All content, 500 questions (physics 101 · engineering 100 · logic 100 · math 99 · English 100) |
| `SUB` | Subject keys → Thai/English name, emoji, accent colour, boss name, description |
| `S` | Whole battle state; `startBattle()` resets it |
| `TIER` | `mix` \| `core` (d1–2) \| `hard` (d2–3); chosen in the boss dossier and applied by `buildSet()` |
| `renderMenu` / `paintDossier` / `renderStats` | Every screen is re-rendered on language change |
| `setView` / `goHome` / `goStats` / `openSheet` | Navigation: views, bottom tabs, settings sheet |
| `M.th` / `M.en` + `t(key, …)` | UI copy; static nodes use `data-i18n` / `data-i18n-aria` |
| `onCorrect` / `onWrong` / `checkRage` | Damage, healing, combo, rage trigger |
| `renderMenu` / `paintEnd` / `renderStats` | Every screen is re-rendered on language change |

Question schema (must stay stable):

```js
{ id:"ph1", s:"physics", d:1, q:"…", o:["…","…","…","…"], a:1, w:"…" }
```

`id` unique · `s` ∈ physics | engineering | logic | math | english · `d` 1–3 · `o` exactly 4 ·
`a` correct index · `w` explanation (Thai).

## 4. Decisions and constraints

- **Zero external requests.** No Google Fonts, CDN, images or audio files. The page must run from a
  USB stick / `file://`. If a webfont is ever added, it must be inlined.
- **Thai is the default UI language.** Persisted at `bbTPAT3_v1.lang`; do not change the default.
- **Question content is not translated.** The UI switches TH/EN but content stays in its teaching
  language, with a `TH`/`EN` chip on the card. Translating the 100-item bank is a deliberate
  separate project.
- **Balance constants are choices, not accidents:** boss 1300 HP (Rush 1600), damage
  `70 + 12×combo` capped at combo 6, crit ×1.5 within 8 s, wrong answer costs 1 heart and heals 30,
  base clock 32 s → 26 s in rage. Changing one usually requires retuning the rest.
- **Storage key `bbTPAT3_v1`** holds mute, focus mode, language, per-subject `{c,n,best}`,
  totals, best combo/score and day streak. Bump the key for breaking shape changes.
- **3D is CSS transforms + a 2D canvas**, not Three.js. This keeps the file under 100 KB and offline.
- **Navigation is tab-based**: bottom bar (Play / Stats / Settings), subject carousel on Play, boss dossier sheet with tier chips, and a one-tap 🏠 in battle. Do not reintroduce the old chip row or a separate back button on Stats — the tab bar is the home affordance.
- **Content scale**: 500 questions inline (~140 KB). Up to ~800 is fine inline; beyond that, move `BANK` to a separate classic `<script src="bank.js">` (not `fetch`, so `file://` still works) and load it before the app script.
- **Skill rules:** Shield 2 uses / 5 s CD, Freeze 2 / 6 s, 50/50 3 / 8 s. Weak-point rematches get
  1/1/2. A cut option can never be answered even by keyboard.
- `el.*` is a cache of element IDs; renaming an ID in markup requires updating that map.

## 5. Known issues

- Emoji are used for boss faces and skill icons; their look depends on the OS emoji font
  (headless Chrome falls back to monochrome glyphs).
- Audio is fully synthesised; there is no music and some browsers need a first tap to unlock.
- Stats are per device and are not synced. Clearing site data wipes them.
- No automated test suite is committed; regressions are currently caught by manual/headless play.
- The 500-question bank is enough to replay but heavy users will still see repeats; adding items is
  the intended maintenance path. Keep the per-subject pools balanced across difficulty 1–3.

## 6. Continuity

- **Deployed:** https://somphol.github.io/boss-battle-tpat3/ (GitHub Pages, `main`, root)
- **Repo:** https://github.com/somphol/boss-battle-tpat3
- **Local master copy:** `/Users/somphol/Projects/boss-battle-tpat3/index.html`
- Design language: cyberpunk arcade; palette in `:root`; each subject owns an accent colour used by
  the boss ring, HP bar, cards and particles.

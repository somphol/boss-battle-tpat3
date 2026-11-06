# Boss Battle: TPAT3 + English

A single-file arcade quiz game for TPAT3-style science/engineering/logic and English practice.
Answer correctly to damage the boss, chain combos, spend three tactical skills, and survive the rage phase.

**Play:** https://somphol.github.io/boss-battle-tpat3/

## What is in it

- **500 original practice questions** — physics 101, engineering 100, logic 100, math 99, English 100 — each with an explanation.
- **5 bosses + Boss Rush** (all subjects, 18 questions).
- **Modern navigation**: a subject carousel on the home screen, a boss dossier with accuracy/best/bank size, three difficulty tiers (mixed · core · hard), and a persistent bottom tab bar (Play / Stats / Settings) plus a one-tap Home button in battle.
- **3 skills**: Shield (blocks one hit), Time Freeze (+10 s), 50/50 (removes two wrong options) — limited uses plus cooldowns.
- **Rage phase** below 50% boss HP: red aura, faster clock.
- **Bilingual UI — Thai by default**, English toggle in the top-left. Question content stays in the language it is taught in (Thai for TPAT3 subjects, English for the English set) and is labelled `TH` / `EN` on the card.
- **Diagnostic report** at the end: rank badge, accuracy bars by topic, strongest/weakest analysis, and a tap-to-reveal review of every missed question.
- Combo multiplier, crits, screen shake, canvas particles, synthesised Web Audio SFX and a procedural synthwave BGM loop (toggleable in Settings).
- **Boss forms change with difficulty** (core / mixed / hard) and again in the rage phase.

## Controls

| Action | Key |
|---|---|
| Answer | `1` `2` `3` `4` |
| Shield / Freeze / 50-50 | `Q` `W` `E` |
| Next question | `Enter` or `Space` |
| Quit battle | `Esc` or the 🏠 button |
| Move between tabs | `Esc` closes sheets; tabs are tap/click |

Everything also works by touch/mouse.

## Notes

- Questions are **written for this project as "แนวข้อสอบ"** — they follow the exam blueprint but are not reproduced past papers.
- One HTML file, no build, no external requests; it runs from `file://` and offline.
- Stats and language preference live in `localStorage` on the device.

## Run locally

```bash
open index.html
# or
python3 -m http.server 8080
```

See [AGENTS.md](AGENTS.md) for the engineering handoff.

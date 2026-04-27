# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

Private GitHub repo: `lloydy1982/politics-project`

**Commit and push frequently.** After every meaningful unit of work — new file, feature, fix, or config change — stage the relevant files and push to `origin/main`. Use clean, descriptive commit messages focused on the "why". Never let significant work sit uncommitted.

## What has been built

### VoteWise — Welsh Senedd 2026 Voting Guide (`index.html`)

A single-page, zero-dependency party-matching quiz for the Welsh Senedd 2026 election.

**How it works:**
- Full-screen disclaimer/landing screen on first load (skipped when loading a shared URL)
- 12 questions across key Welsh policy areas
- 6 answer options per question — one per party, each drawn from that party's official 2026 manifesto
- Each option has a hidden score object mapping party IDs to points (0–3); the party whose position it represents always scores 3
- After all 12 answers, each party's score is expressed as a percentage of their maximum possible score (each party can max at 36)
- Results show: winner card with match %, 2nd and 3rd place, all 6 parties ranked with animated bar chart
- Share via Web Share API (mobile) or Twitter fallback; copy-to-clipboard with fallback
- URL state: answers encoded as a 12-char `?r=` query param (valid chars `0–5` for 6 options) — results are directly shareable and bookmarkable
- Transparency note on results screen with links to all six official manifestos

**Parties covered:** Welsh Labour, Welsh Conservatives, Plaid Cymru, Liberal Democrats, Green Party, Reform UK

**Policy areas:** NHS Wales, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Devolution & Independence, Transport & Roads, Social Care, Policing & Justice, Agriculture & Farming, Energy, Welsh Language

**Content accuracy:** All 72 option/party pairings sourced directly from official manifesto text files (`manifestos/*.txt`, extracted from PDFs). Full audit conducted against `lab.txt`, `con.txt`, `ld.txt`, `pc.txt`, `grn.txt`, `ref.txt`. Lib Dem Welsh language position confirmed against manifesto pp.46–47. Lib Dem energy policy confirmed renewables-only (no nuclear in manifesto). Secondary scores corrected throughout.

**Manifesto source files:** `manifestos/` contains both the original PDFs and extracted `.txt` files for all six parties. Use the `.txt` files for future content checks.

### VoteWise — Scottish Parliament 2026 Voting Guide (`scotland.html`)

Identical quiz structure for the 2026 Holyrood election.

**Parties covered:** Scottish Labour, Scottish Conservatives, SNP, Liberal Democrats, Scottish Greens, Reform UK Scotland

**Policy areas:** NHS Scotland, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Scottish Independence, Transport, Social Care, Policing & Justice, Agriculture & Fishing, Energy, Scottish Culture

**Content accuracy:** All 72 option/party pairings sourced from `manifestos-scotland/*.txt` (extracted from PDFs). Full audit conducted. SNP colour: `#D4A017` (gold). Reform Scotland Hate Crime Act abolition and SNP ferry/transport positions confirmed from manifesto text.

**Manifesto source files:** `manifestos-scotland/` contains PDFs and `.txt` files for all six parties.

### Shared infrastructure

**Tech stack:** Pure HTML/CSS/JS — no build system, no dependencies, works offline

**Design:** Clean editorial style. White (`#ffffff`) background, dark slate (`#1a2332`) text, teal (`#0d9488`) accent — chosen to be clearly distinct from Reform UK's cyan (`#12B6CF`). Light grey (`#f8f9fa`) section backgrounds, `#e5e7eb` borders. Inter (Google Fonts). Flat cards, minimal border radius, 3px teal progress bar.

**Election nav:** Both pages have a nav bar linking between Wales and Scotland quizzes, with the active election underlined in teal.

**Analytics:** Anonymous usage tracking via Counter.dev (script in `<head>`, no personal data).

**Logos:** Local SVG wordmarks in `logos/` — Wales: `lab.svg`, `con.svg`, `pc.svg`, `ld.svg`, `grn.svg`, `ref.svg`; Scotland: `slab.svg`, `scon.svg`, `snp.svg`, `sld.svg`, `sgrn.svg`, `sref.svg`.

**Social sharing:** `og-image.png` (1200×630) used by both quizzes — Wales-branded. Scotland currently uses the same image (see todos).

**Accessibility:** Answer options use `role="radio"`, `aria-checked`, and arrow-key navigation. Options grouped with `role="radiogroup"`.

**Hosting:** Live at `votewise.info` via GitHub Pages (repo made public) with Cloudflare DNS. CNAME file in repo root. If users report stale content, purge Cloudflare cache via dashboard → Caching → Purge Everything.

## No build system

No package manager, bundler, test runner, or linter. Add a Commands section when tooling is introduced.

## What still needs to be done

### Content
- [ ] Review whether a Welsh-language (`cy`) version is needed
- [ ] Consider a "why" explanation per question on results — showing which parties aligned with the user's answer

### Design / UX
- [x] ~~Create a Scotland-specific `og-image-scotland.png` (1200×630) and update `scotland.html` meta tags~~ — done; `og-image-scotland.png` generated with Scotland party colours
- [ ] Mobile hardware test: 380px breakpoint coded and reviewed; options stack vertically so 6-option layout is safe, but untested on a real device
- [x] ~~WCAG AA colour contrast check~~ — party colours are backgrounds only (bars, dots), not text; fallback logo boxes now use luminance-aware text colour (dark on LD/SNP/Reform/Con/Green, white on Labour)

### Features
- [ ] Consider a brief expandable policy explanation beneath each question

### Infrastructure
- [ ] Set up Prettier once the project grows further

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
- Options are shuffled per-question at startup using a deterministic seeded Fisher-Yates shuffle (seed = question index), so order varies across questions but is identical on every page load — shared URLs decode correctly
- After all 12 answers, each party's score is expressed as a percentage of their maximum possible score (each party can max at 36)
- Results show: winner card with match %, 2nd and 3rd place, all 6 parties ranked with animated bar chart
- Share via Web Share API (mobile) or Twitter fallback; copy-to-clipboard with fallback
- URL state: answers encoded as a 12-char `?r=` query param (valid chars `0–5` for 6 options) — results are directly shareable and bookmarkable
- Transparency note on results screen with links to all six official manifestos

**Parties covered:** Welsh Labour, Welsh Conservatives, Plaid Cymru, Liberal Democrats, Green Party, Reform UK

**Policy areas:** NHS Wales, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Devolution & Independence, Transport & Roads, Social Care, Policing & Justice, Agriculture & Farming, Energy, Welsh Language

**Content accuracy:** All 72 option/party pairings sourced directly from official manifesto text files (`manifestos/*.txt`, extracted from PDFs). Two full audits conducted — the second (April 2026) was a line-by-line check of every option against the source `.txt` files. Key corrections from the second audit:
- Transport (Greens): removed unsupported tidal claim; corrected to public transport doubling pledge
- Social Care (Plaid, Lib Dems, Greens): all three options rewritten against manifesto text
- Policing (Labour, Conservatives, Lib Dems): UK-level pledges replaced with devolved Welsh manifesto positions
- Agriculture (Labour, Plaid, Lib Dems, Reform): four options corrected; "guaranteed markets" and "direct subsidies" language not in manifestos
- Energy (Plaid, Lib Dems, Greens, Reform): four options corrected; Plaid net-zero target was misattributed, Greens tidal claim invented, Reform position understated
- Welsh Language (Reform): option rewritten against manifesto
- `ctx` (What's this about?) fields: Education fixed (Curriculum for Wales replaced curriculum framework, not SATs); Welsh Language fixed (Census figure 538,000/17.8%, not 900,000/29%)

**Manifesto source files:** `manifestos/` contains both the original PDFs and extracted `.txt` files for all six parties. Use the `.txt` files for future content checks.

### VoteWise — Scottish Parliament 2026 Voting Guide (`scotland.html`)

Identical quiz structure for the 2026 Holyrood election.

**Parties covered:** Scottish Labour, Scottish Conservatives, SNP, Liberal Democrats, Scottish Greens, Reform UK Scotland

**Policy areas:** NHS Scotland, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Scottish Independence, Transport, Social Care, Policing & Justice, Agriculture & Fishing, Energy, Scottish Culture

**Content accuracy:** All 72 option/party pairings sourced from `manifestos-scotland/*.txt` (extracted from PDFs). Full line-by-line audit conducted April 2026. Key corrections:
- NHS (Lib Dems, Greens, Reform): all three options rewritten against manifesto text
- Education (Labour, Conservatives, SNP, Greens): all four options rewritten; Labour "2,000 teachers" lacked "up to" and "specialist" qualifiers; SNP free meals overstated (primary only); Conservative option was invented
- Housing (SNP): "right for tenants to buy" corrected to "right of first refusal" per manifesto heading
- Climate (Greens): "100% community-owned renewables" corrected to "at least 1GW of community-owned projects by 2030"
- Economy (Reform): "significantly reduce business regulation" not in manifesto; replaced with actual pledges (income tax, business rates, LBTT)
- Independence (Labour): "devolution is delivering" contradicts manifesto; corrected to reflect SNP squandering of devolution
- Social Care (Conservatives, Lib Dems): Conservative "sustainable long-term funding" was Reform's language; Lib Dems "health hubs" corrected to "health teams"
- Policing (Conservatives): "longer sentences for repeat offenders" is Reform's pledge; corrected to Conservative pledges on sex offenders and Hate Crime Act
- Agriculture (Labour, SNP): "guaranteed secure markets" and "tenant farmer rights" not in manifestos
- Culture (Labour, Conservatives): Labour "fund" corrected to "pilot"; Conservative option replaced with actual Culture Act pledges
- `ctx` fields: Housing date corrected (2024 not 2023); Social Care updated (free personal care extended to all adults in 2019, not over-65s only)

**Manifesto source files:** `manifestos-scotland/` contains PDFs and `.txt` files for all six parties. Filenames: `Scottishlab.txt`, `Scottishcon.txt`, `SNP.txt`, `Scottishlibdems.txt`, `Scottishgreens.txt`, `Scottishreform.txt`.

### Shared infrastructure

**Tech stack:** Pure HTML/CSS/JS — no build system, no dependencies, works offline

**Design:** Clean editorial style. White (`#ffffff`) background, dark slate (`#1a2332`) text, teal (`#0d9488`) accent — chosen to be clearly distinct from Reform UK's cyan (`#12B6CF`). Light grey (`#f8f9fa`) section backgrounds, `#e5e7eb` borders. Inter (Google Fonts). Flat cards, minimal border radius, 3px teal progress bar.

**Results UI:** Winner card has a subtle party-colour background tint (4% opacity) and the match percentage number is coloured with the winning party's colour. Runner-up cards have a 3px coloured top border. Result bars are 8px tall. Answer options show A–F labels.

**Option order:** Options are shuffled per-question using a seeded Fisher-Yates algorithm (IIFE before the STATE block in each file). The seed is the question index (1-based), giving a different but consistent order per question. URL sharing is unaffected because the shuffle is deterministic.

**Election nav:** Both pages have a nav bar linking between Wales and Scotland quizzes, with the active election underlined in teal.

**Analytics:** Anonymous usage tracking via Counter.dev (script in `<head>`, no personal data).

**Logos:** Local SVG wordmarks in `logos/` — Wales: `lab.svg`, `con.svg`, `pc.svg`, `ld.svg`, `grn.svg`, `ref.svg`; Scotland: `slab.svg`, `scon.svg`, `snp.svg`, `sld.svg`, `sgrn.svg`, `sref.svg`.

**Social sharing:** `og-image.png` (1200×630) Wales-branded; `og-image-scotland.png` (1200×630) Scotland-branded. Both use dark slate design with teal accent and party colour bars.

**Accessibility:** Answer options use `role="radio"`, `aria-checked`, and arrow-key navigation. Options grouped with `role="radiogroup"`.

**Hosting:** Live at `votewise.info` via GitHub Pages (repo made public) with Cloudflare DNS. CNAME file in repo root. If users report stale content, purge Cloudflare cache via dashboard → Caching → Purge Everything.

## No build system

No package manager, bundler, test runner, or linter. Add a Commands section when tooling is introduced.

## What still needs to be done

### Infrastructure
- [ ] Set up Prettier once the project grows further

## Completed

- Welsh-language version — decided against
- Scotland OG image — `og-image-scotland.png` (1200×630) with Scottish party colours
- WCAG contrast — party colours are backgrounds only; fallback logo boxes use luminance-aware text
- Mobile — tested on real device, confirmed working
- Policy context toggles — "What's this about?" on every question (both quizzes)
- Answer breakdown on results — "How we matched you" section with per-question source links to manifestos
- Contact email — `hello@votewise.info` via Cloudflare Email Routing → Gmail; shown at bottom of results
- twitter:site meta tag removed (account does not exist)
- Visual polish — A–F option labels, coloured winner %, party tint on winner card, thicker bars, coloured runner-up borders
- Full line-by-line manifesto audit of all 24 policy sections (Wales + Scotland), April 2026
- `ctx` fact-check across all 24 sections, April 2026
- Seeded per-question option shuffle — prevents party order being predictable across questions

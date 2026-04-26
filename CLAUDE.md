# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

Private GitHub repo: `lloydy1982/politics-project`

**Commit and push frequently.** After every meaningful unit of work — new file, feature, fix, or config change — stage the relevant files and push to `origin/main`. Use clean, descriptive commit messages focused on the "why". Never let significant work sit uncommitted.

## What has been built

### VoteWise — Welsh Senedd 2026 Voting Guide (`index.html`)

A single-page, zero-dependency party-matching quiz branded **VoteWise**, built for the Welsh Senedd 2026 election.

**How it works:**
- Full-screen disclaimer/landing screen shown on first load (skipped when loading a shared URL)
- 12 questions across key Welsh policy areas
- Each answer option has a hidden score object mapping party IDs to points (0–3)
- After all 12 answers, each party's score is expressed as a percentage of their maximum possible score
- Results show: winner card with match %, 2nd and 3rd place, all 6 parties ranked with animated bar chart
- Share via Web Share API (mobile) or Twitter fallback; copy-to-clipboard with fallback
- URL state: answers encoded as a 12-char `?r=` query param — results are directly shareable and bookmarkable

**Parties covered:** Welsh Labour, Welsh Conservatives, Plaid Cymru, Liberal Democrats, Green Party, Reform UK

**Policy areas covered:** NHS Wales, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Devolution & Independence, Transport & Roads, Social Care, Policing & Justice, Agriculture & Farming, Energy, Welsh Language

**Tech stack:** Pure HTML/CSS/JS — no build system, no dependencies, works offline

**Design:** Clean editorial style. White (`#ffffff`) background, dark slate (`#1a2332`) text, mid-blue (`#2563eb`) accent on buttons/highlights, light grey (`#f8f9fa`) section backgrounds, `#e5e7eb` borders. Inter (Google Fonts) throughout. Flat cards, minimal border radius, 3px blue progress bar.

**Content accuracy:** All 48 answer option/party pairings were audited against the six official Welsh Senedd 2026 manifestos (April 2026). 14 text corrections and multiple scoring corrections were applied. Sources: official manifesto pages, ITV Wales, LabourList, State of Wales, CIEEM, Friends of the Earth Cymru, Will Hayward Wales newsletter. Lib Dem nuclear and Welsh language positions were resolved against available sources.

**Analytics:** Anonymous usage tracking via Counter.dev (script in `<head>`, no personal data).

**Logos:** Local SVG wordmarks in `logos/` (lab.svg, con.svg, pc.svg, ld.svg, grn.svg, ref.svg) — no external dependencies.

**Social sharing:** `og-image.png` (1200×630) in project root — matches current blue/white design, includes illustrative party result bars.

**Accessibility:** Answer options use `role="radio"`, `aria-checked`, and arrow-key navigation. Options grouped with `role="radiogroup"`.

## No build system

No package manager, bundler, test runner, or linter is set up yet. Add a Commands section to this file when tooling is introduced.

## What still needs to be done

### Content
- [ ] Verify Lib Dem Welsh language policy (Q12-B and Q12-C placements for LD are unconfirmed — manifesto PDF was not fully parsed)
- [ ] Consider adding a "why" explanation card per question on the results screen showing which parties aligned with the user's chosen answer
- [ ] Review whether the quiz needs a Welsh-language (`cy`) version

### Design / UX
- [ ] Mobile test: verify layout on real small-screen devices (380px breakpoint is coded but untested on hardware)
- [ ] Check colour contrast on party-coloured percentage figures in results (WCAG AA)

### Features
- [ ] Consider adding a brief policy explanation beneath each question (hidden by default, expandable)

### Infrastructure
- [ ] Decide on hosting — GitHub Pages requires the repo to be public (currently private); alternatives: Netlify drop, Vercel, or upgrade GitHub plan
- [ ] Add a custom domain if publishing publicly
- [ ] Set up Prettier once the project grows beyond a single file

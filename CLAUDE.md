# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

Private GitHub repo: `lloydy1982/politics-project`

**Commit and push frequently.** After every meaningful unit of work — new file, feature, fix, or config change — stage the relevant files and push to `origin/main`. Use clean, descriptive commit messages focused on the "why". Never let significant work sit uncommitted.

## What has been built

### Welsh Senedd 2026 Voting Guide (`index.html`)

A single-page, zero-dependency party-matching quiz for the Welsh Senedd 2026 election.

**How it works:**
- 12 questions across key Welsh policy areas
- Each answer option has a hidden score object mapping party IDs to points (0–3)
- After all 12 answers, each party's score is expressed as a percentage of their maximum possible score
- Results show: winner card with match %, 2nd and 3rd place, all 6 parties ranked with animated bar chart
- Share via Web Share API (mobile) or Twitter fallback; copy-to-clipboard with fallback

**Parties covered:** Welsh Labour, Welsh Conservatives, Plaid Cymru, Liberal Democrats, Green Party, Reform UK

**Policy areas covered:** NHS Wales, Education & Schools, Housing & Rent, Climate & Environment, Economy & Jobs, Devolution & Independence, Transport & Roads, Social Care, Policing & Justice, Agriculture & Farming, Energy, Welsh Language

**Tech stack:** Pure HTML/CSS/JS — no build system, no dependencies, works offline

**Design:** Editorial/professional style inspired by gov.uk and MIT Technology Review. Warm off-white background (`#f5f4f0`), dark masthead, Georgia serif headings, deep red accent (`#8b1a1a`), flat cards with minimal border radius, thin 2px progress bar.

**Content accuracy:** All 48 answer option/party pairings were audited against the six official Welsh Senedd 2026 manifestos (April 2026). 14 text corrections and 2 scoring corrections were applied. Sources used: official manifesto pages, ITV Wales, LabourList, State of Wales, CIEEM, Friends of the Earth Cymru, Will Hayward Wales newsletter.

## No build system

No package manager, bundler, test runner, or linter is set up yet. Add a Commands section to this file when tooling is introduced.

## What still needs to be done

### Content
- [ ] Verify Lib Dem Welsh language policy (their manifesto PDF was not fully parsed — Q12-B and Q12-C placements for LD are unconfirmed)
- [ ] Verify Lib Dem nuclear/energy policy (Q11-B nuclear support for LD is unconfirmed from available sources)
- [ ] Consider adding a "why" explanation card per question on the results screen showing which parties aligned with the user's chosen answer
- [ ] Review whether the quiz needs Welsh-language (`cy`) version

### Design / UX
- [ ] Party logos: currently loaded from Wikimedia URLs — consider hosting locally to avoid external dependency and logo breakage
- [ ] Mobile test: verify layout on small screens (380px breakpoint is coded but untested on real devices)
- [ ] Accessibility: add `aria-pressed` or `role="radio"` to answer option buttons; check colour contrast on party-coloured percentage figures
- [ ] Add a meta `og:image` for richer social sharing previews

### Features
- [ ] URL state: encode answers in the URL query string so results are shareable/bookmarkable directly
- [ ] Consider adding a brief policy explanation beneath each question (hidden by default, expandable)
- [ ] Analytics / usage tracking (privacy-respecting, e.g. Plausible or simple server-side counter) if the site is hosted publicly

### Infrastructure
- [ ] Decide on hosting (GitHub Pages is simplest given the repo setup)
- [ ] Add a custom domain if publishing publicly
- [ ] Set up a linter / formatter (Prettier) once the project grows beyond a single file

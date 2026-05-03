# Phase 1 — Content & Positioning Refresh

**Date:** 2026-05-04
**Site:** dayoajayi.github.io
**Scope:** Single-file edit to `index.html`. No CSS, asset, or stack changes.

## Goal

Refresh the personal portfolio site so it (a) stops shipping stale and broken content (typos, missing dates, dead Universal Analytics, incorrect employment year), and (b) positions Dayo for inbound freelance and project engagements while building GlowConsult.ai. End state: the site is sendable to `jobs@obcido.com` as a working "resume" within the week.

## Non-goals (deferred to later phases)

- Visual redesign — typography, color, layout, dark mode (Phase 2)
- Stack modernization — jQuery 3.4.1, Bootstrap 4, dual FontAwesome (Phase 2)
- New structural sections — Selected Work, Writing/Talks (Phase 2/3)
- Re-enabling analytics — GA4 or a privacy-friendly alternative (Phase 3)
- SCSS recompile — not needed; all changes are HTML-only

## Positioning context

- **Audience:** prospective freelance / consulting clients (B-leaning, not active job hunt). Near-term, may be forwarded to `jobs@obcido.com` by a referral.
- **GlowConsult treatment:** "B" — visible disclosure as context for why Dayo is freelance-only; not foregrounded as founder-first.
- **Engagement type:** "D" — implementation-heavy + mid-range delivery. Open to obcido-style hands-on dev/webmaster work AND multi-week project ownership. Not primarily senior-advisory.
- **Capability triple:** AI data engineering · API design and integration · application architecture and modernization.
- **Overall positioning thesis:** "deeply understands AI and can deploy it within an org to generate value."

## Repo publication boundaries (prerequisite)

GitHub Pages serves every file from this repo's root. To keep design docs and editor scratch out of the public site (decision: Option A from the 2026-05-04 review), Phase 1 also adds two top-level config files:

### 0a. New file: `_config.yml`

Adds a minimal Jekyll config whose only job is to exclude paths from the deployed site. Activating Jekyll has no expected effect on `index.html` (it's served as-is — no front matter triggers processing) or on `assets/` (referenced paths use no underscore-prefixed folders that Jekyll would auto-exclude).

```yaml
exclude:
  - docs/
  - CLAUDE.md
  - README.md
  - .superpowers/
```

### 0b. New file: `.gitignore`

Keeps brainstorm scratch and OS metadata out of git. Existing tracked `.DS_Store` files (if any) are left in place — untracking is out of scope for Phase 1.

```
.DS_Store
.superpowers/
```

## Changes to `index.html`

### 1. `<head>` updates

| Element | Action |
|---|---|
| `<title>` | Set to `Dayo Ajayi — Software Engineer & AI Consultant` |
| `<meta name="description">` | Set to `Software engineer & consultant with ~15 years of experience. Available for select freelance and project engagements — AI deployment, data engineering, and application modernization.` |
| `<meta name="keywords">` | Set to `Software Engineer, AI Consultant, AI Deployment, AI Data Engineering, API Design, Application Modernization, Cloud Architecture, Freelance, Dallas` |
| Universal Analytics block (lines 4–12, the `gtag.js` script + `gtag('config','UA-137365938-1')`) | **Remove entirely** |
| `google-site-verification` meta | **Keep** unchanged |

### 2. Header (`.profile-content`) copy

Replace the existing three lines (`<h1 class="name">`, `<h2 class="desc">`, `<h4 class="desc1">`) with this block, reusing the existing `desc1` class for the new lines (no CSS work):

```
Dayo Ajayi                                        [h1.name, unchanged]
Software Engineer & AI Consultant                 [h2.desc — typo fixed, role refined]

AI data engineering, API integration, and          [h4.desc1 — NEW]
application modernization — available for
select freelance and project engagements.

Currently building GlowConsult.ai — AI voice       [h4.desc1 — NEW; "GlowConsult.ai"
agents for aesthetics practices.                    is a link to https://glowconsult.ai]

MBA, UTexas McCombs  |  MS Computer Engineering,  [h4.desc1 — existing credentials line,
University of Kansas                                kept verbatim]
```

**Implementation note:** Stacked `<h4 class="desc1">` lines may render densely; visual hierarchy refinement is a Phase 2 concern.

### 3. Social links (`ul.social`)

- LinkedIn `<li>` — keep
- Twitter `<li>` (containing `<i class="fab fa-twitter">`) — **remove entirely**
- GitHub `<li>` — keep

### 4. About Me section

Replace both `<p>` elements inside `.about .content` with three new paragraphs:

> Software engineer and consultant with ~15 years of experience working at the intersection of people, process, and technology. Former management consultant (Deloitte) with an MBA — equally comfortable shipping production code as I am engaging technical and business stakeholders.

> I help organizations deploy AI in ways that actually generate value — across **AI data engineering**, **API design and integration**, and **application architecture and modernization**. The hardest part of AI in most orgs isn't the model; it's the data, the integration surface, and the architectural decisions that determine whether AI makes it from prototype to production.

> Currently building **[GlowConsult.ai](https://glowconsult.ai)** — AI voice agents for aesthetics practices — and taking select freelance and project engagements alongside it. If you have something that fits, [get in touch](mailto:dayo.ajayi@gmail.com).

(Markdown shown for readability; rendered as HTML with `<a>` and `<strong>`.)

### 5. Work Experience — restructure

#### 5a. Add new top entry: GlowConsult.ai

Insert as the **first** `<div class="item">` inside `.experience .content`. Markup pattern matches the existing VMware entry shape (h3.title + place + year span, then a `<b>` role line, then a `<ul>`).

```
GLOWCONSULT.AI                  Dallas, TX   Dec 2024 – Present
Founder & Engineer
- Building AI voice agents for aesthetics practices —
  call answering, appointment booking, lead capture, 24/7 coverage.
- End-to-end ownership: data engineering, voice/LLM integration,
  architecture, and production deployment.
```

(No "Technologies Used" trailer needed — the bullet describes the stack at the right level.)

#### 5b. Single-source-of-truth for dates

The existing entries store dates in two places: an empty `<span class="year">` and a `<b>Role (year - year)</b>` line. Phase 1 consolidates this. For each kept entry:

- Populate `<span class="year">` with month-precision date range (e.g. `Sep 2021 – Dec 2024`).
- Strip the `(year - year)` parenthetical from the `<b>` role line — leaving just the role title.

This puts dates in the template's intended location (`.year` is a styled class) and removes redundancy.

#### 5c. Updated dates for kept entries

| Entry | Span value (`<span class="year">`) | `<b>` line becomes |
|---|---|---|
| VMware Tanzu Labs (formerly Pivotal Labs) | `Sep 2021 – Dec 2024` | `Senior Software Engineer` |
| Solera Holdings | `Aug 2017 – Mar 2021` | `Senior Software Engineer` (also fixes "Sofware" typo and incorrect "2018" start year) |
| Denbury Resources | `Oct 2013 – Aug 2017` | `Programmer Analyst III` |

#### 5d. Condense (option ii — selected by user)

Remove the existing three separate `<div class="item">` blocks for Sogeti USA, FHLBank Topeka, and Deloitte Consulting (lines ~122–153). Replace with a single `<div class="item">` — title row plus an `<h4 class="university">` employer-list row, no bullets:

```
Earlier Career                                          Aug 2007 – Oct 2013
Sogeti USA (Irving, TX) · FHLBank Topeka · Deloitte Consulting (Kansas City, MO)
```

This change incidentally repairs a pre-existing markup bug: in the current HTML, those three entries' `<div class="item">` openings are not closed by their own `</div>` — they are absorbed by the parent container's `</div><!--//content-->`. The condensation removes the malformed blocks entirely.

### 6. Out of scope (Phase 1)

- All sidebar sections — Basic Info, Education, Skills & Tools, Volunteer Experience, Hobbies & Interests
- Footer
- All CSS, JS, and asset files (including `assets/css/styles.css`, `assets/scss/`, `assets/js/main.js`)
- All vendored plugins (jQuery, Bootstrap, popper, jquery-rss, FontAwesome)

## Acceptance criteria

After Phase 1:

- [ ] No occurrence of `Sofware` anywhere in `index.html`
- [ ] All retained work-history entries have a non-empty `<span class="year">` with month-precision dates
- [ ] Solera start year is corrected from `2018` to `Aug 2017`
- [ ] About Me section reads as positioned for AI consulting + freelance availability (no longer "10 years")
- [ ] GlowConsult.ai is mentioned in three places: header sub-line, About paragraph, and as the first Work Experience entry
- [ ] Universal Analytics gtag.js block is removed from `<head>`
- [ ] Twitter `<li>` is removed from social list; LinkedIn and GitHub remain
- [ ] Sogeti / FHLBank / Deloitte are condensed to a single "Earlier Career" entry
- [ ] Site loads and renders correctly when `index.html` is opened locally (manual smoke test in browser)
- [ ] No CSS recompilation required; `assets/css/styles.css` is untouched
- [ ] `_config.yml` exists at repo root with `exclude:` covering `docs/`, `CLAUDE.md`, `README.md`, `.superpowers/`
- [ ] `.gitignore` exists at repo root with entries for `.DS_Store` and `.superpowers/`
- [ ] After publication boundaries are in place, `https://dayoajayi.github.io/CLAUDE.md` and `https://dayoajayi.github.io/docs/superpowers/specs/...` return 404 (verify after deploy)
- [ ] Output is a small set of commits — at minimum: (1) `_config.yml` + `.gitignore` + spec doc, (2) `index.html` content edits

## Risks and known issues

- **Header density.** The four stacked `<h4 class="desc1">` lines may look visually crowded. Acceptable for Phase 1; visual hierarchy is a Phase 2 concern.
- **Template still recognizable.** The third-party "Developer" template aesthetic (Bootstrap 4, two-column card layout) remains visible. Acceptable for Phase 1.
- **Lost detail in condensed entries.** Bullet content for Sogeti / FHLBank / Deloitte (achievements, technologies) is removed from the live page. Source of truth remains in git history; can be restored via Phase 2 case-study work if needed.
- **Jekyll activation side-effects.** Adding `_config.yml` activates Jekyll on this site for the first time. `index.html` (no front matter) and `assets/` content are expected to pass through unchanged. The only known auto-exclusion is folders starting with `_` — `assets/plugins/_jquery-rss/` is already an unused vendor copy (the live one is `assets/plugins/jquery-rss/`), so its exclusion is benign. Worth verifying after first deploy that the live site looks identical to before.
- **DS_Store files already in repo.** `.DS_Store` files exist in the working tree from prior commits. Phase 1's `.gitignore` prevents new ones from being tracked but leaves existing tracked copies in place. Untracking them is a separate small cleanup (not Phase 1 scope).

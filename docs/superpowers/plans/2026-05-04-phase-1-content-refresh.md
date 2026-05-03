# Phase 1 — Content & Positioning Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refresh `dayoajayi.github.io` so it (a) reflects current employment status (GlowConsult.ai full-time since Dec 2024), (b) positions Dayo for inbound freelance + project engagements (B + D positioning), and (c) sets up GitHub Pages publication boundaries so `docs/`, `CLAUDE.md`, and `.superpowers/` are not served on the live site.

**Architecture:** Single-file content edit to `index.html` plus two new top-level config files (`_config.yml`, `.gitignore`). All purely textual; no CSS, JS, or asset changes. Adding `_config.yml` activates Jekyll on this site for the first time, but with no front matter on `index.html`, Jekyll passes the existing site through unchanged.

**Tech Stack:** GitHub Pages, Jekyll (newly activated minimal config), HTML5, Bootstrap 4 (untouched), jQuery (untouched), FontAwesome (untouched).

**Spec:** [`docs/superpowers/specs/2026-05-04-phase-1-content-refresh-design.md`](../specs/2026-05-04-phase-1-content-refresh-design.md)

---

## File Structure

| File | Action | Responsibility |
|---|---|---|
| `_config.yml` | New | Jekyll site config — exclude paths from public serving |
| `.gitignore` | New | Exclude OS metadata and brainstorm scratch from git |
| `index.html` | Modify | All content changes (head, header, About, Work Experience) |
| `CLAUDE.md` | Modify | Update architecture description to reflect Jekyll activation |

## Verification approach

This is a static site with no automated test framework. Verification per task = a combination of:

- Successful `Edit` tool execution (proves the `old_string` was actually present, catching drift between plan and file)
- `grep`-based assertions for keywords (e.g. no `Sofware`, GlowConsult mentioned N times)
- Browser smoke test by opening `index.html` locally

Final task includes post-deploy verification of the live site.

---

## Task 1: Add publication boundaries (`_config.yml`, `.gitignore`)

**Files:**
- Create: `/Users/dayoajayi/workspace/dayoajayi.github.io/_config.yml`
- Create: `/Users/dayoajayi/workspace/dayoajayi.github.io/.gitignore`

- [ ] **Step 1: Create `_config.yml`**

Use Write tool to create `/Users/dayoajayi/workspace/dayoajayi.github.io/_config.yml` with content:

```yaml
exclude:
  - docs/
  - CLAUDE.md
  - README.md
  - .superpowers/
```

- [ ] **Step 2: Create `.gitignore`**

Use Write tool to create `/Users/dayoajayi/workspace/dayoajayi.github.io/.gitignore` with content:

```
.DS_Store
.superpowers/
```

- [ ] **Step 3: Verify both files exist and look right**

Run: `ls -la /Users/dayoajayi/workspace/dayoajayi.github.io/_config.yml /Users/dayoajayi/workspace/dayoajayi.github.io/.gitignore`

Expected: both files listed, non-zero size.

Run: `git status`

Expected: `_config.yml`, `.gitignore`, and `docs/` (containing the spec + this plan) appear as new untracked files. `.superpowers/` does **not** appear (now ignored).

- [ ] **Step 4: Smoke-test that local page still renders**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: the site renders in the browser as it did before the change. (Local `file://` rendering is unaffected by Jekyll config; this just confirms we didn't accidentally damage anything else in the working tree.)

- [ ] **Step 5: Commit**

```bash
git add _config.yml .gitignore docs/superpowers/specs/2026-05-04-phase-1-content-refresh-design.md docs/superpowers/plans/2026-05-04-phase-1-content-refresh.md
git commit -m "$(cat <<'EOF'
Add Jekyll publication boundaries and Phase 1 spec/plan

- _config.yml excludes docs/, CLAUDE.md, README.md, .superpowers/ from the deployed site
- .gitignore keeps .DS_Store and .superpowers/ out of git
- Spec and plan committed under the now-excluded docs/ path

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Run: `git status`

Expected: clean working tree (or only contains uncommitted changes from later tasks not yet started).

---

## Task 2: Remove Universal Analytics + update head metadata

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html` (lines 1–23)

- [ ] **Step 1: Read the current head section to confirm content**

Use Read tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html` with `offset: 1`, `limit: 25`. Confirm lines 4–14 contain the gtag.js block ending in `gtag('config', 'UA-137365938-1');`.

- [ ] **Step 2: Remove the Universal Analytics block and update title**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
<head>
    	<!-- Global site tag (gtag.js) - Google Analytics -->
	<script async src="https://www.googletagmanager.com/gtag/js?id=UA-137365938-1"></script>
	<script>
	  window.dataLayer = window.dataLayer || [];
	  function gtag(){dataLayer.push(arguments);}
	  gtag('js', new Date());

	  gtag('config', 'UA-137365938-1');
    </script>

    <title>Dayo Ajayi - Sofware Engineer | Consultant </title>
```

`new_string`:
```
<head>
    <title>Dayo Ajayi — Software Engineer &amp; AI Consultant</title>
```

- [ ] **Step 3: Update meta description**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
    <meta name="description" content="Dayo Ajayi" />
```

`new_string`:
```
    <meta name="description" content="Software engineer &amp; consultant with ~15 years of experience. Available for select freelance and project engagements — AI deployment, data engineering, and application modernization." />
```

- [ ] **Step 4: Update meta keywords**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
    <meta name="keywords" content="Sofware Engineer, Researcher, Research & Development Engineer, Full Stack Developer, Python Developer, Open Source Software,
                                  Freelancer, Volunteering" />
```

`new_string`:
```
    <meta name="keywords" content="Software Engineer, AI Consultant, AI Deployment, AI Data Engineering, API Design, Application Modernization, Cloud Architecture, Freelance, Dallas" />
```

- [ ] **Step 5: Verify head changes**

Run: `grep -n -i "sofware\|UA-137365938" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: exactly one match — the `Senior Sofware Engineer` line in the Solera entry, which is fixed in Task 6. Zero matches for `UA-137365938`.

Run: `grep -n "AI Consultant" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: at least one match in the `<title>`.

- [ ] **Step 6: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: page still renders. Browser tab shows new title `Dayo Ajayi — Software Engineer & AI Consultant`. No console errors related to missing `gtag` (the script is gone).

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Drop dead Universal Analytics tag and refresh head metadata

UA-* was shut down by Google in July 2023; tag was collecting
nothing. Title, description, and keywords now reflect AI/freelance
positioning (per Phase 1 spec).

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 3: Header copy refresh + remove Twitter

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html` (the `.profile-content` block)

- [ ] **Step 1: Update the role line and add capabilities + GlowConsult sub-lines**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
            <div class="profile-content float-left">
                <h1 class="name">Dayo Ajayi</h1>
                <h2 class="desc">Sofware Engineer | Consultant </h2>
                <h4 class="desc1">MBA, UTexas McCombs | MS Computer Engineering, University of Kansas</h4>
                <ul class="social list-inline">
                    <li class="list-inline-item"><a href="https://www.linkedin.com/in/temidayo/" target="_blank"><i class="fab fa-linkedin"></i></i></a></li>
                    <li class="list-inline-item"><a href="https://twitter.com/idayoajayi" target="_blank"><i class="fab fa-twitter"></i></a></li>
                    <li class="list-inline-item"><a href="https://github.com/dayoajayi" target="_blank"><i class="fab fa-github"></i></a></li>
                </ul>
            </div><!--//profile-->
```

`new_string`:
```
            <div class="profile-content float-left">
                <h1 class="name">Dayo Ajayi</h1>
                <h2 class="desc">Software Engineer &amp; AI Consultant</h2>
                <h4 class="desc1">AI data engineering, API integration, and application modernization &mdash; available for select freelance and project engagements.</h4>
                <h4 class="desc1">Currently building <a href="https://glowconsult.ai">GlowConsult.ai</a> &mdash; AI voice agents for aesthetics practices.</h4>
                <h4 class="desc1">MBA, UTexas McCombs | MS Computer Engineering, University of Kansas</h4>
                <ul class="social list-inline">
                    <li class="list-inline-item"><a href="https://www.linkedin.com/in/temidayo/" target="_blank"><i class="fab fa-linkedin"></i></a></li>
                    <li class="list-inline-item"><a href="https://github.com/dayoajayi" target="_blank"><i class="fab fa-github"></i></a></li>
                </ul>
            </div><!--//profile-->
```

(Notes: this single edit also (a) removes the malformed double `</i></i>` in the LinkedIn link, (b) removes the Twitter `<li>` entirely, (c) adds the two new `<h4 class="desc1">` lines for capabilities and GlowConsult disclosure, and (d) fixes the "Sofware" typo in the role line.)

- [ ] **Step 2: Verify header changes**

Run: `grep -n "twitter\|fa-twitter" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: zero matches.

Run: `grep -n "GlowConsult.ai" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: exactly one match at this point (the header sub-line). Two more will be added in later tasks (About paragraph + Work Experience entry).

Run: `grep -n "AI data engineering" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: at least one match (header capabilities line).

- [ ] **Step 3: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: header now shows the new tagline, two new sub-lines, credentials, and only LinkedIn + GitHub social icons. Visual density is acknowledged in the spec — Phase 2 will refine.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Refresh header copy for AI consulting + freelance positioning

- Headline: Software Engineer & AI Consultant (typo fixed)
- New sub-line: capabilities triple + availability statement
- New sub-line: Currently building GlowConsult.ai
- Removed Twitter/X social link
- Cleaned malformed double </i> in LinkedIn anchor

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 4: About Me rewrite

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html` (the `.about .content` block)

- [ ] **Step 1: Replace both About paragraphs with three new ones**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                        <div class="content">
                            <p>Seasoned software engineer with 10 years of experience who thrives at the intersection of People, Process, and Technology. Former Management Consultant with an MBA. Equally comfortable in the bits-and-bytes as I am with technical colleagues and business stakeholders.</p>
                            <p>Passionate about solving business challenges by designing efficient and high-performance APIs, creating engaging user experiences with modern JavaScript frameworks, and driving application modernization through microservices architecture and cloud migration using platforms like AWS, Azure, and GCP.</p>

                        </div><!--//content-->
                    </div><!--//section-inner-->
                </section><!--//section-->

                <section class="experience section">
```

`new_string`:
```
                        <div class="content">
                            <p>Software engineer and consultant with ~15 years of experience working at the intersection of people, process, and technology. Former management consultant (Deloitte) with an MBA &mdash; equally comfortable shipping production code as I am engaging technical and business stakeholders.</p>
                            <p>I help organizations deploy AI in ways that actually generate value &mdash; across <strong>AI data engineering</strong>, <strong>API design and integration</strong>, and <strong>application architecture and modernization</strong>. The hardest part of AI in most orgs isn&rsquo;t the model; it&rsquo;s the data, the integration surface, and the architectural decisions that determine whether AI makes it from prototype to production.</p>
                            <p>Currently building <strong><a href="https://glowconsult.ai">GlowConsult.ai</a></strong> &mdash; AI voice agents for aesthetics practices &mdash; and taking select freelance and project engagements alongside it. If you have something that fits, <a href="mailto:dayo.ajayi@gmail.com">get in touch</a>.</p>
                        </div><!--//content-->
                    </div><!--//section-inner-->
                </section><!--//section-->

                <section class="experience section">
```

- [ ] **Step 2: Verify**

Run: `grep -n "10 years" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: zero matches.

Run: `grep -c "GlowConsult.ai" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 2 (header sub-line + About paragraph; the third — Work Experience entry — comes in Task 5).

Run: `grep -n "Passionate about solving business challenges" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: zero matches (old paragraph removed).

- [ ] **Step 3: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: About Me section now shows three paragraphs ending with a "get in touch" mailto link. AI capability terms render bold. GlowConsult.ai link is clickable.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Rewrite About Me for AI consulting positioning

Three paragraphs: background + credibility, AI deployment thesis +
capability triple, current focus + soft CTA. Replaces generic
"Passionate about..." copy with thesis-led framing.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 5: Add GlowConsult.ai work-experience entry (top of list)

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html` (`.experience .content`)

- [ ] **Step 1: Insert new GlowConsult entry above the existing VMware entry**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                        <div class="content">
                            <div class="item">
                                <h3 class="title">VMWARE TANZU LABS (formerly Pivotal Labs)<span class="place"> Palo Alto, CA</span>
                                    <span class="year"></span></h3>
                                <b>Senior Software Engineer (2021 - 2024)</b><br>
```

`new_string`:
```
                        <div class="content">
                            <div class="item">
                                <h3 class="title">GLOWCONSULT.AI<span class="place"> Dallas, TX</span>
                                    <span class="year">Dec 2024 &ndash; Present</span></h3>
                                <b>Founder &amp; Engineer</b><br>
                                <ul>
                                    <li>Building AI voice agents for aesthetics practices &mdash; call answering, appointment booking, lead capture, and 24/7 coverage.</li>
                                    <li>End-to-end ownership: data engineering, voice/LLM integration, architecture, and production deployment.</li>
                                </ul>
                            </div><!--//item-->

                            <div class="item">
                                <h3 class="title">VMWARE TANZU LABS (formerly Pivotal Labs)<span class="place"> Palo Alto, CA</span>
                                    <span class="year"></span></h3>
                                <b>Senior Software Engineer (2021 - 2024)</b><br>
```

- [ ] **Step 2: Verify**

Run: `grep -c "GlowConsult\|GLOWCONSULT" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 3 (header + About + Work Experience entry).

Run: `grep -n "Founder &amp; Engineer\|Founder & Engineer" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 1 match.

- [ ] **Step 3: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: Work Experience section now leads with GLOWCONSULT.AI as the first entry, with role, date range, and two-bullet description. VMware entry follows as the second item.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add GlowConsult.ai as current Work Experience entry

Dec 2024 – Present, Founder & Engineer. Two bullets covering the
product (AI voice agents for aesthetics) and ownership scope
(end-to-end: data, integration, architecture, deployment).

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 6: Populate dates and clean role lines for retained entries (VMware, Solera, Denbury)

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

For each of the three retained entries, the goal is the same: put the month-precision date range into `<span class="year">`, and trim the `(year - year)` parenthetical (and any "Sofware" typo) out of the `<b>` role line so the date is in one place.

- [ ] **Step 1: VMware Tanzu Labs — set year span and trim role line**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                                <h3 class="title">VMWARE TANZU LABS (formerly Pivotal Labs)<span class="place"> Palo Alto, CA</span>
                                    <span class="year"></span></h3>
                                <b>Senior Software Engineer (2021 - 2024)</b><br>
```

`new_string`:
```
                                <h3 class="title">VMWARE TANZU LABS (formerly Pivotal Labs)<span class="place"> Palo Alto, CA</span>
                                    <span class="year">Sep 2021 &ndash; Dec 2024</span></h3>
                                <b>Senior Software Engineer</b><br>
```

- [ ] **Step 2: Solera Holdings — set year span, fix typo, fix start year**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                            <div class="item">
                                <h3 class="title">SOLERA HOLDINGS INC<span class="place"> Westlake, TX</span>
                                     <span class="year"></span></h3>
                                     <b>Senior Sofware Engineer (2018 - 2021)</b><br>
```

`new_string`:
```
                            <div class="item">
                                <h3 class="title">SOLERA HOLDINGS INC<span class="place"> Westlake, TX</span>
                                     <span class="year">Aug 2017 &ndash; Mar 2021</span></h3>
                                     <b>Senior Software Engineer</b><br>
```

(Note: this fixes both the "Sofware" typo and the incorrect "2018" start year — Dayo confirmed Aug 2017.)

- [ ] **Step 3: Denbury Resources — set year span and trim role line**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                            <div class="item">
                                <h3 class="title">DENBURY RESOURCES <span class="place"> Plano, TX</span>
                                     <span class="year"></span></h3>
                                     <b>Programmer Analyst III (2013 - 2017)</b><br>
```

`new_string`:
```
                            <div class="item">
                                <h3 class="title">DENBURY RESOURCES <span class="place"> Plano, TX</span>
                                     <span class="year">Oct 2013 &ndash; Aug 2017</span></h3>
                                     <b>Programmer Analyst III</b><br>
```

- [ ] **Step 4: Verify**

Run: `grep -c "Sofware" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 0.

Run: `grep -n "<span class=\"year\"></span>" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: zero matches in the retained entries (Sogeti, FHLBank, Deloitte still have empty year spans at this step but they're removed in Task 7).

Run: `grep -n "Sep 2021\|Aug 2017\|Oct 2013" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 3 matches, one per retained entry.

- [ ] **Step 5: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: VMware, Solera, Denbury entries now show date ranges (the `.year` styling will apply — likely right-aligned or italic depending on theme CSS). Role lines no longer have date parentheticals.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Populate Work Experience dates with month precision; fix typos

- VMware Tanzu Labs: Sep 2021 – Dec 2024
- Solera Holdings: Aug 2017 – Mar 2021 (start year corrected from 2018)
- Denbury Resources: Oct 2013 – Aug 2017
- Removed redundant date parentheticals from <b> role lines
- Fixed "Sofware" → "Software" typo in Solera entry

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 7: Condense earlier career (Sogeti + FHLBank + Deloitte → single entry)

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

This task replaces three malformed `<div class="item">` blocks (whose openings are incorrectly closed by the parent's `</div><!--//content-->`) with a single well-formed entry.

- [ ] **Step 1: Replace the three earlier-career blocks with one condensed entry**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/index.html`:

`old_string`:
```
                            <div class="item">
                                <h3 class="title">SOGETI USA LLC <span class="place"> Irving, TX</span>
                                     <span class="year"></span></h3>
                                     <b>Senior Consultant (2012 - 2013)</b><br>
                                    <ul>
                                        <li>Redesigned a global transportation client’s eCommerce product by leading requirement-gathering sessions and managing relationships among cross-functional stakeholders; reduced downtime by 5x and increased revenue by 15%</li>
                                        <li>Provided guidance on deliverables and provided technical leadership to Fortune 1000, finishing project 45 days early</li>
                                    </ul>
                                    <b><u>Technologies Used:</u></b> C#, MS SQL Server, HTML/CSS, Search Engine Optimization (SEO), Load Balancers
                        </div><!--//content-->

                        <div class="item">
                            <h3 class="title">FHLBank Topeka<span class="place"> Topeka, KS</span>
                                 <span class="year"></span></h3>
                                 <b>Programmer Analyst II (2009 - 2012)</b><br>
                                 <ul>
                                    <li>Consolidated bank’s code base of 100 applications written on the .NET Framework using C#, reducing costs by $500K</li>
                                 </ul>                                 
                                 <b><u>Technologies Used:</u></b> C#, Tableau, Kalido Data Warehouse, Master Data Management (MDM), Business Intelligence (BI)
                    </div><!--//content-->

                    <div class="item">
                        <h3 class="title">DELOITTE CONSULTING LLC <span class="place"> Kansas City, MO</span>
                             <span class="year"></span></h3>
                             <b>Business Technology Analyst (2007 - 2009)</b><br>
                             <ul>
                                 <li>Automated process of disbursing disaster relief funds and reduced processing time from weeks to minutes</li>
                                 <li>Awarded Deloitte’s Distinguished Client Service Award after a series of successful project implementations</li>
                             </ul>
                             
                             <b><u>Technologies Used:</u></b> C#, MS SQL Server, HTML/CSS, Search Engine Optimization (SEO), Google Analytics
                </div><!--//content-->

                    </div><!--//section-inner-->
```

`new_string`:
```
                            <div class="item">
                                <h3 class="title">EARLIER CAREER<span class="year">Aug 2007 &ndash; Oct 2013</span></h3>
                                <h4 class="university">Sogeti USA (Irving, TX) &middot; FHLBank Topeka &middot; Deloitte Consulting (Kansas City, MO)</h4>
                            </div><!--//item-->

                        </div><!--//content-->
                    </div><!--//section-inner-->
```

(Note: the new structure properly closes `<div class="item">` with its own `</div>` before closing `.content` and `.section-inner`. This repairs the pre-existing markup nesting bug for the deleted entries.)

- [ ] **Step 2: Verify markup is now well-formed**

Run: `grep -c "Sogeti\|FHLBank\|Deloitte" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 2 — one match in the About paragraph (`Former management consultant (Deloitte)`) and one in the condensed Earlier Career employer-list line.

Run: `grep -n "EARLIER CAREER\|Earlier Career" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: 1 match.

Run: `python3 -c "import html.parser; html.parser.HTMLParser().feed(open('/Users/dayoajayi/workspace/dayoajayi.github.io/index.html').read())"`

Expected: no exception. (Best-effort sanity check; the standard library parser is permissive but will catch grossly malformed structure.)

- [ ] **Step 3: Smoke-test in browser**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Expected: Work Experience section now ends with a single "EARLIER CAREER" entry (single line under the title listing the three employers). No visual regressions in other sections.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Condense earlier career to single entry

Removes separate Sogeti / FHLBank / Deloitte entries (Aug 2007 –
Oct 2013) in favor of a single "Earlier Career" line. Preserves
the consulting-background narrative without dominating recent
work visually. Incidentally repairs malformed div nesting from
the original three blocks.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 8: Update CLAUDE.md to reflect Jekyll activation

**Files:**
- Modify: `/Users/dayoajayi/workspace/dayoajayi.github.io/CLAUDE.md`

The current CLAUDE.md says the site has "no Jekyll config" and "no build pipeline." After Phase 1, both statements are partially false: a minimal `_config.yml` now exists, and Jekyll is technically the build pipeline. Update for accuracy.

- [ ] **Step 1: Update the "What this repo is" paragraph**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/CLAUDE.md`:

`old_string`:
```
A single-page static personal portfolio site for Dayo Ajayi, deployed at https://dayoajayi.github.io/. Because the repo is named `<username>.github.io`, GitHub Pages serves the root directly — there is no Jekyll config (`_config.yml`) and no build pipeline. Pushing to `master` is the deploy.
```

`new_string`:
```
A single-page static personal portfolio site for Dayo Ajayi, deployed at https://dayoajayi.github.io/. Because the repo is named `<username>.github.io`, GitHub Pages serves the root directly. A minimal `_config.yml` activates Jekyll only to use its `exclude:` directive — `index.html` and `assets/` pass through unchanged; there's no actual build step in the developer sense. Pushing to `master` is the deploy.
```

- [ ] **Step 2: Add a note to "Working on the site" about which paths are excluded from publication**

Use Edit tool on `/Users/dayoajayi/workspace/dayoajayi.github.io/CLAUDE.md`:

`old_string`:
```
To deploy: commit and push to `master`. GitHub Pages picks it up automatically.
```

`new_string`:
```
To deploy: commit and push to `master`. GitHub Pages picks it up automatically.

`_config.yml` excludes `docs/`, `CLAUDE.md`, `README.md`, and `.superpowers/` from the deployed site, so design specs and editor scratch files stay version-controlled but are not web-accessible. If you add a new directory you don't want served (e.g. internal notes), add it to the `exclude:` list.
```

- [ ] **Step 3: Verify**

Run: `grep -n "no Jekyll config\|no build pipeline" /Users/dayoajayi/workspace/dayoajayi.github.io/CLAUDE.md`

Expected: zero matches.

Run: `grep -n "_config.yml" /Users/dayoajayi/workspace/dayoajayi.github.io/CLAUDE.md`

Expected: at least 2 matches (in the architecture paragraph and the working-on-the-site paragraph).

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md
git commit -m "$(cat <<'EOF'
Update CLAUDE.md to reflect Jekyll activation in Phase 1

Architecture description previously claimed "no Jekyll config";
that's no longer true. Documented the new _config.yml exclude
list so future edits know which paths are kept off the live site.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 9: Pre-deploy verification + push

**Files:** none modified; verification only.

- [ ] **Step 1: Final local smoke test**

Run: `open /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`

Visual checklist:
- Header: name, "Software Engineer & AI Consultant", capabilities/availability line, GlowConsult line, credentials, two social icons (LinkedIn, GitHub).
- About Me: three paragraphs ending in "get in touch" mailto link.
- Work Experience entries in order: GlowConsult.ai (Dec 2024–Present), VMware Tanzu Labs (Sep 2021–Dec 2024), Solera Holdings (Aug 2017–Mar 2021), Denbury Resources (Oct 2013–Aug 2017), Earlier Career (Aug 2007–Oct 2013).
- Sidebar: unchanged (Education, Skills & Tools, Volunteer, Hobbies all present).

- [ ] **Step 2: Aggregate grep checks**

Run: `grep -c "Sofware" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 0.

Run: `grep -c "UA-137365938" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 0.

Run: `grep -c "twitter\|fa-twitter" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 0.

Run: `grep -c "GlowConsult\|GLOWCONSULT" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 3.

Run: `grep -c "10 years" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 0.

Run: `grep -c "Sogeti\|FHLBank\|Deloitte" /Users/dayoajayi/workspace/dayoajayi.github.io/index.html`
Expected: 2 (About paragraph + Earlier Career line).

- [ ] **Step 3: Push to deploy**

Run: `git status`
Expected: clean working tree, master ahead of origin/master by ~7 commits.

Run: `git log --oneline origin/master..HEAD`
Expected: 7 commits from Tasks 1–8 (publication boundaries, head metadata, header, About, GlowConsult entry, dates, condense, CLAUDE.md).

Run: `git push origin master`
Expected: push succeeds. GitHub Pages will auto-deploy within ~1–2 minutes.

- [ ] **Step 4: Post-deploy verification**

Wait ~2 minutes for GitHub Pages to rebuild, then:

Run: `curl -sI https://dayoajayi.github.io/ | head -1`
Expected: `HTTP/2 200`.

Run: `curl -sI https://dayoajayi.github.io/CLAUDE.md | head -1`
Expected: `HTTP/2 404`. (Confirms `_config.yml` exclude is working — pre-Phase-1 this returned 200.)

Run: `curl -sI https://dayoajayi.github.io/docs/superpowers/specs/2026-05-04-phase-1-content-refresh-design.md | head -1`
Expected: `HTTP/2 404`.

Run: `curl -s https://dayoajayi.github.io/ | grep -c "Sofware"`
Expected: 0.

Run: `curl -s https://dayoajayi.github.io/ | grep -c "GlowConsult"`
Expected: 3.

If any of the above don't match expectations, Phase 1 is not complete — investigate before declaring done.

- [ ] **Step 5: Mark Phase 1 complete**

Phase 1 acceptance criteria from the spec are fully satisfied. Notify Dayo. Discuss whether to:
- Send the live URL to `jobs@obcido.com` (the original near-term goal).
- Begin Phase 2 (visual upgrade) — see spec for scope.
- Defer Phase 2.

---

## Self-review checklist (executed during plan authoring)

- [x] **Spec coverage:** Every numbered section of the spec maps to a task. Section 0 → Task 1. Section 1 (head) → Task 2. Section 2 (header) → Task 3. Section 3 (social) → Task 3. Section 4 (About) → Task 4. Section 5a (GlowConsult entry) → Task 5. Section 5b/c (date single-source-of-truth) → Task 6. Section 5d (condensation) → Task 7. CLAUDE.md update is implicit in spec's note about Jekyll activation → Task 8.
- [x] **Placeholder scan:** No "TBD", no "implement later", no vague "add error handling." All edits show literal `old_string` / `new_string` content.
- [x] **Type consistency:** No types per se in HTML edits, but class/element names are consistent across tasks (e.g. `desc1` used the same way in Task 3 and earlier tasks). GlowConsult URL `https://glowconsult.ai` is identical in Tasks 3, 4, 5.

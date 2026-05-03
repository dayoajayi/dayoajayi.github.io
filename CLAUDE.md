# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A single-page static personal portfolio site for Dayo Ajayi, deployed at https://dayoajayi.github.io/. Because the repo is named `<username>.github.io`, GitHub Pages serves the root directly. A minimal `_config.yml` activates Jekyll only to use its `exclude:` directive — `index.html` and `assets/` pass through unchanged; there's no actual build step in the developer sense. Pushing to `master` is the deploy.

## Working on the site

There is no build, test, or lint tooling. Local development is just opening `index.html` in a browser:

```sh
open index.html         # macOS
```

To preview with a server (so relative paths and any future fetch() calls behave like production):

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```

To deploy: commit and push to `master`. GitHub Pages picks it up automatically.

`_config.yml` excludes `docs/`, `CLAUDE.md`, `README.md`, and `.superpowers/` from the deployed site, so design specs and editor scratch files stay version-controlled but are not web-accessible. If you add a new directory you don't want served (e.g. internal notes), add it to the `exclude:` list.

## Architecture

The project is intentionally minimal — virtually all content lives in one HTML file.

- **`index.html`** — the entire site. New résumé entries, projects, sections, links go here. The commit history shows that ~95% of changes are to this file.
- **`assets/css/styles.css`** — the stylesheet that `index.html` actually loads. This is the *compiled* output of the SCSS sources.
- **`assets/scss/`** — Sass sources (`styles.scss` → imports `_base.scss`, `_mixins.scss`, `_responsive.scss`). **Important:** there is no build script in this repo. If you edit SCSS, you must also recompile to `assets/css/styles.css` (e.g. `sass assets/scss/styles.scss assets/css/styles.css`) and commit both, otherwise the change won't reach the live site. For small one-off tweaks it's often simpler to edit `styles.css` directly and skip the SCSS round-trip — but then the SCSS becomes out of sync.
- **`assets/js/main.js`** — empty by design. The site has no custom JS; all interactivity comes from Bootstrap.
- **`assets/plugins/`** — vendored Bootstrap, jQuery 3.4.1, Popper, jquery-rss. Loaded via `<script>` tags at the bottom of `index.html`. There is no module bundler.
- **`assets/fontawesome/`** — local FontAwesome 5 (loaded via `assets/fontawesome/js/all.js`). Note that `index.html` *also* pulls FontAwesome 4.7.0 from a CDN; both are present so icon class names from either version may work.
- **`assets/images/`** — profile photo and any inline images.

### Theme provenance

The visual design is the third-party "Developer" portfolio template by Xiaoying Riley / 3rd Wave Media (see header comments in `assets/scss/styles.scss` and `assets/css/styles.css`). When adding new sections, mirror the existing class taxonomy — the theme expects:

- `section` / `aside section` wrappers with a `section-inner shadow-sm rounded` inner div
- `<h2 class="heading">` for section titles
- `<div class="item">` blocks for résumé-style entries with `title`, `place`, `year`, `university`, etc.

Inventing new class names or skipping the wrapper structure breaks spacing and the card look.

### Layout

Two-column responsive layout via Bootstrap 4 grid: `.primary col-lg-8` (left, main résumé content) + `.secondary col-lg-4` (right, sidebar with contact info, education, skills, hobbies). On mobile they stack.

## Conventions worth preserving

- **Google Analytics** tag (`UA-137365938-1`) is the first thing in `<head>`. Don't move or remove it.
- The `<meta name="google-site-verification">` tag is required for Search Console — leave it in place.
- Commit messages in this repo are typically terse (`Update index.html`). Match the style unless the change is structurally significant.

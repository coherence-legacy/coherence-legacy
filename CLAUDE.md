# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A hand-written static HTML site, "coherence legacy": the archive site for Finn Jorgensen's retired design practice (art, architecture, interior design, permaculture, community projects, healing arts). Copy is in English. There is no build system, package manager, templating, JavaScript, or tests. Every page is a standalone `.html` file in `01-pages/`, and all pages share `01-pages/legacy.css`.

To preview, open `01-pages/index.html` in a browser, or serve the folder with `python3 -m http.server -d 01-pages`.

## Layout

- `01-pages/*.html`: flat, with no subdirectories. Groups of pages share a filename prefix: `gov-*` (community governance docs), `mod-*` (massage modalities), `healing-*`, `nb-*` (natural building), `sangre-*`, `thrivalist-*`, `merc-*`. Project pages (ranches, villas, and so on) have no prefix.
- `01-pages/images/`: every image, referenced as `images/<file>` (roughly 360 MB).
- `01-pages/legacy.css`: the only stylesheet. Colors are custom properties on `:root`, and fonts come from Google Fonts through `@import`.
- `images.zip` and `01-pages(2).zip` at the repo root are source archives of the same content. Treat them as originals and don't edit them.

## Page structure (duplicated in every file)

Each page repeats the same shell inline: `header.mast` holds the banner (clouds image, the enso and gankyil marks, the `coherence legacy` wordmark) and `nav.nav`; then comes `main` and the shared `footer`. Because no include mechanism exists, **a change to the header, nav, or footer has to be applied to all 109 files**. Use a scripted find/replace and check afterward that it hit every file.

- **Nav highlighting:** the top-level section a page belongs to gets `class="is-here"` on its nav link. Subpages highlight their parent section; for example, every `mod-*` page highlights `art`. When adding a page, copy the shell from a sibling in the same section.
- **Alternate nav:** `fin.html`, `current.html`, and `projects.html` use a different nav (home / current projects / select projects / the coherence way / fin). Keep it when editing those three pages.
- **Content pattern:** `main` starts with a `p.kicker` (the section tagline) and then an `h1`. Most subpages end with a "back to …" link to their parent page.
- **Content classes in legacy.css:** `.shot` (figure wrapper for images; `.shot.compact` caps the width), `.gallery`, `.pair`, `.cards`/`.card` (link grids on section index pages), `.point`, `.video` (responsive iframe), `.cover-play`, `.note`, `.lead`. Reuse these instead of adding inline styles.
- Headings and the brand text are styled `text-transform: lowercase`, so the source copy is written in lowercase to match.

## Checking links

Internal links and image paths all resolve right now. After you add, rename, or remove pages or images, run this from `01-pages/` to keep it that way:

```sh
for l in $(grep -ho 'href="[a-z0-9-]*\.html"' *.html | sed 's/href="//;s/"//' | sort -u); do [ -f "$l" ] || echo BROKEN $l; done
for f in $(grep -ho 'src="images/[^"]*"' *.html | sed 's/src="//;s/"//' | sort -u); do [ -f "$f" ] || echo MISSING $f; done
```

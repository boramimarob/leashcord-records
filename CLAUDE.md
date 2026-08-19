# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Communication

Reply to the user in Korean (한국어) when working in this repository.

## Project overview

This repo is a single static marketing page for LEASHCORD RECORDS, a house-music DJ/sounds collective based in
Yangyang, Korea. The entire site is one self-contained file: `index.html`. There is no build system, package
manager, bundler, linter, or test suite — the file is meant to be opened directly in a browser or served as static
HTML as-is.

The `imgs/` directory holds standalone `.jpg` copies of every `<img>` photo that's embedded inline in `index.html`
(extracted from the base64 data URIs, one file per `<img>` tag, named by section — e.g. `about-01.jpg`,
`record-surfyy-02.jpg`, `reel-01-poster.jpg`). These files are **not referenced by `index.html`** — the page still
renders entirely from its inline base64 data (see below); `imgs/` exists purely as an extracted-asset reference
copy, so don't assume editing a file there changes the live page.

## Working with this file

- **Preview changes**: open `index.html` directly in a browser, or serve it locally, e.g. `python3 -m http.server`
  from the repo root and visit `http://localhost:8000`.
- There is nothing to install, build, lint, or test. Do not add a package.json/toolchain unless the user asks for
  one — the project is intentionally a single static file.
- The file is large (~3.3 MB) because every photo is embedded directly as a `data:image/jpeg;base64,...` URI, either
  in inline `<style>` rules (e.g. `.hero-bg`, `.contact-bg`) or as `<img src="data:...">` inside section markup
  (e.g. `.about-photos`, `.connect-photos`, `.rec-media`, `.collab-photos`). These base64 lines are extremely long
  (tens to hundreds of thousands of characters) — when reading/grepping the file, filter or truncate on line length
  rather than printing them raw, or you will blow through context.
- The four `.reel-slot` blocks in the RECORD section contain `<video>` elements whose `<source>` tags are
  intentionally commented out, with a poster image shown in the meantime. To wire up a real reel, drop an `.mp4`
  into a `videos/` folder (referenced by path but not present in the repo) and uncomment the corresponding
  `<source src="videos/reel-0N.mp4" type="video/mp4">` line.

## Structure of index.html

The file has three parts, in order:
1. **`<head>`**: meta tags, a Pretendard webfont `<link>` from jsDelivr, and one large inline `<style>` block. The
   stylesheet is organized (see its own header comment) as: CSS variables → layout → nav → hero → per-section
   styles → responsive media queries. Color/spacing tokens live in `:root` (`--ink`, `--sand`, `--coral`, `--amber`,
   `--teal`, `--maxw`, `--nav-h`, etc.) — reuse these instead of hardcoding new colors.
2. **`<body>`**: a fixed nav followed by one `<section>` per page area, always in this order and matching the nav
   anchors: `#top` (hero) → `#about` → `#connect` → `#music` → `#work` → `#record` (track record + reel slots) →
   `#numbers` (count-up stats) → `#collab` → `#contact` → footer. Decorative wavy `<svg class="cord">` dividers sit
   between several sections — they're purely visual and echo the "leash cord" branding motif.
3. **A single inline `<script>`** at the end of `<body>` (IIFE, vanilla JS, no dependencies) that wires up: sticky
   nav background + back-to-top link on scroll, active-nav-link highlighting via `IntersectionObserver`, scroll
   reveal animations (elements with class `.reveal`, observed and given `.in` once visible), a count-up animation
   for `[data-count]` elements in `#numbers`, and click-to-play/pause for `.reel-slot` videos.

Content is bilingual-leaning-Korean (Korean copy with some English labels/headings) — match the existing tone and
language mix when editing copy.

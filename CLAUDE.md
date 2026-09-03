# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A single-file static landing page for **Ramal Virtual Telecom** (cloud PABX/telephony for businesses). `index.html` is the entire production site — plain HTML + inline `<style>` + inline `<script>`, no build step, no dependencies, no framework.

- `index.html` — the actual site. This is the only file that gets deployed/edited for content or design changes.
- `Ramal Virtual LP.dc.html` + `support.js` — the **original** file this project was converted from (a Claude Design "canvas" file with a custom templating runtime: `{{ }}` bindings, `sc-if`/`sc-for`, `style-hover` attributes). It is legacy source material only, kept for reference. Do not edit it expecting it to affect the live site, and do not treat its templating syntax as valid HTML — `index.html` was hand-converted to plain static markup with real CSS classes and vanilla JS.
- `uploads/` — all images referenced by `index.html` (logos, product photos, client logos). Filenames are inconsistent (spaces, mixed case, some `.jpg.jpeg` double extensions) because they were carried over from the original canvas project — **check actual pixel dimensions before trusting a filename or an existing `width`/`height` attribute**; several images in this repo are secretly PNGs with transparency saved under a `.jpeg` extension, and at least two had `width`/`height` attributes that didn't match the real file, causing visible stretching until fixed.

## Running / previewing locally

There is no build command — just open `index.html`. For anything beyond a static glance (testing responsive behavior, verifying images load, running the FAQ/menu/form JS), serve the directory over HTTP rather than opening the file directly:

- This machine has **no working Python** (`python`/`py` resolve to the Windows Store stub, not a real interpreter) and no Node — `python -m http.server` and `npx serve` will not work here.
- Use a PowerShell `HttpListener` one-off script instead (see `.claude/launch.json` pattern used during development: a `powershell.exe -File serve.ps1` config on port 8642, reading files relative to the project root and serving them with correct `Content-Type`).
- Opening `index.html` via a `file://` path in the in-app Browser preview tool renders it in a sandboxed "snapshot" mode that inlines everything as a `data:` URL — relative `uploads/...` image paths silently fail to resolve in that mode. Always serve over `http://localhost` when checking that images actually load.

## Architecture of index.html

Single file, three parts in order:

1. **`<style>` in `<head>`** — CSS custom properties on `:root` for the palette (`--blue`, `--blue-dark`, `--cyan`, `--ink`, `--muted`, etc.), then component styles in source order (header, hero, strip, cards, plans, FAQ, contact form, footer, floating WhatsApp widget), then all responsive overrides grouped into three `@media` blocks at the bottom of the stylesheet:
   - `max-width:1024px` — tablet: single-column hero/sections, 2-col grids.
   - `max-width:860px` — where the header nav collapses into the hamburger menu.
   - `max-width:640px` — phone: single-column everything, reduced hero-card/panel sizing, tighter paddings.
   - A `max-width:400px` block for the smallest phones.

   When a grid needs to collapse to fewer columns responsively, use `minmax(0,1fr)` tracks (not bare `1fr`) — bare `1fr` tracks let a wide child (e.g. an uppercase nowrap button label) blow out the grid and cause horizontal page overflow on narrow viewports. This has been the recurring bug class in this file; check `document.documentElement.scrollWidth` vs `clientWidth` at 320/375/414px after any grid/breakpoint change.

2. **Body markup** — semantic sections in reading order: sticky `<header>` → `<main id="top">` (hero, trust strip, problem cards, solution, plans, "how it works", client logos, FAQ, contact form) → `<footer>` → the floating WhatsApp widget (`.wa-float`, appended after `</main>`, not inside `<header>`).

   The mobile nav panel (`#mobilePanel`) is a **sibling of `<header>`**, not a child of it — `<header>` has `backdrop-filter`, which creates a new containing block and breaks `position:fixed` for any fixed-position descendant. Keep any future fixed/full-screen overlay outside of `<header>` for the same reason.

3. **`<script>` at the end of `<body>`**, one IIFE containing independent, self-contained behaviors — mobile menu toggle, FAQ accordion (data-driven from a `FAQ` array, single-item-open), contact form validation (client-side only, no backend — shows a success message on valid submit, does not actually send anywhere), and the floating WhatsApp bubble (auto-shows once via `sessionStorage`, dismissible).

### Floating WhatsApp widget (`.wa-float`)

Two parts: a pulsing round FAB (`#waFab`, links to `https://wa.me/551130900900`) and a greeting bubble (`#waBubble`) that auto-shows ~2.5s after load unless already dismissed this session (`sessionStorage` key `rv-wa-bubble-dismissed`). The bubble's avatar image is `uploads/perfil-ig.png` — swap the `<img>` `src` there (and its `width`/`height` attrs) if the profile picture changes; the WhatsApp number to update, if it ever changes, appears in three places: the header button, the mobile nav panel, and this widget.

## Git remotes

- `origin` → `https://github.com/LuviCompany/Site-Ramal.git` — the default push/pull target.
- `personal` → the original repo (`pedrofgueira7/ramal-virtual`, URL contains an embedded PAT) — kept as a backup remote, not pushed to unless explicitly asked.

## Project history (this repo, chronological)

1. Converted the original Claude Design canvas (`Ramal Virtual LP.dc.html` + `support.js`, a templated runtime format) into a single static `index.html` — real CSS classes instead of inline `style-hover` attributes, vanilla JS instead of the `{{ }}`/`sc-if`/`sc-for` template DSL, a hamburger nav for mobile, and a client-side-validated contact form (no backend).
2. Fixed the hero section for mobile: text now comes before the image in source/visual order (was image-first); the "ligação em andamento" / "ramais conectados" floating cards got a subtle infinite float animation (`prefers-reduced-motion` respected) and stayed absolutely positioned over the hero image at every breakpoint instead of stacking flat on mobile.
3. Fixed real image-distortion bugs: two images (`homem mexendo no notebook...png` and `notebook - celular - telefone.png`) had `width`/`height` attributes and container `aspect-ratio` that didn't match the files' real pixel dimensions, stretching them. Always verify real dimensions (e.g. via PowerShell `System.Drawing.Image`) rather than trusting existing markup.
4. Fixed mobile-only layout bugs: the trust-strip's 5th item was left-aligned instead of centered (now spans the full grid row on the 2-col mobile layout); the featured plan card was being force-reordered to the top on mobile via `order:-1` (removed, so plan order now matches desktop: A, B, C, Custom); the custom-plan CTA button had **inline** `style="margin-top:auto"` on the `<a>` tag, which silently overrode the CSS media-query fix for its spacing until the inline style was removed.
5. Enlarged the header and footer logos, and discovered both logo image files (despite `.jpg.jpeg` extensions) are actually transparent PNGs with a lot of dead padding around the mark — cropped them to content bounds (`uploads/logo-header.png`, `uploads/logo-footer.png`) so the CSS `height` maps predictably to the visible logo size.
6. Added a floating WhatsApp widget (see architecture section above) — went through a couple of iterations on the bubble's avatar image before landing on `uploads/perfil-ig.png`.
7. Repo initialized, pushed to the personal GitHub, then a second remote for LuviCompany's org was added and promoted to `origin` (default push target) at the user's request.

## User preferences

- Communicates in Brazilian Portuguese; respond in kind.
- Wants the whole site as **one self-contained HTML file** — no build tooling, no framework, no split CSS/JS files.
- Cares a lot about responsive correctness specifically at 320/375/414px (phone), 768/834px (tablet), and 1024/1440px (desktop) — check `scrollWidth` vs `clientWidth` at these widths after layout changes, don't just eyeball one size.
- Prefers changes to be actually verified (in-browser, via a local server) rather than assumed correct from reading the CSS — several bugs in this project's history (inline `style` overrides, wrong image aspect ratios) were invisible from source alone and only showed up when actually rendered.
- When an asset (image, logo) is needed, expects it to be provided as a file dropped into `uploads/` with a filename they give — do not search the broader filesystem for "probably the right image," that has surfaced unrelated personal files before.
- Prefers a plain-language summary of what changed at the end of each turn, not a restatement of the whole file.

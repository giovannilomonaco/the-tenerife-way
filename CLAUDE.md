# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static landing page for **The Tenerife Way** — a private territorial experiences business based in Tenerife, Canary Islands. No build step, no dependencies, no package manager.

## Files

- `index.html` — main landing page (HTML + CSS + JS, single file)
- `admin.html` — password-protected admin panel for managing blog articles

## Running the site

Open `index.html` or `admin.html` directly in a browser. No server required.

## Architecture

All logic is self-contained in each HTML file (inline `<style>` and `<script>`).

**Data persistence:** Blog articles are stored in `localStorage` under the key `ttw_articles` as a JSON array. `index.html` reads this key at page load and falls back to a hardcoded `DEFAULT_ARTICLES` array if empty. `admin.html` writes to the same key, so changes made in the admin panel appear immediately on the landing page in the same browser.

**Admin auth:** Simple password check against the constant `PASSWORD = 'tenerife2026'` in `admin.html`. Session is kept alive via `sessionStorage` key `ttw_admin`.

**Article schema:**
```js
{ id, title, excerpt, tag, date, emoji, image, instagram }
```

## Animations (`index.html`)

- **Navbar entry:** starts `opacity:0; transform:translateY(-100%)`, gets class `nav-visible` via JS 100ms after `load`
- **Hero parallax:** `scroll` listener moves `.hero-bg` at 25% scroll speed and `.hero-silhouette` at 12% — only while `scrollY < innerHeight`
- **Scroll reveal:** CSS classes `.reveal`, `.reveal-left`, `.reveal-right` — elements start hidden, `IntersectionObserver` adds `.visible` on entry. Delay controlled by CSS custom property `--reveal-delay` (inline style)
- **Staggered cards:** `.exp-card`, `.step`, `.test-card`, `.blog-card` get `.reveal` + `--reveal-delay` assigned in JS (120ms steps) via `observeCards()` / the blog's own post-render loop
- **Counter:** `IntersectionObserver` on `.intro-badge` animates the `100%` text from 0 to 100 using `setInterval` at 30ms steps

## Design tokens (CSS variables in `index.html`)

```css
--sand: #F5ECD7
--ocean: #1A6B8A
--forest: #2D5A27
--dark: #1C1C1C
--light: #FDFAF5
--muted: #7A7060
```

Fonts: Playfair Display (headings), Inter (body) — loaded from Google Fonts.

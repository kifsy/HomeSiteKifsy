# kifsy · Project Guide for Claude Code

> **Read this entire file before making any change.** When you finish a change and push, append a new line to the Changelog at the bottom of this file.

## What is kifsy

kifsy is a digital agency targeting small businesses in Israel (restaurants, salons, lawyers, trainers, real estate, local stores). The deliverable is a premium single-page marketing site that converts owners into leads.

Brand voice: minimal, premium, smart, friendly, startup, modern, accessible.

## Live repository

GitHub: https://github.com/DoronZeltzer/kifsy.com

Remote: `origin` = `https://github.com/DoronZeltzer/kifsy.com.git`
Default branch: `main`

## Files

| File | Purpose |
|---|---|
| `index.html` | The marketing site. Inline CSS + JS. Hebrew default, English toggle. |
| `kifsy _standalone_.html` | Original logo identity file. Reference only. **Do not modify.** |
| `CLAUDE.md` | This file. Read first, update last. |

## Brand identity

**Colors:**
- Background: `#D6E3D1` (sage)
- Soft background: `#E1ECDC`
- Warm background: `#ECEFE6`
- Ink (text): `#2A3A2C`
- Accent (muted red): `#A64253`. Used for the period in `kifsy.` and small highlight moments only.

**Typography:**
- Hebrew: `Heebo` (Google Fonts), weights 300, 400, 500, 600, 700, 800.
- English: `Inter` for body, `Inter Tight` for display.
- The `kifsy` wordmark is ALWAYS Latin (Inter Tight) regardless of active language. It is wrapped in `.brand-mark` with `direction: ltr` so the red period stays correctly positioned in RTL.

**Logo:** 3 stylized leaves rendered as an SVG `<symbol id="leaves">` reused across navbar, hero background, footer, and final CTA.

## Hard rules — do not break

### 1. NEVER use em dashes (`—`, U+2014)
Not in copy. Not in code comments. Not in commit messages. Not in chat replies. Not in any language. Substitute with: comma, period, colon, parentheses, or rephrase. This rule is non-negotiable and was given directly by the user.

### 2. Hebrew is the default language
- Site loads with `<html lang="he" dir="rtl">`.
- Every translatable element carries `data-he` and `data-en` attributes. Elements containing inline HTML (like headlines with `<span class="accent-dot">.</span>`) use `data-he-html` and `data-en-html` instead.
- The `setLang()` JS function reads these attributes and swaps `textContent` or `innerHTML`. It also flips `dir`, updates the toggle UI, and persists choice to `localStorage` under the key `kifsy-lang`.
- When adding any new copy, ALWAYS provide both `data-he` and `data-en` (or the `-html` variants).
- The `kifsy.` brand wordmark and the SVG-only mockup pills (`Hummus Eliyahu`, `FitWithRoni`) stay Latin in both modes.

### 3. Do not modify the logo identity file
`kifsy _standalone_.html` is the canonical brand artifact. Leave it untouched.

### 4. Single-file site, no build step
Everything lives in `index.html`. CSS and JS are inline. The site must work by opening the file directly in a browser. No npm, no bundler, no framework runtime.

### 5. RTL details to preserve
- Forward arrows (→) flip via `transform: scaleX(-1)` in RTL.
- Browser mockup chrome stays `direction: ltr` (real browsers do not flip traffic lights).
- Hebrew uses the proper quote glyph `״` not `"` in testimonial styling.
- Reduced motion is respected via `@media (prefers-reduced-motion: reduce)`.

## Workflow for every change

1. **Read this file fully** before touching anything.
2. Make changes in `index.html` (or another file if needed).
3. Open `index.html` in a browser. Verify BOTH Hebrew and English. Check mobile width.
4. Stage, commit with a short present-tense message. No em dashes. No `--no-verify`.
5. Push to `origin main`:
   ```
   git add .
   git commit -m "summary of change"
   git push
   ```
6. **Update the Changelog below** with date and a one-line summary. If you forgot in the same commit, do a small follow-up commit and push that too.

## Setup notes (one-time, for new contributors)

If you see `Author identity unknown` when committing, set repo-local identity:
```
git config user.name "Your Name"
git config user.email "you@example.com"
```

If `git push` asks for credentials, sign in with Git Credential Manager (browser flow), set up an SSH key on GitHub, or use a Personal Access Token in the remote URL.

## Changelog

_Newest first. One line per push: `YYYY-MM-DD · summary`._

- **2026-05-17** · Initial commit. Hebrew-first marketing site with full bilingual toggle, 8 sections (hero, services, pricing, why kifsy, portfolio, testimonials, final CTA, footer), Heebo + Inter Tight typography, palette derived from the kifsy logo, RTL-aware arrows and quotes, no em dashes anywhere. Added `CLAUDE.md` as the canonical contributor guide.

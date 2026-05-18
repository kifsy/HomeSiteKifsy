# kifsy · Project Guide for Claude Code

> **Read this entire file before making any change.** When you finish a change and push, append a new line to the Changelog at the bottom of this file.

## What is kifsy

kifsy is a digital agency targeting small businesses in Israel (restaurants, salons, lawyers, trainers, real estate, local stores). The deliverable is a premium single-page marketing site that converts owners into leads.

Brand voice: minimal, premium, smart, friendly, startup, modern, accessible.

## Live repository

GitHub: https://github.com/kifsy/HomeSiteKifsy

Remote: `origin` = `https://github.com/kifsy/HomeSiteKifsy.git`
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

## Git workflow (team rules)

This repo is worked on by multiple collaborators. Follow these rules without exception:

- **Never auto-push.** Every push requires a manual review first.
- **Pull before starting work.** Always fetch/pull the latest `main` before making any changes.
- **Pull again before pushing.** Rebase or merge any new upstream changes before you push your commit.
- Keep commits clean and focused. One logical change per commit.
- Avoid editing files unrelated to the current task.

```
git pull origin main          # before you start
# ... make changes ...
git pull origin main          # before you push
git add <specific files>
git commit -m "short message"
git push origin main
```

## Workflow for every change

1. **Read this file fully** before touching anything.
2. Pull the latest `main` (`git pull origin main`).
3. Make changes in `index.html` (or another file if needed).
4. Open `index.html` in a browser. Verify BOTH Hebrew and English. Check mobile width.
5. Pull again (`git pull origin main`) to catch any concurrent changes.
6. Stage, commit with a short present-tense message. No em dashes. No `--no-verify`.
7. Push only after manual review:
   ```
   git push origin main
   ```
8. **Update the Changelog below** with date and a one-line summary. If you forgot in the same commit, do a small follow-up commit and push that too.

## Setup notes (one-time, for new contributors)

If you see `Author identity unknown` when committing, set repo-local identity:
```
git config user.name "Your Name"
git config user.email "you@example.com"
```

If `git push` asks for credentials, sign in with Git Credential Manager (browser flow), set up an SSH key on GitHub, or use a Personal Access Token in the remote URL.

## Accessibility (Israeli SI 5568 / WCAG 2.0 AA)

The site is built to comply with the Israeli accessibility regulations (תקנות שוויון זכויות לאנשים עם מוגבלות, תשע״ג-2013). When making changes, preserve these features:

- **Floating accessibility menu** (`.a11y-fab` + `.a11y-panel`) available on every page, with controls for: text size (3 levels), contrast (normal, high contrast, dark), grayscale, link highlighting, readable font (Arial), larger cursor, and animation pause. Choices persist to `localStorage` as `kifsy-a11y`.
- **Accessibility statement modal** (`#a11yStatement`) with full bilingual content (HE/EN) including coordinator contact info, list of implemented adjustments, known limitations, last-updated date. Linked from footer and from the panel.
- **Skip-to-content link** (first child of `<body>`), visible only on keyboard focus.
- **`<main id="main">` landmark** wrapping the page content.
- **Focus indicators**: `:focus-visible` outline in accent color, 3px, 3px offset.
- **Semantic structure**: proper heading order (h1, h2, h3), `aria-label` on icon-only buttons, `aria-pressed` on toggles, `aria-expanded` on the FAB, `role="dialog"` + `aria-modal="true"` on the statement, `hidden` attribute + transition for show/hide.
- **Esc closes** the statement first, then the panel.
- All accessibility menu strings carry `data-he` / `data-en`.

**Coordinator placeholder:** the statement names Doron Zeltzer with `hello@kifsy.co` and `+972-50-000-0000`. Replace with the real coordinator details before going live.

When adding new components: include keyboard support, `aria-label` for icon buttons, focusable interactive elements, and adequate contrast. Test with the `a11y-contrast-high` class applied.

## Changelog

_Newest first. One line per push: `YYYY-MM-DD · summary`._

- **2026-05-18** · Add privacy.html, terms.html, cookies.html (bilingual legal pages, sticky TOC, brand style); add cookie-banner-preview.html prototype with bar and manage-panel states.
- **2026-05-18** · Align about-page timeline icons to the start side of each language (right in HE, left in EN). Switched grid to '40px 1fr' with 'order: -1' on the icon, changed line position to logical 'inset-inline-start', and replaced hard-coded 'text-align: right' with 'text-align: start'.
- **2026-05-18** · Add about.html (8-section About Us page); integrate full shared nav with lang toggle and burger; add 'אודותינו' link to index.html and contact.html navs.
- **2026-05-17** · Replace BebKey and ramtours iframes with headless-Chrome screenshots; BebKey blocked iframes (X-Frame-Options: DENY), ramtours fired a JS alert when embedded. Screenshots saved to portfolio/previews/.
- **2026-05-17** · Wire real GA4 Measurement ID (G-SXNX3SDS48) replacing placeholder; fix testimonial role from "חומוס אליהו" to "חומוס יוסי".

- **2026-05-17** · Add 6 portfolio mockup pages (hummus-eliyahu, salon-maya, cohen-partners, fit-with-roni, levi-realty, hadar-boutique) under portfolio/; wire index.html cards to open them in a new tab.
- **2026-05-17** · Add contact.html with Formspree form, two team contacts (Doron/Ron), business hours; wire all index.html contact links to the new page.
- **2026-05-17** · Update hero subtitle copy: new bilingual text emphasizing AI, modern design, and fast launch.
- **2026-05-17** · Set up info@kifsy.com email forwarding via ImprovMX (MX records added to Namecheap). Updated all hello@kifsy.co references in index.html to info@kifsy.com.
- **2026-05-17** · Updated logo SVG symbol paths in index.html to match the actual kifsy-mark.png (three pointed leaves fanning from a shared base, center leaf tallest). Added kifsy-mark.png as favicon to index.html and all 6 service pages.
- **2026-05-17** · Split pricing into two plans: Starter (no SEO/Google Business) and Ads & Growth (from 350 NIS/month, includes SEO, Google Business, and Google Ads management).
- **2026-05-17** · Added 6 service detail pages under services/ (business-websites, landing-pages, website-management, mobile-optimization, whatsapp, ai-workflows). Wired all Learn more links in index.html to the new pages.
- **2026-05-17** · Move a11y FAB back to bottom corner (bottom-right EN, bottom-left HE). Panel opens upward above it.
- **2026-05-17** · Fix grayscale mode breaking position:fixed on FAB. Replaced filter:grayscale on body with backdrop-filter on body::before so position:fixed stays anchored to the viewport.
- **2026-05-17** · Move a11y FAB to mid-height side of viewport (top: 50%) so it is always visible regardless of scroll. Panel opens to the interior side of the button. Right side in English, left side in Hebrew.
- **2026-05-17** · Fix accessibility FAB position: now bottom-right in English (LTR) and bottom-left in Hebrew (RTL), using inset-inline-end. Updated CLAUDE.md with team git workflow rules (pull before work, manual review before push).
- **2026-05-17** · Migrated repo to https://github.com/kifsy/HomeSiteKifsy (org repo). Removed defunct personal repo remote. Updated CLAUDE.md to point to new canonical repo.
- **2026-05-17** · Fix accessibility panel disappearing during use. Root causes: document-level outside-click handler was firing for clicks that bubbled through the statement modal and panel internals; closed statement still trapped pointer events for 300ms during its fade; `.a11y-toggle` lacked `position: relative` so its dot pseudo was anchored to the panel. Solution: `stopPropagation` inside the panel and statement card; `pointer-events: none` on closed statement; FAB now shows accent color when expanded; better dark-mode contrast for menu controls.
- **2026-05-17** · Added Israeli-law accessibility layer: floating menu with text-size / contrast / dark mode / grayscale / link highlight / readable font / larger cursor / motion controls (persisted to localStorage), full bilingual accessibility statement modal with coordinator contact info, skip-to-content link, `<main>` landmark, focus indicators, footer link to the statement. Updated this CLAUDE.md with the a11y rules.
- **2026-05-17** · Initial commit. Hebrew-first marketing site with full bilingual toggle, 8 sections (hero, services, pricing, why kifsy, portfolio, testimonials, final CTA, footer), Heebo + Inter Tight typography, palette derived from the kifsy logo, RTL-aware arrows and quotes, no em dashes anywhere. Added `CLAUDE.md` as the canonical contributor guide.

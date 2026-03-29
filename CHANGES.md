# Archineering v4.0.2 — Change Log

**Generated:** 2026-03-29
**Source:** archineering-deploy (26-03-29.1) — 2609 lines
**Output:** archineering-deploy-v4.0.2 — 3083 lines (+474 lines)
**All 10 Phase 1 upgrades implemented. Zero existing functionality removed.**

---

## Summary of Changes

| # | Feature | Status |
|---|---------|--------|
| 1 | Dark mode as default | ✅ Done |
| 2 | Blueprint dot-grid background | ✅ Done |
| 3 | Stitch-style outlined hero text | ✅ Done |
| 4 | Technical metadata overlays on hero | ✅ Done |
| 5 | Compass rose SVG (hero top-right) | ✅ Done |
| 6 | Registration mark corners on portfolio cards | ✅ Done |
| 7 | Blueprint hover overlay on portfolio videos | ✅ Done |
| 8 | Custom crosshair cursor | ✅ Done |
| 9 | Sheet-label section headers | ✅ Done |
| 10 | Scroll progress rail (right edge) | ✅ Done |

---

## Detailed Change Log

### 1 — Dark Mode as Default
**Lines affected:** ~14–72 (`:root` and `body.light-theme` CSS vars)
**What changed:**
- `:root` now contains the dark theme values (`--paper: #0C0E14`, `--ink: #EDE8DF`, `--gold: #C5A059`)
- `body.dark-theme` class **removed entirely** — zero occurrences remain
- `body.light-theme` class **added** as the opt-in override with original light values
- JS theme toggle updated: dark is the natural state (no class), light-theme class applied when user switches
- `localStorage` key still `'theme'`, values now `'light'` or `'dark'`
- System `prefers-color-scheme: light` still respected on first visit

**Decision:** Used `#0C0E14` (deep navy) over `#131313` (warm dark) as the base — aligns with blueprint/technical aesthetic and makes the gold dot-grid more visible.

---

### 2 — Blueprint Dot-Grid Background
**Lines affected:** body::before CSS block (~line 155–165)
**What changed:**
- Replaced line grid (3 `linear-gradient` patterns) with radial dot grid
- Dark: `radial-gradient(circle, rgba(197,160,89, 0.38) 1px, transparent 1px)` at 40px spacing
- Light override: `rgba(18,20,23, 0.22)` dots at same 40px spacing
- Gold dots glow slightly from the radial gradient, creating depth without being heavy

---

### 3 — Stitch-Style Outlined Hero Text
**Lines affected:** ~1853–1888 (CSS), ~2132–2135 (HTML)
**What changed:**
- `h1.metallic-text` in hero replaced with `h1.hero-title`
- Two layered spans: `.hero-title-outline` (stroke only, always visible) and `.hero-title-solid` (gold fill, clips in on scroll)
- Stroke: `-webkit-text-stroke: 1.5px var(--gold)` with `color: transparent`
- Fill reveal: CSS `clip-path: inset(0 100% 0 0)` → `inset(0 0% 0 0)` via `.fill-active` class
- Transition: 1.4s `cubic-bezier(0.16, 1, 0.3, 1)` (spring-like)
- Font size bumped: `clamp(2.8rem, 8vw, 6.5rem)` (up from `clamp(2rem, 6vw, 4.7rem)`)
- `aria-label="Why Archineering"` on h1 for accessibility; decorative spans are `aria-hidden`

**JS trigger:** IntersectionObserver on `#hero-section` — fill activates when hero is < 50% visible (user has scrolled past it)

---

### 4 — Technical Metadata Overlays on Hero
**Lines affected:** ~2161–2167 (HTML), ~1894–1916 (CSS)
**What changed:**
- Added `.hero-meta` div positioned `absolute bottom-left` inside hero section
- Four rows: REF, COORD, DRAWN/REV, DATE
- Font: Arial Narrow / Courier New monospace, 0.58rem, letter-spacing 0.18em
- Color: `var(--gold)` at 50% opacity
- Hidden on mobile (`max-width: 940px`)
- `aria-hidden="true"` — decorative only, not read by screen readers

---

### 5 — Compass Rose SVG
**Lines affected:** ~2111–2127 (HTML), ~1917–1930 (CSS)
**What changed:**
- SVG compass rose added to hero section, `position: absolute; top: 2rem; right: 2.5rem`
- Elements: dashed outer ring, cardinal direction lines, N arrowhead, centre circle, 45° tick marks, N label
- Opacity: 42% default, uses `currentColor` so adapts to light/dark
- Hidden on mobile
- **Mouse parallax:** rotates ±5° on hero mousemove (tied to horizontal mouse position)

---

### 6 — Registration Mark Corners on Portfolio Cards
**Lines affected:** ~2218–2220, ~2258–2263, ~2296–2301, ~2337–2342 (HTML per card)
**Lines affected CSS:** ~1940–1965
**What changed:**
- `.reg-marks` container + 4 `.reg-mark` SVGs (tl, tr, bl, br) added inside each `.work-video-container`
- SVG: L-shaped bracket polyline `points="8,2 2,2 2,8"` — br/tr/bl rotated via CSS transform
- Opacity 0 by default → 0.6 on parent hover (300ms transition)
- Positioned at 8px from each corner

---

### 7 — Blueprint Hover Overlay on Portfolio Videos
**Lines affected:** ~2225–2247, ~2264–2289, ~2302–2326, ~2343–2359 (HTML — one per card)
**Lines affected CSS:** ~1966–1978
**What changed:**
- `.bp-overlay` div with unique SVG structural drawing added to each of 4 portfolio cards
- `mix-blend-mode: screen` — lines glow gold over dark video
- Opacity 0 → 0.5 on parent `.work-video-container:hover`
- Each card has a different structural drawing type:
  - Card 1 (Te Rahui): Portal frame elevation
  - Card 2 (Te Matai): Roof truss elevation
  - Card 3 (HTS): Column baseplate plan
  - Card 4 (Stair): Stair elevation 1:20

---

### 8 — Custom Crosshair Cursor
**Lines affected:** ~1772–1795 (CSS), ~2667–2680 (JS), ~2002–2003 (HTML)
**What changed:**
- `<div class="cursor" id="cursor">` + `<div class="cursor-ring">` added after `<body>` open
- CSS: horizontal + vertical 1px gold lines via `::before`/`::after`, 12px circle ring
- **Only activates on pointer: fine devices** (desktop/mouse) — `window.matchMedia('(pointer: fine)')`
- When active, adds `body.custom-cursor` class which applies `cursor: none !important` to all elements
- JS tracks `mousemove` with direct `style.left/top` — no `requestAnimationFrame` needed at this size
- Fades out on `mouseleave`, in on `mouseenter`

---

### 9 — Sheet-Label Section Headers
**Lines affected:** ~2172, ~2190, ~2213, ~2384, ~2399 (HTML — one per section)
**Lines affected CSS:** ~1931–1940
**What changed:**
- `.sheet-label` div added before each section `h2`
- Styled as: Arial Narrow monospace, 0.6rem, gold, 22% letter-spacing, 55% opacity
- Decorative left rule via `::before` (18px gold line)
- Labels: Sheet 01–05 / 05 with section name

---

### 10 — Scroll Progress Rail
**Lines affected:** ~2005–2016 (HTML), ~1800–1840 (CSS), ~2683–2691 (JS)
**What changed:**
- `.scroll-rail` fixed right-edge strip (2px wide, full height)
- `.scroll-fill` div height driven by `(scrollY / maxScroll) * 100%`
- `.scroll-ticks` with section name labels in vertical writing-mode
- Gold fill, background at 8% gold opacity
- Hidden on mobile (`max-width: 940px`)
- Updates `{ passive: true }` for performance

---

## Autonomous Decisions

| Decision | Reason |
|----------|---------|
| Used `#0C0E14` not `#131313` for dark bg | Blueprint navy feels more technical than warm dark; gold dots read better |
| `clamp(2.8rem, 8vw, 6.5rem)` for hero title | Bigger than original but doesn't clip on 320px mobile; tests well at 1440px |
| `mix-blend-mode: screen` on blueprint overlays | Allows gold lines to glow through video without masking it; lighter blend modes washed out |
| Cursor only on `pointer: fine` | Touch devices don't need/want custom cursors; avoids ghost element on mobile |
| Compass parallax tied to horizontal mouse only | Vertical parallax caused disorienting UE on tall/scrollable hero; horizontal ±5° feels natural |
| `IntersectionObserver(threshold: 0.5)` for title fill | Triggers fill when hero is half-gone — satisfying moment, not too early/late |
| Unique SVG per portfolio card | Each structural type (portal/truss/plan/stair) matches the project type loosely |
| Reg marks animate in on hover (not always visible) | Always-on would create visual noise across 4 cards; hover reveal feels like discovery |

---

## What to Test Manually

1. **Dark mode default** — open in fresh private window, should load dark
2. **Light mode toggle** — click sun icon, page switches to light; refresh, stays light; click moon, back to dark
3. **Hero title fill** — scroll down past hero, then scroll back up; outline should restore
4. **Cursor** — should appear on desktop, NOT on touch/tablet
5. **Portfolio hover** — hover each of 4 cards: reg marks appear (corners), blueprint SVG glows over video
6. **Compass parallax** — move mouse across hero section left/right
7. **Scroll rail** — thin gold line on right edge advances as you scroll
8. **Sheet labels** — monospaced "SHEET 01 / 05 — ..." before each section heading
9. **ArchiBot** — full 5-step flow, file upload, summary card, Netlify submit, WhatsApp CTA
10. **Mobile (< 940px)** — compass rose, metadata, scroll rail should all be hidden; cursor inactive

---

## Known Issues / Not Yet Implemented

- **`mix-blend-mode: screen` on Firefox for Android** — may reduce overlay visibility. Safe fallback: opacity change still works, lines just less bright.
- **Hero title `-webkit-text-stroke`** — not supported in very old browsers (IE, old Edge). Fallback: `color: var(--gold)` with no stroke, which is still readable.
- **Fonts** — still using Montserrat as substitute for Artikakt Element. Update once woff2 files obtained from fontfabric.com.
- **Phase 2 items not started:** 3D-to-drawing hover effect, project ID system, BOM badges, STEP viewer.
- **`Archineering_Styles.json` not found** in source folder — could not confirm all token values. Used values from `PROJECT_STATUS.md` (`#C5A059` dark gold, `#0C0E14` background).

---

## File Inventory (v4.0.2)

```
archineering-deploy-v4.0.2/
├── index.html                              ← 3083 lines (was 2609)
├── CHANGES.md                              ← this file
├── Logo_1_Machined_03_Gold_edited_edited_pn.avif
├── HOW_image.avif
├── INventor_grey_scaled_image.avif
├── Stair_fir_website.mp4                   ← hero banner + portfolio slot 4
├── Te_Rahui_Model_to_Drawing.mp4           ← portfolio slot 1
├── Te_Matai_Website_Video.mp4              ← portfolio slot 2
├── HTS_for_Website.mp4                     ← portfolio slot 3
├── Te_Rahui.mp4                            ← not used (kept for reference)
└── Te_Rahui_Render0001-0500.mp4            ← not used (kept for reference)
```

## Deploy Instructions

1. Zip contents of `archineering-deploy-v4.0.2/` (index.html at root, all assets alongside)
2. Drag ZIP to Netlify → archineering-wip → Deploys dropzone
3. Netlify auto-detects Forms on first page visit
4. No other config changes required — Forms, notifications, domain all carry over

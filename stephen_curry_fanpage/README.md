# Handoff: Stephen Curry Fan Page

## Overview
A single-page fan tribute site for NBA star Stephen Curry (Golden State Warriors). School assignment: built with Gen AI tools, constrained to **HTML, vanilla CSS, and vanilla JavaScript only** — no frameworks, no build step, no libraries.

## About the Design Files
Unlike a typical handoff, **`fanpage.html` is not a mockup to be reimplemented** — it is the actual, final vanilla HTML/CSS/JS deliverable, already meeting the assignment's tech constraint exactly. There is no target framework to port it to.

Your job here is implementation finishing, not translation:
1. Drop in the real photos (see **Assets** below) in place of the current placeholders / temp uploads.
2. Verify everything still renders correctly and adjust image crops/positions if needed.
3. Otherwise treat the HTML/CSS/JS as-is unless the user asks for changes.

## Fidelity
**High-fidelity — final implementation.** All colors, type, spacing, copy, and interactions are final, not placeholders (except the 6 gallery photos, which are intentionally-labeled placeholder tiles awaiting real images).

## Sections (single page, in order)
Persistent header + 10 stacked `<section>`s, navigated via sticky nav jump links (anchors: `#home #bio #stats #titles #timeline #rivalry #gallery #funfacts #quiz`).

1. **Header/Nav** — fixed, 76px tall, `rgba(9,12,20,.7)` + blur, gains solid bg/border after 10px scroll (`.scrolled`). Left: "CURRY30" wordmark. Right: 8 nav links, active link underlined gold based on scroll position. Below 980px width: links collapse into a dropdown panel toggled by a 3-line hamburger that animates into an ×.
2. **Hero (`#home`)** — 2-col grid (1.1fr text / 0.9fr image) on a radial navy→blue gradient with a giant faint "30" watermark. Eyebrow, "STEPHEN CURRY" headline (Archivo Black, clamp 3–6rem), tagline, two pill chips, portrait photo (3:4), bouncing "Scroll" cue. Stacks to 1 col under 840px.
3. **Bio (`#bio`)** — dark-navy panel (`--ink-2`). Photo (4:5) + 4 bio paragraphs + a 2-col definition list of quick facts (Born, Height, College, Drafted, Family, Married to).
4. **Stats (`#stats`)** — blue gradient band. 4-card grid, each with a count-up number (JS-animated on scroll into view), label, sub-line. Below: 3 pill badges (Finals MVP, Olympic Gold, All-Time 3PT Leader).
5. **Titles (`#titles`)** — 4 circular gold "medal" cards (year + opponent beaten) + a 2-col bullet list of awards + a unanimous-MVP footnote.
6. **Timeline (`#timeline`)** — dark-navy panel. Vertical line + gold dot per era (11 entries, 1988→2026), plus a full-width banner photo/caption at the end.
7. **Rivalry (`#rivalry`)** — 2-col: narrative copy (left) + a 4-row accordion (right), one row per Finals year (2015/16/17/18) vs. Cleveland, each expandable for a short recap. Win/loss pill per row.
8. **Gallery (`#gallery`)** — 3-col grid (2-col/1-col responsive) of 6 clickable tiles, each opening a lightbox modal with a caption. Currently striped placeholders — **this is where 6 of your dropped-in photos go.**
9. **Fun Facts (`#funfacts`)** — 3-col grid (responsive) of 6 numbered fact cards.
10. **Quiz (`#quiz`)** — 5-question multiple-choice trivia quiz, one question at a time, instant right/wrong highlighting, progress counter, final score + message, "Play Again" reset.
11. **Lightbox** (global overlay) + **Back-to-top button** (floating, appears after 500px scroll) + **Footer** (unofficial-fan-page disclaimer on a dark blue band).

## Interactions & Behavior
All vanilla JS, one `DOMContentLoaded` block, no dependencies:
- **Scroll**: toggles header `.scrolled` state and back-to-top visibility (`scrollY > 10` / `500`).
- **Mobile nav**: hamburger toggles `.open` on nav + button; any nav link click closes it.
- **Active nav link**: `IntersectionObserver` on all `<section id>` (rootMargin `-40% 0px -55% 0px`) toggles `.active` on the matching link.
- **Scroll reveal**: every `.reveal` element fades/slides up once via `IntersectionObserver` (threshold 0.15), then unobserves itself.
- **Stat counters**: `.count` spans animate 0 → `data-target` over 1200ms (cubic ease-out, `requestAnimationFrame`) the first time their card is 60% visible.
- **Rivalry accordion**: click a `.rivalry-trigger` toggles `.open` on its parent `.rivalry-item` (`max-height` transition); rows are independent, no auto-close of others.
- **Gallery lightbox**: click a `.gallery-tile` sets the caption and opens `#lightbox` (`.open` + `aria-hidden=false`), focuses the close button. Closes via backdrop click, × button, or Escape; restores focus to the tile that opened it.
- **Quiz**: `quizData` array (question/options/correct index) drives one rendered question at a time; selecting an option locks all options, colors correct green / picked-wrong red, enables Next; last question shows a score screen with a message tier (perfect / solid / room to grow) and a restart button.
- **Back to top**: smooth-scrolls to top on click.
- Respects `prefers-reduced-motion` (disables smooth scroll + collapses all transition/animation durations).

## State Management
No external state/data layer — everything is local closures/DOM state inside the single script block:
- `quizIndex`, `quizScore` — current question pointer and running score.
- `lastFocused` — the gallery tile that opened the lightbox, for focus restore.
- Open/closed state for mobile nav, lightbox, and each rivalry row lives entirely in DOM classList (no JS variables needed).

## Design Tokens
Defined as CSS custom properties on `:root`:

| Token | Value | Use |
|---|---|---|
| `--ink` | `#090c14` | primary background |
| `--ink-2` | `#111a30` | alternate panel background |
| `--blue` | `#1d428a` | Warriors blue |
| `--blue-deep` | `#081633` | dark blue (stats band, footer) |
| `--gold` | `#ffc72c` | Warriors gold — primary accent |
| `--gold-soft` | `#ffe08a` | lighter gold (hover/badge text) |
| `--white` | `#f5f6fa` | body text |
| `--muted` | `#96a1c2` | secondary text |
| `--line` | `rgba(245,246,250,.12)` | hairline borders |
| `--radius-lg` / `--radius-md` | `20px` / `14px` | card / tile corner radii |
| `--header-h` | `76px` | fixed header height (also section `scroll-margin-top` basis) |
| `--container` | `1160px` | max content width |

Typography: **Archivo Black** (Google Font, headings only, weight 400 — it's a single-weight display face) + system sans stack (`-apple-system, "Segoe UI", Roboto, Helvetica, Arial`) for body/UI text. No third font.

## Assets

### Already wired in (3 photos)
These `<img>` `src` paths currently point at temp upload paths — **repoint them at your dropped-in folder** (filenames will likely differ; matched by content below):

| Section | Current `src` in fanpage.html | Content | Crop notes |
|---|---|---|---|
| Hero portrait | `uploads/images-1787473333269-yklz.jpg` | Curry holding the Larry O'Brien Trophy, 2022 championship celebration | `object-fit: cover; object-position: 35% center;` — landscape source cropped to a 3:4 box, biased left toward Curry |
| Bio portrait | `uploads/images-1787473333274-9367.jpg` | Curry smiling/laughing courtside | 4:5 box, `object-position: center 20%` |
| Timeline banner | `uploads/images-1787473333279-hk1x.jpg` | 2026-27 season promo image (Curry + Draymond Green + teammate) | Full-width, natural aspect ratio, no crop |

### Still needed (6 gallery photos)
The Gallery section (`#gallery`) has 6 placeholder `<button class="gallery-tile placeholder reveal" data-caption="...">` tiles, in this order:

1. 2015 NBA Championship parade through Oakland
2. Signature step-back three-pointer
3. Pregame warm-up routine at Chase Center
4. Hoisting the 2022 Finals MVP trophy
5. With daughter Riley at a postgame press conference
6. Team USA, 2024 Paris Olympics

**To wire in a real photo**, replace a tile's inner content and drop the `placeholder` class, e.g.:
```html
<!-- before -->
<button class="gallery-tile placeholder reveal" data-caption="2015 NBA Championship parade through Oakland">PHOTO<small>2015 championship parade</small></button>

<!-- after -->
<button class="gallery-tile reveal" data-caption="2015 NBA Championship parade through Oakland">
  <img src="gallery/2015-parade.jpg" alt="2015 NBA Championship parade through Oakland" loading="lazy">
</button>
```
The CSS rule `.gallery-tile img { width:100%; height:100%; object-fit:cover; border-radius:inherit; }` is already in place — no style changes needed. `data-caption` must stay; it feeds the lightbox.

## Files
- `fanpage.html` — the entire site (HTML + inline `<style>` + inline `<script>`, single file, no external JS/CSS dependencies besides the Google Fonts `<link>` for Archivo Black).

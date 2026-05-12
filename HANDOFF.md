# Art Lounge — Project Handoff Document
**Author:** Adiwang Paul Wilbur  
**Year:** 2026  
**Project:** Art Lounge — Personal Visual Artist Portfolio Website

---

## Table of Contents
1. [Layout Selection](#1-layout-selection)
2. [Tokenization](#2-tokenization)
3. [Originality Statement](#3-originality-statement)

---

## 1. Layout Selection

### Overview
Art Lounge is a multi-page website consisting of four core pages: **Home (index.html)**, **Gallery (gallery.html)**, **Tutorials (tutorials.html)**, and **Contact (contact.html)**. Each page uses a distinct layout strategy chosen to best serve its content and purpose.

---

### 1.1 Global Layout — Sticky Header + Footer Shell

All pages share a consistent shell:

- **Header** is `position: sticky; top: 0` so navigation remains accessible as the user scrolls. It uses `display: flex` with `align-items: center` to horizontally arrange the brand, search bar, and nav links in a single row.
- **Footer** is a simple full-width bar pinned to the bottom of the content flow, using `text-align: center` and a subtle top border separator.
- The `body` uses `display: flex; flex-direction: column; min-height: 100vh` so the footer always sits at the bottom even on short pages.

**Why this layout?** Consistency across pages reduces cognitive load for the visitor. The sticky header ensures navigation is never out of reach regardless of scroll position.

---

### 1.2 Home Page (index.html) — Full-Viewport Hero

The homepage uses a **full-viewport cinematic hero layout**:

- The hero section is `width: 100%; height: 100vh` — it occupies the entire screen on load with no scroll needed to see the main message.
- A **layered stacking approach** is used: the background image sits in an absolutely positioned `.hero-bg` div, a CSS gradient overlay darkens it for text contrast, and the content (`.hero-content`) sits above with `position: relative; z-index: 2`.
- Text content is left-aligned and arranged in a vertical column (`flex-direction: column`) with staggered entrance animations (`animation: revealUp`) applied to each child element individually.
- **Stats strip** is `position: absolute; bottom: 48px; left: 80px` — anchored to the lower-left of the hero for a magazine-style editorial feel.
- **Scroll indicator** is `position: absolute; bottom: 40px; right: 48px` — anchored to the lower-right.

**Why this layout?** A full-bleed hero with layered depth immediately communicates the artistic identity of the site. It mirrors the compositional language of professional portfolio and editorial sites.

---

### 1.3 Gallery Page (gallery.html) — CSS Masonry (Pinterest-Style)

The gallery uses a **CSS multi-column masonry layout**:

```css
.gallery-grid {
  columns: 236px;
  column-gap: 16px;
}
.pin-card {
  break-inside: avoid;
  margin-bottom: 16px;
}
```

- `columns: 236px` tells the browser to create as many columns as fit at that minimum width — automatically responsive without media query intervention.
- `break-inside: avoid` on each card prevents images from being split across columns.
- Cards have **variable natural heights** since images have `height: auto` — this creates the staggered Pinterest-style appearance organically.
- The **modal** uses `display: flex` with a left image panel (`flex: 1.1`) and a right info panel (`width: 320px; flex-shrink: 0`). On mobile, it switches to `flex-direction: column`.

**Why this layout?** CSS columns masonry is the most reliable cross-browser approach for Pinterest-style grids without JavaScript. It naturally adapts column count to screen width, making it inherently responsive.

---

### 1.4 Tutorials Page (tutorials.html) — Fixed-Column Masonry

The tutorials grid also uses CSS columns masonry but with a **fixed column count** rather than a minimum width:

```css
.tutorials-grid {
  column-count: 4;
  column-gap: 14px;
}
```

Cards have explicit heights (`tall` = 320px image, standard = 200px image) to create a deliberate rhythm rather than pure organic flow. This suits tutorials where content hierarchy (tall = featured, short = supplementary) needs to be intentional.

**Breakpoints:**

| Screen Width | Column Count |
|---|---|
| > 1100px | 4 columns |
| 768px – 1100px | 3 columns |
| 480px – 768px | 2 columns |
| < 480px | 1 column |

**Why this layout?** A controlled column count gives the designer intentional control over which cards appear prominent (tall) versus supportive (short), reinforcing the editorial hierarchy.

---

### 1.5 Responsive Strategy

All pages follow a **mobile-first breakpoint structure**:

- **≤ 768px** — hamburger nav replaces horizontal nav links; layouts stack vertically; modal panels restack.
- **≤ 480px** — padding reduces, column counts drop to 1, font sizes scale down via `clamp()`.
- The header nav is hidden on mobile via `display: none` and revealed only when the `.nav-open` class is toggled by the hamburger button.

---

## 2. Tokenization

### Overview
Tokenization in this project refers to the design token system — the set of reusable named values (colors, spacing, typography, radii) that define the visual language consistently across all pages.

---

### 2.1 Color Tokens

| Token Name | Value | Usage |
|---|---|---|
| `--bg` | `#f0ebe3` | Gallery page background (warm off-white) |
| `--surface` | `#ffffff` | Card backgrounds, modal backgrounds |
| `--text` | `#1a1a1a` | Primary text on light backgrounds |
| `--muted` | `#767676` | Secondary text, placeholders, tags |
| `--accent` | `#e60023` | Pinterest-red Save buttons, like button active state |
| `--accent-h` | `#ad081b` | Hover state of accent red |
| `--border` | `#e0d9d0` | Card borders, dividers on light pages |
| Dark bg | `#0f0f0f` | Tutorials page background |
| Dark surface | `#1a1a1a` | Tutorial card backgrounds |
| Gold | `#c9a96e` | Homepage and tutorials accent — eyebrow labels, play button, CTA button |
| Gold hover | `#e0bf82` | Hover state of gold accent |

**Two distinct palettes are used intentionally:**
- **Light palette** (gallery, contact) — warm off-white `#f0ebe3` background for a clean gallery feel.
- **Dark palette** (home, tutorials) — near-black `#0f0f0f` / `#0a0a0a` background for cinematic, editorial drama.

---

### 2.2 Typography Tokens

| Token | Font | Weights | Usage |
|---|---|---|---|
| Display / Headings | `Cormorant Garamond` | 300, 400 italic, 600 | Hero titles, modal titles, page headings |
| UI / Body | `Outfit` | 300, 400, 500, 600 | Nav links, body text, buttons, tags, labels |
| System fallback | `Inter, system-ui, sans-serif` | — | General body fallback in style.css |

**Type scale (approximate):**

| Role | Size |
|---|---|
| Hero title | `clamp(2.8rem, 6vw, 5rem)` — fluid, scales with viewport |
| Section title | `1.4rem – 1.5rem` |
| Card title | `0.9rem` |
| Labels / Tags | `0.68rem – 0.78rem` |
| Body / Description | `0.9rem – 1rem` |
| Footer / Caption | `0.78rem – 0.82rem` |

`clamp()` is used on hero titles so they scale smoothly across all screen sizes without hard breakpoints.

---

### 2.3 Spacing Tokens

Spacing follows an **8px base grid**:

| Scale | Value | Used for |
|---|---|---|
| XS | `4px – 6px` | Tag gaps, small inner padding |
| S | `8px – 12px` | Button padding, icon gaps |
| M | `14px – 16px` | Card gaps, column gaps |
| L | `20px – 24px` | Modal inner padding |
| XL | `36px – 40px` | Section side padding |
| XXL | `48px – 80px` | Hero vertical padding |

---

### 2.4 Shape / Radius Tokens

| Token | Value | Used for |
|---|---|---|
| `--radius` | `16px` | Pin cards, tutorial cards |
| Pill | `999px` | Buttons, search bar, nav links, tags |
| Modal | `20px – 24px` | Modal containers |
| Small | `10px` | Related grid images, form inputs |

---

### 2.5 Shadow Tokens

| Token | Value | Used for |
|---|---|---|
| `--shadow` | `0 4px 20px rgba(0,0,0,.10)` | Card resting state |
| `--shadow-h` | `0 8px 30px rgba(0,0,0,.18)` | Card hover state |
| Modal shadow | `0 24px 80px rgba(0,0,0,.35)` | Modal container |
| Tutorial card hover | `0 16px 48px rgba(0,0,0,.5)` | Tutorial card hover on dark bg |

---

### 2.6 Animation Tokens

| Name | Definition | Used for |
|---|---|---|
| `revealUp` | `translateY(16–20px) → 0, opacity 0 → 1` | All entrance animations on load |
| `fadeIn` | `opacity 0 → 1` | Modal backdrop, lightbox |
| `slideUp` | `translateY(20px) → 0 + fadeIn` | Modal container entrance |
| `scrollDrop` | Animated gold line drop | Homepage scroll indicator |
| Stagger delays | `0.05s` increments per card | Tutorial card staggered entrance |

---

## 3. Originality Statement

### Statement of Original Work

This website — **Art Lounge** — is an original personal portfolio created by **Adiwang Paul Wilbur** in 2026. All design decisions, layout structures, CSS architecture, and JavaScript logic were authored specifically for this project.

---

### 3.1 Design Originality

The visual design system of Art Lounge was developed independently and is not derived from any template, theme, or pre-built UI kit. Specific original design decisions include:

- The **dual-palette system** (warm light gallery palette + dark cinematic editorial palette) was conceived to give different pages distinct emotional registers while remaining part of the same cohesive brand.
- The **gold accent `#c9a96e`** was chosen deliberately as an alternative to generic blue or purple brand colors, reflecting the warmth and craft of hand-drawn artwork.
- The **Cormorant Garamond + Outfit** font pairing was selected to contrast a classical, elegant serif display face with a modern geometric sans-serif — communicating that the site bridges traditional art and a contemporary digital space.
- The **full-viewport hero with layered depth** (background image → gradient overlay → positioned text → floating stats) is an original compositional choice modeled after editorial photography layout principles, not a copied component.
- The **staggered entrance animation sequence** (eyebrow → title → tagline → CTA → stats, each 120–180ms apart) was crafted to guide the viewer's eye through the hierarchy in order of importance.

---

### 3.2 Code Originality

All HTML, CSS, and JavaScript in this project was written from scratch:

- No CSS frameworks (Bootstrap, Tailwind, Bulma, etc.) were used.
- No JavaScript libraries or plugins (jQuery, GSAP, Swiper, etc.) were used.
- The Pinterest-style masonry grid is implemented using native **CSS `columns`** — not a third-party masonry library.
- The modal system, search functionality, filter system, and hamburger nav are all implemented in **vanilla JavaScript**.
- The crossfade hero background slideshow uses a CSS class toggle approach with `transition: opacity` — no external carousel or slider library.

---

### 3.3 Content & Assets

- All artwork images displayed in the gallery (`draw1.jpg` through `draw15.jpg`) are original artworks created by the author.
- All tutorial video thumbnails (`Thumbnail1.jpg` through `Thumbnail7.jpg`) are original image assets produced for this project.
- The logo (`Logo-Art.png`), background images, and decorative GIFs are original assets created or curated by the author.
- YouTube video IDs embedded in the tutorials page link to externally hosted content and are attributed to their respective creators via the "Watch on YouTube" links in the modal.

---

### 3.4 References & Inspirations

The following were used as **visual inspiration only** — no code or assets were copied:

| Inspiration Source | Aspect Referenced |
|---|---|
| Pinterest | Masonry card grid layout pattern, modal detail view structure |
| Editorial photography sites | Full-viewport hero composition, stat strip placement |
| Behance / Dribbble portfolios | Dark background aesthetic for creative portfolio pages |
| Google Fonts | Typefaces `Cormorant Garamond` and `Outfit` (free open-source fonts) |

---

*This handoff document was prepared to accompany the Art Lounge source files and certifies that the work described herein represents the original creative and technical output of Adiwang Paul Wilbur.*

---

&copy; 2026 Adiwang Paul Wilbur — Art Lounge

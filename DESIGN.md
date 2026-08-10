# Vicciz Marketing Design Language

Shared visual system for the public Vicciz surfaces: `pitch.vicciz.com` and
`demo.vicciz.com`. Both pages ship as single, dependency-free HTML files, so the
system is duplicated by copy rather than imported — this document is the source
of truth that keeps the copies honest.

Derived from the Vicciz app's dual-theme spec
(`Kiipling/docs/design/dual-theme-tokens.md`) and `index.css`. Marketing pages
use the **Twilight Voyage (dark)** theme only. Light mode is an app concern; the
pitch and demo are deliberately single-theme so the brand reads the same in
every deck screenshot and link preview.

## Palette philosophy

> Vicciz is "a calm, premium travel architect, not a noisy ad-driven OTA."

Navy carries authority, gold carries action, red carries conflict. Nothing else
gets a hue. Marketing pages add no new brand colors — every accent on these
pages is gold at some alpha.

## 1. Tokens

Declared identically at `:root` on both pages.

| Token | Value | Role |
|---|---|---|
| `--navy` | `rgb(22 51 90)` | Brand authority; raised panels, borders |
| `--gold` | `rgb(224 137 44)` | Action, eyebrows, emphasis, focus |
| `--red` | `rgb(239 68 68)` | Conflict / breakage only |
| `--green` | `rgb(52 211 153)` | Resolved / shipped state only |
| `--bg` | `rgb(11 18 32)` | Page background |
| `--surface` | `rgb(19 32 58)` | Card / panel background |
| `--text` | `rgb(255 255 255)` | Headings, body |
| `--text-secondary` | `rgb(255 255 255 / 0.72)` | Lede, paragraph body |
| `--text-muted` | `rgb(255 255 255 / 0.5)` | Meta, captions, footer |
| `--hairline` | `rgb(255 255 255 / 0.12)` | Borders, dividers |

Alpha rule: white text alpha never drops below `0.5` on `--bg` or `--surface`
(0.5 white on `#0B1220` ≈ 4.7:1, clears WCAG AA for normal text). Gold on `--bg`
≈ 6.5:1 and on `--surface` ≈ 5.5:1 — safe for text and for the 3:1 non-text UI
threshold.

### Page background

Both pages carry the same two-stop radial wash, which echoes the warmth in the
logo mark. It is the strongest single cue that the two pages are one brand:

```css
background-image:
  radial-gradient(60rem 40rem at 78% 12%, rgb(224 137 44 / 0.13), transparent 62%),
  radial-gradient(52rem 42rem at 12% 88%, rgb(22 51 90 / 0.55), transparent 65%);
```

Never `background-attachment: fixed` — it repaints badly on mobile and clips the
gradient to the viewport instead of the document.

## 2. Typography

| Role | Family | Weight | Size |
|---|---|---|---|
| `h1` | Lora, Georgia, serif | 600 | `clamp(2rem, 5.2vw, 3.1rem)`, line-height 1.14, tracking -0.015em |
| `h2` | Lora, Georgia, serif | 600 | `clamp(1.5rem, 3vw, 2rem)`, line-height 1.2 |
| `h3` | Lato | 700 | `1.05rem` |
| Body | Lato | 400 | `1rem`/1.6 |
| Lede | Lato | 400 | `clamp(1.02rem, 1.6vw, 1.14rem)`, `--text-secondary`, max 34rem |
| Eyebrow | Lato | 700 | `0.72rem`, uppercase, tracking 0.16em, gold |
| Meta | Lato | 400 | `0.8rem`, `--text-muted` |

Display serif is reserved for `h1` and `h2`. `h3` and below are Lato — the same
rule the app enforces. `h1 em` is the emphasis idiom: `font-style: normal` plus
gold. It is the only place a headline changes color.

Two font families, four weights, loaded in one request:
`Lato:wght@300;400;700;900` + `Lora:wght@500;600;700`, `display=swap`.

## 3. Layout

- Page max-width `1140px`, centred.
- Page padding `clamp(1.75rem, 4vw, 3.5rem) clamp(1.25rem, 4vw, 2.5rem)`.
- Section rhythm `clamp(3rem, 7vw, 5rem)` between top-level sections.
- Prose max-width `34rem` (lede) / `46rem` (body) — never full-bleed text.
- One breakpoint only: `900px`. Below it, everything is one column in DOM order.
- `text-wrap: balance` on headings, `text-wrap: pretty` on ledes.

## 4. Components

### Masthead
Mark (2.25rem, `border-radius: 0.6rem`) + Lora wordmark (1.2rem/600) + a
right-aligned gold pill. The pill is a `--tag`: 0.7rem, 700, uppercase, tracking
0.14em, gold text, `1px solid rgb(224 137 44 / 0.4)`, `border-radius: 999px`,
padding `0.3rem 0.75rem`. It names the surface — "Product demo" on demo,
"Company pitch" on pitch.

### Panel
`background: var(--surface)`, `1px solid var(--hairline)`, `border-radius: 1rem`,
padding `clamp(1.25rem, 2.5vw, 1.75rem)`. The single container primitive: cards,
stat tiles, FAQ items and the demo's chapter list are all panels.

### Stat tile
A panel containing a Lora figure (`clamp(1.75rem, 4vw, 2.4rem)`, gold) over a
`--text-muted` 0.8rem label. Tiles sit in
`grid-template-columns: repeat(auto-fit, minmax(11rem, 1fr))`. Figures are
literal and sourced — no rounded-up marketing numbers.

### Numbered step
Gold 999px-radius index chip (1.6rem square, `rgb(224 137 44 / 0.12)` fill,
gold 700 text) + Lato 700 title + `--text-secondary` body. Used for the pipeline
on pitch and the chapter list on demo.

### Link / CTA
Primary CTA: gold background, `--bg` text, 700, `border-radius: 0.6rem`, padding
`0.7rem 1.15rem`. Secondary: transparent with `1px solid var(--hairline)` and
`--text` label. Inline links: gold, underline with `text-underline-offset: 0.2em`.

### Focus
Global `:focus-visible { outline: 3px solid rgb(224 137 44 / 0.5); outline-offset: 2px }`
— the app's `--focus-ring` translated to marketing markup. Never removed.

## 5. Motion

Reveal-on-scroll only, 0.5s `cubic-bezier(0.4, 0, 0.2, 1)`, opacity + 12px rise.
Hover transitions 0.2s. Everything is wrapped in
`@media (prefers-reduced-motion: reduce)` guards that disable transform and
animation. No autoplay, no parallax, no carousels.

## 6. Do / Don't

| ✅ Do | ❌ Don't |
|---|---|
| Use gold for exactly one action per view | Introduce indigo, pink, or teal accents |
| Keep Lora for h1/h2, Lato everywhere else | Set body copy in a serif |
| Cite a source next to every number | Publish a stat you cannot attribute |
| Ship one self-contained HTML file | Add a CDN framework or build step |
| State disruption in red, resolution in green | Use red or green decoratively |

## 7. Divergence log

Where the two pages legitimately differ:

- **demo** is a single viewport-height experience built around a video device
  frame; **pitch** is a long scrolling document with sections and an FAQ.
- **pitch** carries a language switcher (EN/HE) and RTL support; **demo** is
  English-only and `noindex` until launch.
- **pitch** is indexed and carries full JSON-LD; **demo** carries only OG tags.

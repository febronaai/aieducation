---
tags:
  - design-system
  - reference
created: 2026-06-10
---

# Design System — Retro Terminal

> A high-legibility dark terminal aesthetic. Retro feel comes from structure and typography, never from visual effects. Drop this note into any AI prompt to reproduce the look exactly.

---

## Prompt snippet

Paste this when starting a new project:

```
Use my retro-terminal design system:
- Fonts: Press Start 2P (headings/labels only, 7–9px, sparingly) + IBM Plex Mono (all body, 14px, lh 1.7)
- Palette: bg #0e0e16, surface #181828/#1e1e32, accent #6c5ce7 (light #9d94f0, dim #3d3680), text #eceaf8, muted #8884b0, faint #3e3c60, borders #2a2a44/#3a3a5c
- Rules: zero border-radius everywhere, 1–2px solid borders, no gradients, no shadows, no glow, no scanlines
- Icons: monochrome SVG on 28×28 grid, stroke-width 1.5, currentColor only — no emoji
- Accent used only for: active states, open/selected elements, left-border callout stripes
- Retro feel from: monospace type, sharp corners, all-caps labels, // prefix motifs, [ button ] bracket style
```

---

## Colour palette

| Role | Hex | Usage |
|---|---|---|
| Background | `#0e0e16` | Page / outermost bg |
| Nav / header | `#13131f` | Sticky header surface |
| Surface | `#181828` | Card default bg |
| Surface raised | `#1e1e32` | Card open/hover bg |
| Border default | `#2a2a44` | All borders at rest |
| Border strong | `#3a3a5c` | Hover borders, button borders |
| Accent | `#6c5ce7` | Primary accent — use sparingly |
| Accent light | `#9d94f0` | Text on accent bg, open card titles |
| Accent dim | `#3d3680` | Accent borders, bg fills |
| Text primary | `#eceaf8` | All body text |
| Text muted | `#8884b0` | Secondary text, subtitles, icons at rest |
| Text faint | `#3e3c60` | Dividers, inactive labels, chevrons |
| Green confirm | `#4ade80` | Copy success / confirmation only |

> [!note] Accent discipline
> `#6c5ce7` appears in exactly three places: active filter buttons, open card borders, and the left stripe on callout blocks. Nowhere else. This is what makes it feel like an accent rather than a theme colour.

---

## Typography

### Typefaces

| Role | Family | Size | Weight | Case |
|---|---|---|---|---|
| Display / wordmark | Press Start 2P | 9–11px | 400 | ALL CAPS |
| Card titles / labels | Press Start 2P | 6–8px | 400 | ALL CAPS |
| All body text | IBM Plex Mono | 14px | 400 | Sentence |
| Strong body | IBM Plex Mono | 14px | 500–600 | Sentence |
| Small labels / tags | IBM Plex Mono | 10–11px | 500 | ALL CAPS |

### Rules

- **IBM Plex Mono is the default.** Press Start 2P is a seasoning — used only for titles and labels where its pixel character earns its place. Overusing it destroys legibility.
- Line height: `1.7` for body, `1.6` for list items, `1.8` for pixel-font labels (to compensate for its tight descenders)
- Letter spacing: `0.08–0.14em` on all-caps labels, `0.04em` on subtitles, `0` on body
- No serif. No sans-serif. Monospace only.

### Google Fonts import

```html
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Press+Start+2P&display=swap" rel="stylesheet">
```

---

## Layout & structure

### Borders & corners

```css
border: 1px solid #2a2a44;   /* default */
border: 1px solid #3a3a5c;   /* hover */
border: 1px solid #6c5ce7;   /* active / open */
border-radius: 0;             /* always — no rounding anywhere */
```

### Cards

```css
.card {
  background: #181828;
  border: 1px solid #2a2a44;
  /* no border-radius, no box-shadow */
}
.card:hover { border-color: #3a3a5c; background: #1e1e32; }
.card.open  { border-color: #6c5ce7; background: #1e1e32; }
```

### Callout / insight block

```css
.callout {
  border-left: 3px solid #6c5ce7;
  background: rgba(108, 92, 231, 0.07);
  padding: 10px 14px;
  /* no border-radius */
}
```

### Labels & badges

```css
.label {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.1em;
  color: #9d94f0;
  background: rgba(108, 92, 231, 0.12);
  border: 1px solid #3d3680;
  padding: 2px 8px;
  text-transform: uppercase;
}
```

### Buttons

```css
.btn {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.08em;
  padding: 7px 16px;
  background: transparent;
  color: #8884b0;
  border: 1px solid #3a3a5c;
  cursor: pointer;
}
/* Bracket motif via pseudo-elements */
.btn::before { content: '[ '; }
.btn::after  { content: ' ]'; }
.btn:hover { border-color: #6c5ce7; color: #9d94f0; background: rgba(108,92,231,0.08); }
```

### Filter / nav buttons

```css
.filter-btn        { color: #8884b0; border-color: #2a2a44; }
.filter-btn:hover  { border-color: #3d3680; color: #eceaf8; }
.filter-btn.active { border-color: #6c5ce7; color: #9d94f0; background: rgba(108,92,231,0.12); }
/* Active state prefix */
.filter-btn.active::before { content: '▶ '; color: #9d94f0; }
```

---

## Icons

- **Format:** Inline SVG, 28×28 viewBox
- **Stroke:** `stroke-width: 1.5`, `stroke-linecap: square` (not round — keeps the angular feel)
- **Colour:** `currentColor` only — inherits from parent, never hardcoded
- **Fill:** none on paths/shapes; `fill="currentColor"` only for small solid dots or arrowheads
- **No emoji.** Emoji render inconsistently across OS and break the mono palette.
- **Pixel-art logo:** build from `<rect>` elements on a 4px grid using the accent ramp (`#6c5ce7`, `#9d94f0`, `#3d3680`)

### Wordmark logo — Brain 2.0

The canonical wordmark is a pixel-art brain in **side profile**, facing left, on a 32×32 grid using 2×2px pixels. Uses warm flesh tones — no accent purple. The brain reads as organic and anatomical, not abstract.

**Palette:**

| Role | Hex |
|---|---|
| Highlight (top surfaces) | `#fad0c0` |
| Midtone | `#e8a898` |
| Base flesh | `#c97a6e` |
| Shadow | `#a85a50` |
| Deep shadow / edge | `#8a4840` |
| Stem / base | `#6e3830` / `#5a2820` |

**Structure:** Wide convolution folds across the top half (alternating highlight and shadow pixels simulate gyri and sulci). The shape tapers to a brainstem at bottom-centre. Dark fissure pixels (`#a85a50`) interrupt the highlight rows at irregular intervals to suggest depth.

**Subtitle convention:** `BRAIN 2.0 // [EDITION NAME]` in IBM Plex Mono 11px, `var(--muted)` colour (`#8884b0`).
---

## Effects — what NOT to do

| Effect | Status |
|---|---|
| `border-radius` | ❌ Never |
| `box-shadow` | ❌ Never |
| `text-shadow` | ❌ Never |
| Gradients | ❌ Never |
| Glow / `filter: blur` | ❌ Never |
| CRT scanlines | ❌ Never |
| CSS animations (blink, pulse) | ❌ Avoid — feels AI-generated |
| Emoji | ❌ Never |
| Transitions | ✅ OK at 0.1–0.15s for border-color and background only |

> [!warning] The temptation
> Every retro-themed AI generation reaches for scanlines, glow, and blinking text. Resist all of it. The aesthetic works because it is restrained. The moment you add a glow effect the whole thing reads as costume, not design.

---

## Motifs & copy style

- Section labels: `ALL CAPS`, `letter-spacing: 0.18em`, `color: faint`, followed by a full-width `1px` rule
- Comments / prefixes: `//` before section labels (`// KEY INSIGHT`, `// FILTER`)
- Button brackets: `[ COPY MARKDOWN ]` via `::before`/`::after`
- Chevrons: `▶` rotated 90° when open — never `▼` or `∨`
- Footer: pipe-separated `ITEM // ITEM // ITEM`, all caps, faint colour
- Scrollbar: 6px, thumb `#3d3680`, track `#0e0e16`

---

## HTML boilerplate

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Press+Start+2P&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg:       #0e0e16;
  --nav:      #13131f;
  --surface:  #181828;
  --raised:   #1e1e32;
  --border:   #2a2a44;
  --border2:  #3a3a5c;
  --accent:   #6c5ce7;
  --accent-l: #9d94f0;
  --accent-d: #3d3680;
  --white:    #eceaf8;
  --muted:    #8884b0;
  --faint:    #3e3c60;
  --green:    #4ade80;
  --mono:     'IBM Plex Mono', monospace;
  --pixel:    'Press Start 2P', monospace;
}
body {
  font-family: var(--mono);
  background: var(--bg);
  color: var(--white);
  font-size: 14px;
  line-height: 1.7;
}
</style>
</head>
<body>
<!-- content -->
</body>
</html>
```

---

## Quick-reference card

```
FONTS     Press Start 2P (titles, 7–9px) + IBM Plex Mono (body, 14px/1.7)
BG        #0e0e16
SURFACE   #181828 / #1e1e32
ACCENT    #6c5ce7 → light #9d94f0 → dim #3d3680
TEXT      #eceaf8 / #8884b0 / #3e3c60
BORDERS   1–2px solid, zero border-radius everywhere
ICONS     SVG 28×28, stroke 1.5, currentColor
EFFECTS   none — no shadows, glow, gradients, scanlines, animations
MOTIFS    ALL CAPS labels · // prefixes · [ buttons ] · ▶ chevrons
```

---

*Reference this note when starting a new canvas embed, HTML tool, or any visual project in this vault.*

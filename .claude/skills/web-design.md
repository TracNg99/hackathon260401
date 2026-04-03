---
name: web-design
description: Tommy Dan · Edge8 design system compliance — enforce brand colour palette, Inter typography, white background layout, orange accents, and accessibility standards across Canva graphics and HTML landing pages
type: skill
agents: ["02-graphic-designer", "03-web-designer"]
---

# Skill: web-design

## Purpose
Enforce Tommy Dan · Edge8's visual identity consistently across all design outputs — Canva graphics (Agent 02) and HTML landing pages (Agent 03).

## Edge8 Design System

### Colour Palette

```css
/* Base */
--color-bg:       #FFFFFF;   /* white — page background */
--color-nav:      #111111;   /* near-black — navigation bar */
--color-headline: #111111;   /* near-black — all headlines */
--color-body:     #444444;   /* dark grey — body text */
--color-muted:    #888888;   /* medium grey — captions, secondary text */
--color-border:   #E5E5E5;   /* light grey — subtle borders */
--color-surface:  #F5F5F5;   /* off-white — card/section backgrounds */
--color-dark-card:#2D2D2D;   /* dark card — date/detail sidebars */

/* Primary accent */
--color-orange:   #F97316;   /* orange — all CTAs, badges, accents, borders */

/* Nav / footer */
--color-nav-border: #F97316; /* 3px orange bottom border on nav */
```

> **No dark navy backgrounds.** The old `#0F172A`/`#1E293B` palette is retired. All pages use white backgrounds with black headlines and orange accents.

### Typography (Inter font family)

| Role | Size | Weight | Colour | Transform |
|------|------|--------|--------|-----------|
| Graphic headline | 56px | 900 | #111111 | UPPERCASE |
| Graphic sub-headline | 20px | 400 | #444444 | — |
| Page H1 | clamp(36px,5vw,64px) | 900 | #111111 | UPPERCASE |
| Page H2 | clamp(24px,3.5vw,42px) | 900 | #111111 | UPPERCASE |
| Section label | 11px | 800 | #F97316 | UPPERCASE, letter-spacing 0.12em |
| Body text | 16px | 400 | #444444 | — |
| CTA button text | 13–14px | 900 | #FFFFFF on #F97316 or #111111 | UPPERCASE, letter-spacing 0.07em |
| Nav links | 13px | 600 | #CCCCCC | UPPERCASE, letter-spacing 0.05em |
| Footer / caption | 12px | 400 | #888888 | — |

Load via: `<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800;900&display=swap" rel="stylesheet">`

### Layout Rules

**Canva Graphics (1200×630px)**
- White background (#FFFFFF)
- Service badge: top-left, orange (#F97316) rectangle — no rounded corners
- Headline: large black bold UPPERCASE, left-aligned
- Sub-headline: dark grey, smaller weight
- Orange diagonal corner cut: bottom-right (decorative triangle)
- Black diagonal corner cut: top-left (decorative triangle)
- "FREE CONSULTATION" orange badge: top-right
- Logo / Tommy Dan name: top-right area, black on white
- CTA text: orange (#F97316), uppercase, no underline
- No rounded corners on any element

**HTML Landing Pages**
- White background, black bold UPPERCASE headlines
- Dark nav (#111111) with 3px orange bottom border — no rounded corners
- Hero: speaker block (Tommy Dan) + dark detail card side-by-side
- Diagonal corner cuts: black top-left, orange bottom-right
- Max content width: `max-width: 1200px; margin: 0 auto; padding: 0 24px`
- Section padding: `80px 0`
- Orange CTA buttons — sharp square corners (`border-radius: 0`)
- Dark detail card (#2D2D2D) with orange left border for dates/details
- Subscribe band: orange background (#F97316), white text, black CTA button
- Footer: dark (#111111), orange top border, white text

### Accessibility Standards

- Contrast ratio ≥ 4.5:1 for all body text (#444 on #FFF = 9.7:1 ✓)
- All images: `alt` attribute required
- Buttons: `aria-label` on icon-only buttons
- Forms: `<label>` associated with every `<input>`
- Focus ring: `outline: 2px solid #F97316; outline-offset: 2px`
- Viewport meta: `<meta name="viewport" content="width=device-width, initial-scale=1">`

### Component Patterns

**Service Badge**
```html
<span style="display:inline-block;background:#F97316;color:#fff;font-size:11px;font-weight:800;text-transform:uppercase;letter-spacing:0.1em;padding:6px 16px;">
  TAXATION UPDATE
</span>
```

**CTA Button (primary — orange)**
```html
<a href="#" style="display:inline-block;background:#F97316;color:#fff;font-weight:900;font-size:13px;text-transform:uppercase;letter-spacing:0.07em;padding:14px 36px;text-decoration:none;">
  Subscribe Free →
</a>
```

**CTA Button (secondary — black)**
```html
<a href="#" style="display:inline-block;background:#111;color:#fff;font-weight:900;font-size:13px;text-transform:uppercase;letter-spacing:0.07em;padding:14px 36px;text-decoration:none;">
  See How It Works →
</a>
```

**Subscribe Form**
```html
<form id="subscribe-form" style="display:flex;gap:0;flex-wrap:wrap;">
  <input type="email" name="email" required placeholder="your@email.com"
    style="flex:1;min-width:240px;padding:14px 18px;border:2px solid #111;font-size:15px;outline:none;"
    aria-label="Email address" />
  <button type="submit"
    style="background:#F97316;color:#fff;font-weight:900;font-size:13px;text-transform:uppercase;letter-spacing:0.07em;padding:14px 28px;border:none;cursor:pointer;">
    Subscribe Free →
  </button>
</form>
```

**Dark Detail Card**
```html
<div style="background:#2D2D2D;border-left:4px solid #F97316;padding:28px 24px;color:#fff;">
  <div style="font-size:10px;font-weight:800;color:#F97316;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:8px;">THIS WEEK'S UPDATE DETAILS</div>
  <div style="font-size:13px;color:#888;text-transform:uppercase;letter-spacing:0.06em;">Published</div>
  <div style="font-size:32px;font-weight:900;color:#fff;">02 APRIL</div>
</div>
```

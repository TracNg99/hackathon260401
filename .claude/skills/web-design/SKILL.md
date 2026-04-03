---
name: web-design
description: Edge8 design system compliance — enforce brand colour palette, Inter typography, dark card layout, and accessibility standards across Canva graphics and HTML landing pages
type: skill
agents: ["02-graphic-designer", "03-web-designer"]
---

# Skill: web-design

## Purpose
Enforce Edge8's visual identity consistently across all design outputs — Canva graphics (Agent 02) and HTML landing pages (Agent 03).

## Edge8 Design System

### Colour Palette

```css
/* Base */
--color-bg:       #0F172A;   /* dark navy — page/card background */
--color-surface:  #1E293B;   /* slightly lighter — card surfaces */
--color-text:     #F1F5F9;   /* near-white — primary text */
--color-muted:    #94A3B8;   /* slate — secondary text, captions */
--color-border:   #334155;   /* subtle borders */

/* Service accent colours */
--color-taxation: #3B82F6;   /* blue */
--color-audit:    #8B5CF6;   /* purple */
--color-account:  #10B981;   /* green */
```

### Typography (Inter font family)

| Role | Size | Weight | Colour |
|------|------|--------|--------|
| Graphic headline | 48px | 800 | #F1F5F9 |
| Graphic sub-headline | 20px | 400 | #94A3B8 |
| Page H1 | 40px | 800 | #F1F5F9 |
| Page H2 | 28px | 700 | #F1F5F9 |
| Body text | 16px | 400 | #94A3B8 |
| CTA / badge text | 14–16px | 600 | service accent |
| Footer / caption | 12px | 400 | #64748B |

Load via: `<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">`

### Layout Rules

**Canva Graphics (1200×630px)**
- Left zone (0–780px): all text content — headline, sub-headline, badge, CTA
- Right zone (780–1200px): photo placeholder only (Agent 02), real photo after designer handoff
- Dark navy background with subtle left-to-right gradient (#0F172A → #1E293B)
- Service badge: top-left, pill shape, service accent background
- Edge8 logo: top-right of left zone, white version
- Divider line: `{SERVICE_COLOR}` at 40% opacity between headline and CTA

**HTML Landing Pages**
- Mobile-first, Tailwind CSS via CDN
- Hero: full-width graphic as background with dark overlay
- Max content width: `max-w-4xl mx-auto`
- Section padding: `py-16 px-6`
- Card border-top accent: `border-t-4 border-{service-color}`
- Subscribe button: full-width on mobile, inline on desktop

### Accessibility Standards

- Contrast ratio ≥ 4.5:1 for all body text on dark backgrounds
- All images: `alt` attribute required
- Buttons: `aria-label` on icon-only buttons
- Forms: `<label>` associated with every `<input>`
- Focus ring: `focus:ring-2 focus:ring-{service-color}`
- Viewport meta: `<meta name="viewport" content="width=device-width, initial-scale=1">`

### Component Patterns

**Service Badge**
```html
<span class="inline-flex items-center px-3 py-1 rounded-full text-xs font-semibold bg-blue-500/20 text-blue-400 border border-blue-500/30">
  TAXATION UPDATE
</span>
```

**CTA Button**
```html
<button class="w-full sm:w-auto px-6 py-3 bg-blue-500 hover:bg-blue-600 text-white font-semibold rounded-lg transition-colors focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-slate-900">
  Subscribe for Weekly Updates
</button>
```

**Subscribe Form**
```html
<form id="subscribe-form" action="/api/subscribe" method="POST" class="flex flex-col sm:flex-row gap-3">
  <input type="email" name="email" required placeholder="your@email.com"
    class="flex-1 px-4 py-3 bg-slate-800 border border-slate-700 rounded-lg text-slate-100 placeholder-slate-500 focus:ring-2 focus:ring-blue-500"
    aria-label="Email address" />
  <button type="submit" ...>Subscribe</button>
</form>
```

---
name: react-nextjs
description: Responsive HTML landing page patterns for Edge8 — mobile-first, Tailwind CDN, semantic HTML, ARIA accessibility, vanilla JS form handling
type: skill
agents: ["03-web-designer"]
---

# Skill: react-nextjs

## Purpose
Build semantic, accessible, fully responsive HTML landing pages for Edge8's weekly marketing campaign. No JS frameworks — pure HTML + Tailwind CDN + vanilla JS only.

## Invocation
Used in Agent 03, Step 2 (Build Landing Pages).

## Page Structure (Required Order)

```html
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
  <!-- 1. Meta & SEO -->
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="description" content="{SERVICE} update from Edge8 Malaysia" />
  <meta property="og:title" content="{HEADLINE}" />
  <meta property="og:image" content="../{GRAPHIC_PATH}" />
  <meta property="og:type" content="article" />
  <title>{HEADLINE} | Edge8 {SERVICE}</title>

  <!-- 2. Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet" />

  <!-- 3. Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- 4. CSS Variables -->
  <style>
    :root {
      --color-primary:  {colors.primary};
      --color-bg:       #0F172A;
      --color-surface:  #1E293B;
      --color-text:     #F1F5F9;
      --color-muted:    #94A3B8;
    }
    body { font-family: 'Inter', sans-serif; background: var(--color-bg); color: var(--color-text); }
  </style>
</head>
<body>

  <!-- Section 1: Hero -->
  <!-- Section 2: Content -->
  <!-- Section 3: Subscribe -->
  <!-- Section 4: Services -->
  <!-- Section 5: Footer -->

</body>
</html>
```

## Section Templates

### Hero Section
```html
<section class="relative min-h-[60vh] flex items-end" aria-label="Hero">
  <div class="absolute inset-0 bg-cover bg-center" style="background-image: url('../{GRAPHIC_PATH}')" role="img" aria-label="{HEADLINE}"></div>
  <div class="absolute inset-0 bg-gradient-to-t from-slate-900 via-slate-900/70 to-transparent"></div>
  <div class="relative z-10 max-w-4xl mx-auto px-6 pb-12 w-full">
    <span class="inline-flex items-center px-3 py-1 rounded-full text-xs font-semibold mb-4" style="background:color-mix(in srgb,var(--color-primary) 20%,transparent);color:var(--color-primary);border:1px solid color-mix(in srgb,var(--color-primary) 30%,transparent)">
      {SERVICE_TAG}
    </span>
    <h1 class="text-4xl sm:text-5xl font-extrabold text-slate-100 mb-4 leading-tight">{HEADLINE}</h1>
    <p class="text-xl text-slate-300 max-w-2xl">{SUB_HEADLINE}</p>
  </div>
</section>
```

### Content Section
```html
<section class="max-w-4xl mx-auto px-6 py-16" aria-label="Article content">
  <div class="bg-slate-800/50 rounded-2xl p-8 border border-slate-700">
    <p class="text-slate-300 text-lg leading-relaxed mb-6">{NEWS_SUMMARY}</p>
    <a href="{SOURCE_URL}" target="_blank" rel="noopener noreferrer"
       class="inline-flex items-center gap-2 text-sm font-medium hover:underline"
       style="color:var(--color-primary)" aria-label="Read source article">
      Read full source →
    </a>
  </div>
</section>
```

### Subscribe Section
```html
<section class="bg-slate-800/30 py-16" aria-labelledby="subscribe-heading">
  <div class="max-w-2xl mx-auto px-6 text-center">
    <h2 id="subscribe-heading" class="text-3xl font-bold text-slate-100 mb-3">Stay Updated Weekly</h2>
    <p class="text-slate-400 mb-8">Get Edge8's Malaysia compliance brief every Wednesday.</p>
    <form id="subscribe-form" action="/api/subscribe" method="POST" class="flex flex-col sm:flex-row gap-3" novalidate>
      <input type="email" name="email" required placeholder="your@email.com"
        class="flex-1 px-4 py-3 bg-slate-900 border border-slate-700 rounded-lg text-slate-100 placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-slate-900"
        style="--tw-ring-color:var(--color-primary)"
        aria-label="Email address" autocomplete="email" />
      <button type="submit"
        class="px-6 py-3 font-semibold rounded-lg text-white transition-opacity hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-slate-900"
        style="background:var(--color-primary);--tw-ring-color:var(--color-primary)"
        aria-label="Subscribe for weekly updates">
        Subscribe for Weekly Updates
      </button>
    </form>
    <p id="form-message" class="mt-4 text-sm text-slate-400" aria-live="polite"></p>
  </div>
</section>
```

### Services Section
```html
<section class="max-w-4xl mx-auto px-6 py-16" aria-labelledby="services-heading">
  <h2 id="services-heading" class="text-2xl font-bold text-slate-100 mb-8 text-center">Our Services</h2>
  <div class="grid sm:grid-cols-3 gap-6">
    <!-- repeat for each service -->
    <a href="./tax-landing.html"
       class="block bg-slate-800 rounded-xl p-6 border-t-4 hover:bg-slate-700 transition-colors focus:outline-none focus:ring-2"
       style="border-color:#3B82F6;--tw-ring-color:#3B82F6"
       aria-label="Taxation updates">
      <h3 class="font-bold text-slate-100 mb-2">Taxation</h3>
      <p class="text-sm text-slate-400">SSM & HASIL compliance updates for Malaysian businesses.</p>
    </a>
  </div>
</section>
```

## Form Handling (Vanilla JS)
```html
<script>
  document.getElementById('subscribe-form').addEventListener('submit', async (e) => {
    e.preventDefault();
    const msg = document.getElementById('form-message');
    const email = e.target.email.value;
    msg.textContent = 'Subscribing...';
    try {
      const res = await fetch('/api/subscribe', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email })
      });
      msg.textContent = res.ok ? 'You\'re subscribed! Check your inbox.' : 'Something went wrong. Please try again.';
    } catch {
      msg.textContent = 'Connection error. Please try again.';
    }
  });
</script>
```

## Validation Checklist (run before commit)
- [ ] `<meta name="viewport">` present
- [ ] `<meta property="og:image">` set to correct graphic path
- [ ] Subscribe form has `id="subscribe-form"`, `action`, `method`
- [ ] All service cards link to correct `.html` files
- [ ] All images have `alt` text
- [ ] No console errors (test by opening in browser)
- [ ] Renders correctly at 375px (mobile) and 1280px (desktop)

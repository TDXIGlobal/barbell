# BarBell Landing Page — Build Plan

## Context
BarBell is a B2B SaaS gym companion sold to gym operators at $500/month. This landing page is the top of the sales funnel — a gym owner reads an outreach email, clicks a link, and this is what they see. It must stop them in under 5 seconds, qualify them through the form, and direct them to a Calendly call. The page must feel premium and operator-native — not a startup pitch, not a template.

No existing codebase. Output is one file: `index.html` in `c:\Users\Micha\OneDrive\Desktop\TDXI Global\Projects\BarBell\`.

---

## Design Decisions

### Inspiration Reference
Equinox (equinox.com) — premium gym brand, not SaaS. Key signals applied to BarBell:
- Split form layout: left column = pitch + credibility, right column = form (not a centered card)
- Bottom-border-only input fields — underline style, editorial, not boxy SaaS inputs
- Extreme vertical breathing room — each section has one job and a lot of space around it
- No photography needed — the oversized BARBELL wordmark anchors the hero the way their athlete photography does
- Section labels are sparse, minimal, small caps — barely there

### Typography
- **Display / Wordmark:** `Bebas Neue` — geometric, commanding, industrial. Used for BARBELL wordmark, section labels, and stat numbers.
- **Headlines:** `Playfair Display` — bold serif with character. Used for all H1/H2 headline copy.
- **Body:** `DM Sans` — clean, refined humanist. Used for paragraphs, form labels, captions.
- Import via Google Fonts `@import` at top of `<style>`.

### Color Palette (Committed)
- `--bg`: `#0F0F0F` — near-black base
- `--surface`: `#1A1A1A` — card/form surfaces
- `--border`: `#2A2A2A` — subtle borders
- `--accent`: `#C9A84C` — muted gold (single accent color, CTAs, highlights)
- `--accent-hover`: `#B8943A` — slightly deeper on hover
- `--text-primary`: `#F0EDE6` — off-white
- `--text-muted`: `#888888` — secondary text
- `--text-dim`: `#555555` — captions / fine print

### Motion
- Hero: staggered fade-in sequence on load (headline → subheadline → supporting → CTA)
- Sections: `IntersectionObserver` triggers `fade-up` class (opacity 0 → 1, translateY 20px → 0)
- Stats: count-up animation when stat cards scroll into view
- Nav: `backdrop-filter` blur + dark `background-color` after 80px scroll
- CTA button: `background-color` + `letter-spacing` shift on hover — no bounce

---

## File Structure
Single file: `index.html`
- `<head>`: Google Fonts, meta, CSS variables, all styles
- `<body>`: 9 sections + script block at bottom

---

## Section Build Plan

### 1. `<nav>` — Sticky Navigation
- `position: fixed; top: 0; width: 100%; z-index: 100`
- Transparent on load → dark + blur after 80px scroll (JS `scroll` event)
- Left: `BARBELL` in Bebas Neue, linked to `#`
- Right: `<a href="#apply">` CTA button styled with gold accent

### 2. `#hero` — Full Viewport Hero
- `min-height: 100vh; display: flex; align-items: center`
- Background: `#0F0F0F` with subtle CSS noise texture overlay (using `background-image: url("data:image/svg+xml,...")` or a CSS pseudo-element with opacity grain)
- Oversized "BARBELL" watermark behind content at ~4% opacity (absolute positioned, Bebas Neue, pointer-events: none)
- Content stacked with staggered fade-in: headline → subheadline → supporting line → CTA
- Small badge/callout: "50% churn within 6 months — industry average" floating inline near headline
- Primary CTA button + muted fine print beneath

### 3. `#problem` — The Problem (Stats-Forward)
- Section label: small caps, `--text-muted`, Bebas Neue, letter-spaced
- H2 headline in Playfair Display
- Three stat cards in a CSS Grid (`grid-template-columns: repeat(3, 1fr)` → stacks to 1 column on mobile)
- Each card: large stat number (Bebas Neue, ~80px, gold accent) + label below
- Stat numbers use `data-target` attribute — JS count-up fires on IntersectionObserver
- Paragraph block below cards

### 4. `#how-it-works` — Mechanism
- Section label + H2
- Four feature blocks in a 2×2 grid (→ 1 column on mobile)
- Each block: inline SVG icon (Tabler Icons CDN or hand-written minimal SVG) + bold H3 + body text + italic quote
- Fade-up animation staggered per block (delay 0ms, 100ms, 200ms, 300ms)

### 5. `#revenue` — Revenue Layer
- Section label + H2 + body paragraph
- Three tier cards in a row (→ stack on mobile)
- Cards: `--surface` background, `--border` border, tier name (Bebas Neue) + description
- Middle card (Standard) gets gold accent border to indicate recommended
- Small note beneath cards

### 6. `#credibility` — Military Proof Point
- Full-width dark section, centered
- Large blockquote in Playfair Display italic, off-white
- Supporting body paragraph beneath

### 7. `#partnership` — Exclusivity Section
- Section label + H2 + body (Jacksonville / Virginia Beach copy)
- Gold CTA button

### 8. `#apply` — Application Form
- **Split two-column layout** (from Equinox inspo): left column = pitch copy + credibility pull quote; right column = form
  - Left: section label, headline "Tell us about your gym.", subheadline, and a small credibility block ("Military-grade readiness standards. Limited gym partnerships.")
  - Right: form itself — no card border needed since the two-column layout creates visual separation
  - On mobile: stacks to single column, left copy on top, form below
- **Input style:** bottom-border-only — `border: none; border-bottom: 1px solid var(--border); background: transparent; color: var(--text-primary)` — no box inputs, no rounded corners. Matches the editorial Equinox aesthetic.
- Focus state: bottom border transitions to `--accent` gold
- 6 fields (ALL required):
  1. **First Name** — `<input type="text">` — valid if non-empty after trim
  2. **Gym Name** — `<input type="text">` — valid if non-empty after trim
  3. **Email** — `<input type="email">` — valid against `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`
  4. **Member count** — pill button group (Under 50 / 50–150 / 150–300 / 300+) — valid if any pill selected; stored in hidden `<input id="member-count-val">`
  5. **Biggest challenge** — `<textarea>` — valid if non-empty after trim
  6. **Decision-maker** — pill button group (3 options) — valid if any pill selected; stored in hidden `<input id="decision-maker-val">`
- Pill buttons: `<button type="button">` elements that toggle `.selected` class via JS, write value to hidden input on click
- On submit: JS validates all 6 fields, marks invalid fields with `.error` class and inline error message; does NOT submit if any fail

**Qualification branching on submit (valid form):**
- If member count = "Under 50": hide form, show `#waitlist-state` — message: *"Thanks for applying. BarBell is currently optimized for gyms with 50+ active members. We'll reach out as we expand to smaller operators — we'll keep your application on file."* No Calendly shown.
- If member count = any other value: hide form, show `#confirmation-state` — success message + inline Calendly embed (see below)

**Calendly security — no pre-rendered URL in HTML:**
- `YOUR_CALENDLY_URL` is defined as a JS `const` in the `<script>` block at the bottom — never in the HTML markup
- The Calendly inline widget (`<div class="calendly-inline-widget">`) is absent from the HTML entirely on page load
- On confirmed submission, JS dynamically creates the widget div, sets `data-url` attribute to the JS constant, appends it to `#confirmation-state`, then injects the Calendly embed script tag via `document.createElement('script')` — nothing inspectable before form passes validation

### 9. `<footer>` — Footer
- Three-column flex: wordmark | copyright | email
- One line beneath: "BarBell is available exclusively through gym partnership."
- Mobile: stacks to center-aligned single column

---

## JavaScript Plan (inline `<script>` at bottom)

```js
// Calendly URL — defined here only, never in HTML
const CALENDLY_URL = 'YOUR_CALENDLY_URL';

// Nav scroll behavior
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 80);
});

// Hero staggered fade-in (runs on DOMContentLoaded, delay via CSS transition-delay)

// IntersectionObserver for .fade-up elements
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); observer.unobserve(e.target); }
  });
}, { threshold: 0.15 });
document.querySelectorAll('.fade-up').forEach(el => observer.observe(el));

// Stat count-up — separate observer for .stat-number[data-target]
// Uses requestAnimationFrame, 1.5s linear count from 0 to data-target integer

// Pill button selection — per group
document.querySelectorAll('.pill-group').forEach(group => {
  group.querySelectorAll('button').forEach(btn => {
    btn.addEventListener('click', () => {
      group.querySelectorAll('button').forEach(b => b.classList.remove('selected'));
      btn.classList.add('selected');
      group.querySelector('input[type=hidden]').value = btn.dataset.value;
    });
  });
});

// Form submit — validate all 6 required fields
document.getElementById('partner-form').addEventListener('submit', e => {
  e.preventDefault();
  let valid = true;

  // Text/email field validation
  ['first-name', 'gym-name', 'email', 'challenge'].forEach(id => {
    const el = document.getElementById(id);
    const isEmail = id === 'email';
    const ok = isEmail
      ? /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(el.value.trim())
      : el.value.trim().length > 0;
    el.closest('.field').classList.toggle('error', !ok);
    if (!ok) valid = false;
  });

  // Pill group validation
  ['member-count-val', 'decision-maker-val'].forEach(id => {
    const el = document.getElementById(id);
    const ok = el.value.length > 0;
    el.closest('.pill-group').classList.toggle('error', !ok);
    if (!ok) valid = false;
  });

  if (!valid) return;

  // Qualification branch
  const memberCount = document.getElementById('member-count-val').value;
  document.getElementById('partner-form').style.display = 'none';

  if (memberCount === 'Under 50') {
    document.getElementById('waitlist-state').style.display = 'block';
  } else {
    document.getElementById('confirmation-state').style.display = 'block';
    injectCalendly();
  }
});

// Calendly injection — only called after qualified form submission
function injectCalendly() {
  const widget = document.createElement('div');
  widget.className = 'calendly-inline-widget';
  widget.dataset.url = CALENDLY_URL;
  widget.style.minWidth = '320px';
  widget.style.height = '700px';
  document.getElementById('calendly-mount').appendChild(widget);

  const script = document.createElement('script');
  script.src = 'https://assets.calendly.com/assets/external/widget.js';
  script.async = true;
  document.body.appendChild(script);
}
```

---

## CSS Approach

- CSS custom properties at `:root` for all design tokens
- `*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }`
- Mobile-first breakpoints: `@media (max-width: 768px)` for mobile overrides
- `.fade-up { opacity: 0; transform: translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }`
- `.fade-up.visible { opacity: 1; transform: translateY(0); }`
- Stagger via `.fade-up:nth-child(n) { transition-delay: calc(n * 0.1s); }`

---

## Verification

1. Open `index.html` directly in a browser (no server needed)
2. Check hero loads with stagger animation
3. Scroll down — confirm each section fades in
4. Verify stat numbers count up when scrolled into view
5. Click pill buttons — confirm selection state toggles
6. Submit form with empty fields — confirm validation fires
7. Submit valid form — confirm form hides and confirmation appears
8. Resize browser to mobile width — confirm nav, stats, feature blocks, tiers stack correctly
9. Click "Apply for Partnership" in nav — confirm smooth scroll to `#apply`

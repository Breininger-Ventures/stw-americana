# Small Town Windows — CLAUDE.md
## Project Specification & Build Guide

> **Status:** Pitching homepage first. Full site architecture documented below so Claude always builds with the complete structure in mind — but only `index.html` is being built in this phase.

---

## 1. Project Overview

**Client:** Jeramie — Owner, Small Town Windows  
**Business:** Residential window contractor, Northwest Ohio  
**Phone:** (419) 231-0111  
**Email:** smalltownwindows@gmail.com  
**Website:** smalltownwindows.us  
**Concept:** Americana Hometown — patriot navy, American red, star gold. Think small-town Main Street hardware store crossed with a proudly American brand.

### The One Thing Every Page Must Communicate
This is Jeramie's business. He lives here, works here, and treats every home like his own. Not a franchise. Not a call center. A neighbor.

---

## 2. Brand Identity (Non-Negotiable)

### 2.1 Color Tokens
Always defined in `:root`. Never hard-code hex values anywhere else.

```css
:root {
  --navy:      #1B2A4A;   /* Patriot navy — trust band, service panels */
  --navy-dark: #0F1A2E;   /* Near-black navy — nav, hero bg, footer */
  --red:       #C0392B;   /* American red — ALL CTAs, border accents */
  --gold:      #D4AF37;   /* Star gold — logo accent, stats, stars, cursor */
  --white:     #F5F0E8;   /* Warm parchment — page bg, light sections */
  --off:       #EDE8DC;   /* Off-white — alternate sections, cards */
  --slate:     #8090A8;   /* Muted — secondary text, captions */
  --ink:       #1A1A2E;   /* Deep body text on light backgrounds */
  --near-black: #1A1008;  /* Footer only */
}
```

**Hard rules:**
- Pure `#FFF` or `#000` — NEVER. Use tokens above.
- `--red` is for CTAs and emphasis only — one dominant red element per section.
- `--gold` is reserved for the logo "Town" word, stars, stat numbers, and the cursor. Never body text.
- Section rhythm (top → bottom): `--navy-dark` → `--navy` → `--white` → `--navy` → `--red` → `--navy-dark` → `--off` → `--navy` → `--near-black`

### 2.2 Typography

```html
<!-- Always include this import on every page -->
<link href="https://fonts.googleapis.com/css2?family=Fjalla+One&family=Teko:wght@400;500;600;700&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Source+Sans+3:wght@300;400;600&display=swap" rel="stylesheet">
```

| Font | Role | Usage |
|------|------|-------|
| **Fjalla One** | Display / Headlines | All H1, H2, H3. Always `text-transform: uppercase`. 72px hero → 48px section → 32px card |
| **Teko** | Labels / Nav / UI | Nav links, eyebrows, form labels, tags, buttons. Letter-spacing 0.12–0.28em always |
| **Libre Baskerville** | Body / Quotes | Body copy, testimonials (italic), about text. Line-height 1.75–1.8 |
| **Source Sans 3** | Meta / Captions | Small labels, legal text, captions |

**NEVER use:** Inter, Roboto, Arial, system fonts, or any sans-serif for headlines.

### 2.3 Logo Markup
```html
<div class="nav-logo">
  Small<em>Town</em> Windows
  <!-- OR use the STW seal image once provided -->
</div>
```
- "Town" is always `color: var(--gold)` via `em` tag or `span`
- The RWB gradient rule line always appears beneath the wordmark
- Minimum clearspace: 16px digital, 0.25in print

### 2.4 Flag Motifs (Signature Design Elements)
These must appear throughout the site — they are what makes this brand instantly recognizable:

1. **Star Canton Block** — Navy square filled with `★` stars. Top-left of hero, nav watermark
2. **Red & White Stripes** — Alternating vertical/horizontal bands. Right edge of hero, left edge of about section
3. **Diagonal Hatch** — `repeating-linear-gradient(-45deg, rgba(255,255,255,0.03) 0, rgba(255,255,255,0.03) 1px, transparent 1px, transparent 8px)` on ALL red sections
4. **RWB Gradient Rule** — `linear-gradient(90deg, var(--red), var(--white) 50%, var(--navy))` — under every logo, footer top, hero bottom
5. **Stamp / Seal** — Red circle, gold border. Used in hero photo overlay for "★★★★★ 100% Satisfied"
6. **Side Stripe Accent** — 6–8px RWB stripe pattern, left border on white/light sections

### 2.5 Buttons

```css
/* Primary — use for the ONE main CTA per section */
.btn-primary {
  background: var(--red);
  color: var(--white);
  font-family: 'Fjalla One', sans-serif;
  font-size: 15px; letter-spacing: 0.1em; text-transform: uppercase;
  padding: 14px 32px;
  border: none; border-bottom: 3px solid rgba(0,0,0,0.22);
  border-radius: 2px; cursor: pointer;
  position: relative; overflow: hidden;
}
/* Shimmer on hover */
.btn-primary::before {
  content: ''; position: absolute; top: 0; left: -120%;
  width: 60%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transform: skewX(-20deg);
}
.btn-primary:hover::before { animation: shimmer 0.6s ease forwards; }
.btn-primary:active { transform: translateY(1px); }

/* Ghost variants */
.btn-ghost-navy { border: 2px solid var(--navy-dark); color: var(--navy-dark); background: transparent; /* same font/padding as primary */ }
.btn-ghost-red  { border: 2px solid var(--red); color: var(--red); background: transparent; }
.btn-text       { background: none; border: none; color: var(--red); font-family: 'Teko', sans-serif; font-size: 15px; letter-spacing: 0.14em; text-transform: uppercase; display: flex; align-items: center; gap: 8px; transition: gap 0.2s; }

@keyframes shimmer {
  0%   { left: -120%; }
  100% { left: 150%; }
}
```

Star prefix `★` on all primary and secondary CTAs — it's a brand rule.

---

## 3. Animation System

### 3.1 Custom Gold Cursor (Every Page — Desktop Only)
```html
<style>
  @media (pointer: fine) { * { cursor: none; } }
  #cur-dot { width:8px; height:8px; border-radius:50%; background:var(--gold); position:fixed; top:0; left:0; pointer-events:none; z-index:99999; transform:translate(-50%,-50%); opacity:0; transition:transform 0.12s ease, opacity 0.3s; }
  #cur-ring { width:36px; height:36px; border-radius:50%; border:1.5px solid rgba(212,175,55,0.5); position:fixed; top:0; left:0; pointer-events:none; z-index:99998; transform:translate(-50%,-50%); opacity:0; transition:opacity 0.3s, width 0.2s, height 0.2s, border-color 0.2s; }
</style>
<div id="cur-dot"></div>
<div id="cur-ring"></div>
<script>
const dot=document.getElementById('cur-dot'),ring=document.getElementById('cur-ring');
let mx=0,my=0,rx=0,ry=0;
document.addEventListener('mousemove',e=>{mx=e.clientX;my=e.clientY;dot.style.opacity='1';ring.style.opacity='1';});
document.addEventListener('mouseleave',()=>{dot.style.opacity='0';ring.style.opacity='0';});
(function loop(){rx+=(mx-rx)*0.12;ry+=(my-ry)*0.12;dot.style.left=mx+'px';dot.style.top=my+'px';ring.style.left=rx+'px';ring.style.top=ry+'px';requestAnimationFrame(loop);})();
document.querySelectorAll('a,button,.svc-item,.port-item,.review-card').forEach(el=>{
  el.addEventListener('mouseenter',()=>{dot.style.transform='translate(-50%,-50%) scale(2.2)';ring.style.width='52px';ring.style.height='52px';ring.style.borderColor='rgba(212,175,55,0.85)';});
  el.addEventListener('mouseleave',()=>{dot.style.transform='translate(-50%,-50%) scale(1)';ring.style.width='36px';ring.style.height='36px';ring.style.borderColor='rgba(212,175,55,0.5)';});
});
</script>
```
Place just before `</body>` on every page.

### 3.2 Hero Load Sequence (index.html only)
Fires once on page load. Each element enters in sequence:

```javascript
// Sequence: eyebrow (0.1s) → word 1 (0.2s) → word 2 (0.32s) → word 3 (0.44s) 
// → sub (0.6s) → CTAs (0.9s) → photo (0.3s) → stamp bounce (1.1s) → rule wipe (1.4s) → trust items (1.5s+)

// Word march: each .word-inner slides up from translateY(100%) → translateY(0)
// Photo: translateX(24px) opacity:0 → translateX(0) opacity:1
// Stamp: scale(0) rotate(-20deg) → scale(1) rotate(0) using spring: cubic-bezier(0.34,1.56,0.64,1)
// Rule: width: 0 → 100% over 1.4s
// Trust items: staggered translateY(12px) opacity:0 → in, 0.15s apart
```

### 3.3 Scroll Reveal (All Sections)
```javascript
// IntersectionObserver — threshold: 0.15
// Elements with [data-reveal] fade up from translateY(24px) opacity:0
// Siblings stagger: nth-child delay 0.1s apart
// Easing: cubic-bezier(0.16, 1, 0.3, 1) — snappy ease-out
// Duration: 0.6s reveals, 0.2s interactions

const revealObs = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('revealed');
      revealObs.unobserve(entry.target);
    }
  });
}, { threshold: 0.15 });
document.querySelectorAll('[data-reveal]').forEach(el => revealObs.observe(el));
```

```css
[data-reveal] { opacity: 0; transform: translateY(24px); transition: opacity 0.6s ease, transform 0.6s cubic-bezier(0.16,1,0.3,1); }
[data-reveal].revealed { opacity: 1; transform: translateY(0); }
[data-reveal]:nth-child(2) { transition-delay: 0.1s; }
[data-reveal]:nth-child(3) { transition-delay: 0.2s; }
[data-reveal]:nth-child(4) { transition-delay: 0.3s; }
```

### 3.4 Stat Counter Roll-Up
Fires when the trust band enters the viewport.
```javascript
function countUp(el, target, duration) {
  const isFloat = !Number.isInteger(target);
  let start = 0;
  const step = target / (duration / 16);
  const timer = setInterval(() => {
    start = Math.min(start + step, target);
    el.textContent = isFloat ? start.toFixed(1) : Math.floor(start);
    if (start >= target) clearInterval(timer);
  }, 16);
}
// Trigger: countUp(stat1El, 5.0, 1200), countUp(stat2El, 100, 1400)
```

### 3.5 CTA Idle Pulse + Shimmer
Primary CTA button on hero and contact section:
```css
.pulse-ring, .pulse-ring-2 {
  position: absolute; inset: -8px;
  border: 2px solid var(--red); border-radius: 4px;
  opacity: 0; animation: pulse-out 3s ease-in-out infinite;
}
.pulse-ring-2 { border-color: var(--gold); animation-delay: 0.5s; }

@keyframes pulse-out {
  0%   { inset: -4px; opacity: 0.6; }
  100% { inset: -20px; opacity: 0; }
}
```
Start pulse animation after 2s delay so it doesn't fire immediately on load.

### 3.6 Service List Stagger
```javascript
// On section entry, stagger items 120ms apart from translateX(-20px)
items.forEach((item, i) => {
  setTimeout(() => {
    item.style.opacity = '1';
    item.style.transform = 'translateX(0)';
  }, 100 + i * 120);
});
```

### 3.7 Portfolio Card Hover
```css
.port-item { overflow: hidden; }
.port-item img, .port-item .port-bg { transition: transform 0.4s cubic-bezier(0.16,1,0.3,1), filter 0.3s; }
.port-item:hover .port-bg { transform: scale(1.04); filter: brightness(1.15); }
.port-overlay { transform: translateY(100%); transition: transform 0.35s cubic-bezier(0.16,1,0.3,1); }
.port-item:hover .port-overlay { transform: translateY(0); }
/* Mini flag pin fades in on hover */
.port-flag { opacity: 0; transform: translateY(-6px); transition: opacity 0.25s 0.1s, transform 0.25s 0.1s; }
.port-item:hover .port-flag { opacity: 1; transform: translateY(0); }
```

### 3.8 Promise Section Stripe Wipe
```css
/* Light sweep across the red section as it enters viewport */
.promise::before {
  content: ''; position: absolute; top: 0; right: 0;
  width: 0; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.06));
  transition: width 1.2s cubic-bezier(0.16,1,0.3,1);
}
.promise.wiped::before { width: 100%; }
```

### 3.9 Animation Rules
- Easing standard: `cubic-bezier(0.16, 1, 0.3, 1)` for all reveals
- Spring easing: `cubic-bezier(0.34, 1.56, 0.64, 1)` for stamp/bounce
- Duration: 0.4s–0.8s reveals · 0.15s–0.25s interactions
- Never use: parallax, auto-play video, particle effects, heavy page transitions
- Always include: `@media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; } }`

---

## 4. Site Architecture

> The full site consists of 5 pages. We are only building `index.html` in this phase. But all navigation links, footer links, and internal hrefs must be coded as if the full site exists — use the filenames below.

```
smalltownwindows.us/
├── index.html          ← BUILD THIS FIRST (current phase)
├── services.html       ← future phase
├── portfolio.html      ← future phase (needs real photos)
├── contact.html        ← future phase
└── about.html          ← future phase
```

### Navigation Links (use on every page)
```html
<a href="index.html">Home</a>
<a href="portfolio.html">Our Work</a>
<a href="services.html">Services</a>
<a href="about.html">About Jeramie</a>
<a href="contact.html">Free Estimate</a>
```

---

## 5. index.html — Homepage Build Spec

Build all sections in this exact order. Do not skip or reorder.

---

### SECTION 01 — Mobile Menu Overlay

Render above everything else in the DOM. Hidden by default, toggled by hamburger.

```html
<div class="mobile-menu" id="mobile-menu">
  <button class="mobile-menu-close" id="menu-close">&times;</button>
  <a href="portfolio.html">Our Work</a>
  <a href="services.html">Services</a>
  <a href="about.html">About Jeramie</a>
  <a href="contact.html">Contact</a>
  <div class="mobile-menu-phone">(419) 231-0111</div>
  <a href="contact.html" class="mobile-menu-cta">★ Free Estimate</a>
</div>
```

**Mobile menu styles:**
- `position: fixed; inset: 0; background: var(--navy-dark); z-index: 200`
- `flex-direction: column; align-items: center; justify-content: center; gap: 36px`
- Links: `font-family: 'Fjalla One'; font-size: 34px; color: var(--white); text-transform: uppercase`
- Phone: `font-family: 'Fjalla One'; font-size: 22px; color: var(--gold)`
- Close button: `position: absolute; top: 24px; right: 24px`
- Open/close via JS: toggle class `.open { display: flex; }`

---

### SECTION 02 — Navigation

**Mobile (< 768px):** hamburger left · logo center · "Estimate" CTA right  
**Desktop (≥ 768px):** logo left · nav links center · phone + "Free Estimate" right

```
Height: 62px mobile / 76px desktop
Background: var(--navy-dark)
Border-bottom: 4px solid var(--red)
Position: sticky; top: 0; z-index: 100
```

**Logo block:**
```html
<div class="nav-logo-wrap">
  <div class="nav-logo">Small<em>Town</em> Windows</div>
  <!-- RWB rule line beneath -->
  <div class="nav-logo-rule"></div>
  <div class="nav-logo-sub">★ Northwest Ohio's Trusted Installer ★</div>
</div>
```
- Logo: `font-family: 'Fjalla One'; font-size: 22px (mobile) / 24px (desktop); color: var(--white)`
- `em` tag: `color: var(--gold); font-style: normal`
- Rule: `height: 2px; background: linear-gradient(90deg, var(--red), var(--white) 50%, var(--navy)); opacity: 0.5`
- Sub: `font-family: 'Teko'; font-size: 10px; letter-spacing: 0.2em; color: rgba(245,240,232,0.3); display: none on mobile`

**Desktop nav links:** `font-family: 'Teko'; font-size: 14px; letter-spacing: 0.14em; text-transform: uppercase; color: rgba(245,240,232,0.55)`  
**Hover:** `color: var(--white)` + red underline slides in from left via `::after { width: 0 → 100% }`

**Phone block (desktop only):**
```html
<div class="nav-phone-wrap">
  <div class="nav-phone">(419) 231-0111</div>
  <div class="nav-phone-label">Call or Text</div>
</div>
```
- Phone: `font-family: 'Fjalla One'; font-size: 20px; color: var(--white)`
- Label: `font-family: 'Teko'; font-size: 10px; letter-spacing: 0.14em; color: rgba(245,240,232,0.3)`

**CTA button:** `background: var(--red); font-family: 'Fjalla One'; letter-spacing: 0.08em; border-bottom: 2px solid rgba(0,0,0,0.2)` + shimmer on hover

---

### SECTION 03 — Hero

**Mobile:** Full-width photo (56vw tall, min 260px) stacked above dark content block  
**Desktop:** 50/50 split — content left, photo right, `min-height: 88vh`

**Left panel / mobile content block:**
- Background: `var(--navy-dark)`
- Star canton decoration: `position: absolute; top: 0; left: 0; width: 140px; height: 110px; background: var(--navy); opacity: 0.4` filled with `★` stars in a grid
- Right-edge stripes: alternating `var(--red)` and `rgba(245,240,232,0.15)` vertical bands, 16px wide

**Copy — exact text:**
```
Eyebrow: "Serving Northwest Ohio Since Day One"
H1 line 1: "Your Neighbor."
H1 line 2: "Your"  [color: var(--red)]
H1 line 3: "Window Guy."  [color: var(--gold)]
Sub: "Custom energy-efficient windows installed with craftsman precision — backed by a lifetime transferable warranty and a promise to treat your home like our own."
CTA 1: "★ Schedule Free Estimate"  → href="contact.html"
CTA 2: "See Our Work ↓"  → href="portfolio.html"
```

**H1 animation markup:**
```html
<h1 class="hero-h1">
  <span class="word"><span class="word-inner">Your Neighbor.</span></span>
  <span class="word"><span class="word-inner red">Your</span></span>
  <span class="word"><span class="word-inner gold">Window Guy.</span></span>
</h1>
```
`.word { overflow: hidden; display: block; }` — `.word-inner { transform: translateY(100%); }` — animates to `translateY(0)` on load

**Stats row (pinned to bottom of left panel, desktop only):**
```
★ 5.0  |  Google Reviews
100+   |  Homes Served
BBB A+ |  Accredited
∞      |  Lifetime Warranty
```
- `position: absolute; bottom: 40px; left: 64px; right: 64px`
- `border-top: 1px solid rgba(255,255,255,0.08)`
- Num: `font-family: 'Fjalla One'; font-size: 26px; color: var(--gold)`
- Label: `font-family: 'Teko'; font-size: 11px; letter-spacing: 0.14em; color: rgba(245,240,232,0.35)`
- Trigger `countUp()` when trust band enters viewport

**Right panel (photo):**
- Background placeholder: `linear-gradient(160deg, #243050 0%, #0F1520 100%)`
- Window frame decoration: `border: 1.5px solid rgba(245,240,232,0.06); inset: 28px`
- Label: `[ Hero Project Photo — replace with real job site photo ]`

**Stamp overlay:**
```html
<div class="hero-stamp" id="hero-stamp">
  <div class="stamp-stars">★★★★★</div>
  <div class="stamp-text">100%<br>Satisfied</div>
</div>
```
- `position: absolute; bottom: 52px; left: -28px`
- `width: 116px; height: 116px; border-radius: 50%`
- `background: var(--red); border: 3px solid var(--gold)`
- Animates in: `scale(0) rotate(-20deg)` → `scale(1) rotate(0)` with spring easing at 1.1s delay

---

### SECTION 04 — Trust Band

```
Background: var(--navy)
Padding: 20px 52px (desktop) / scrollable horizontal strip (mobile)
Border-bottom: 3px solid var(--navy-dark)
```

**5 trust items:**
```
⭐  5.0 Stars on Google     / All Verified Reviews
🛡  BBB Accredited · A+    / Better Business Bureau
♾️  Lifetime Warranty       / Parts & Labor Covered
💰  Transparent Pricing     / No Surprise Fees · Ever
🏠  100+ Homes Served       / Northwest Ohio
```

Each item: `display: flex; align-items: center; gap: 10px`  
Value: `font-family: 'Fjalla One'; font-size: 15px; color: var(--white)`  
Label: `font-family: 'Teko'; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color: rgba(245,240,232,0.35)`  
Separator: `width: 1px; height: 32px; background: rgba(255,255,255,0.08)`  
Mobile: `overflow-x: auto; scrollbar-width: none` — items are `flex-shrink: 0`  
Animation: staggered fade-up, 0.15s apart, trigger on viewport entry

---

### SECTION 05 — About / Story

**Mobile:** Photo top → content below  
**Desktop:** 50/50 split — `grid-template-columns: 1fr 1fr`

```
Left panel: background var(--white), padding 80px 64px
  → Side stripe accent: 8px RWB stripe on left edge
Right panel: photo placeholder, background linear-gradient(#3E2810, #1A0E04)
```

**Left panel copy — exact text:**
```
Eyebrow (red): "The Story Behind the Name"
H2: "Small town values."
H2 em: "Big-city craft."
Body p1: "Jeramie built Small Town Windows on a simple idea: your neighbors deserve the same quality workmanship that big-city homeowners pay twice the price for. Every window we install, we treat like it's going into our own home."
Body p2: "Because around here, we'll see you at the grocery store — and our reputation means everything."
```

**Owner card:**
```html
<div class="owner-card">
  <div class="owner-avatar">J</div>
  <div>
    <div class="owner-name">Jeramie — Owner &amp; Installer</div>
    <div class="owner-title">Calling Northwest Ohio home since birth.</div>
  </div>
</div>
```
- Card: `background: var(--off); border-left: 3px solid var(--red); padding: 18px 20px`
- Avatar: `width: 48px; height: 48px; border-radius: 50%; background: var(--navy-dark); border: 2px solid var(--gold); font-family: 'Fjalla One'; font-size: 20px; color: var(--white)`

**Photo panel:** Two stacked placeholders in a 2-col grid, each 160px tall  
`[ Jeramie / crew photo ]` and `[ Job site in progress ]`

Apply `[data-reveal]` to: eyebrow, H2, both body paragraphs, owner card — staggered

---

### SECTION 06 — Services

**Mobile:** Stacked tap-list (accordion style)  
**Desktop:** 50/50 split panel — left: about continuation OR `--navy` bg with service list

```
Background: var(--navy)
Padding: 80px 64px
```

**Eyebrow (gold):** "What We Do"  
**H2 (white):** "Windows done right," / **em (gold):** "start to finish."

**3 services — tap list on mobile, visible all on desktop:**
```
01  Window Replacement
    Custom-fit replacements for homes of all ages. We measure twice,
    install once, and clean up completely.

02  Energy-Efficient Glass
    Low-E coatings and argon-fill insulation that cut your heating
    and cooling bills season after season.

03  New Construction
    Working with builders from framing to final inspection. We fit
    your schedule and your vision.
```

Each service row:
- `background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.06); border-left: 4px solid var(--red)`
- `padding: 18px 16px; border-radius: 0 2px 2px 0`
- Hover: `background: rgba(255,255,255,0.08); border-left-color: var(--gold); transform: translateX(4px)`
- Arrow nudges right on hover
- Link → `services.html`
- Apply `[data-reveal]` with stagger — slides in from `translateX(-20px)`

---

### SECTION 07 — Promise

```
Background: var(--red)
Padding: 80px 64px desktop / 48px 18px mobile
Position: relative; overflow: hidden
Diagonal hatch texture on background (see Section 2.4, motif 3)
```

**Mobile:** Checklist stacked → warranty pill below  
**Desktop:** `grid-template-columns: 1fr 1fr; gap: 80px`

**Left — copy — exact text:**
```
Stars row: ★ ★ ★ ★ ★ (white, small)
H2: "We don't just install windows."
H2 continued: "We earn trust."

Checklist:
✓  Transparent, Itemized Pricing
   Clear price before any work begins. No surprises, ever.

✓  Clean Worksite Guaranteed
   We leave your home cleaner than we found it. Every time.

✓  Same-Day Communication
   We communicate clearly and promptly — every step of the way.

✓  Lifetime Transferable Warranty
   Fully transferable to the next owner — real added value to your home.
```

Check circle: `background: rgba(255,255,255,0.2); border: 1.5px solid rgba(255,255,255,0.4); border-radius: 50%; width: 22px; height: 22px`  
Title: `font-family: 'Teko'; font-weight: 600; color: var(--white)`  
Desc: `font-family: 'Libre Baskerville'; font-style: italic; color: rgba(255,255,255,0.65)`

**Right — warranty box:**
```html
<div class="warranty-box">
  <!-- Star decoration: position absolute top:-14px, centered -->
  <div class="warranty-word">Lifetime</div>
  <div class="warranty-sub">★ Fully Transferable Warranty on Every Install ★</div>
  <div class="warranty-desc">
    We'll come to you, measure your windows, walk you through your
    options, and give you a clear price — no pressure, no sales tactics.
    Just a neighbor giving you an honest answer.
  </div>
</div>
```
- `background: var(--navy-dark); border: 3px solid var(--gold); border-radius: 2px; padding: 48px 40px; text-align: center`
- Word: `font-family: 'Fjalla One'; font-size: 52px; text-transform: uppercase; color: var(--gold); line-height: 1`
- Sub: `font-family: 'Teko'; font-size: 13px; letter-spacing: 0.2em; text-transform: uppercase; color: rgba(245,240,232,0.4)`
- Desc: `font-family: 'Libre Baskerville'; font-size: 14px; line-height: 1.75; color: rgba(245,240,232,0.65); font-style: italic`

**Section entrance:** Stripe wipe animation fires on section entry (see Animation 3.8)  
Then checklist items slide in from left, 0.15s stagger

---

### SECTION 08 — Portfolio Preview

```
Background: var(--navy-dark)
Padding: 80px 64px desktop / 48px 18px mobile
```

**Header:**
```
Section tag (gold): "Our Work"
H2 (white): "Homes We've"
H2 em (red): "Transformed."
Link right: "View Full Gallery →" → href="portfolio.html"
```

**Filter tabs (desktop / scrollable mobile):**
```
[ All Projects ]  [ Replacement ]  [ Energy Upgrade ]  [ Custom Style ]  [ Storm Windows ]
```
Active: `background: var(--red); border-color: var(--red); color: var(--white)`  
Inactive: `border: 1px solid rgba(255,255,255,0.12); color: rgba(245,240,232,0.45)`

**Grid layout:**
```
Desktop: grid-template-columns: 2fr 1fr 1fr; grid-template-rows: 240px 240px
  → Item 1 spans 2 rows (featured)
Mobile: 2-column grid, item 1 spans full width
```

**5 portfolio items — exact names:**
```
1. (featured) "Victorian Restoration"   Defiance, OH · 8 Windows
2.            "Ranch Upgrade"           Hicksville, OH
3.            "New Build Install"       Bryan, OH
4.            "Full Re-Window"          Paulding, OH
5.            "Energy Upgrade"          Napoleon, OH
```

Each item:
- Background: dark gradient placeholder (will be replaced with real photos)
- Mini flag pin: `position: absolute; top: 10px; right: 10px` — 5 horizontal stripes (red/white alternating), 18px wide — fades in on hover
- Overlay slides up from bottom on hover (see Animation 3.7)
- Name: `font-family: 'Fjalla One'; font-size: 17px; color: var(--white); text-transform: uppercase`
- Tag: `font-family: 'Teko'; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--gold)`

**Photo placeholders:** Use `linear-gradient` dark tones in brown/navy/green variants to suggest real photos. Comment each: `<!-- Replace with real project photo: jeramie-defiance-victorian.jpg -->`

---

### SECTION 09 — Reviews

```
Background: var(--off)
Padding: 80px 64px desktop / 48px 18px mobile
```

**Header:**
```
Section tag (red): "What Neighbors Are Saying"
H2: "Real reviews."
H2 em (red): "Real neighbors."
```

**3 review cards (desktop 3-col, mobile stacked):**
```
Card 1:
★★★★★
"Jeramie showed up when he said he would, kept us informed the
whole way, and left our home spotless."
Sarah M. — Defiance, OH

Card 2:
★★★★★
"Energy savings were noticeable after the very first month.
The quality of the installation is flawless."
Mike & Linda T. — Bryan, OH

Card 3:
★★★★★
"Honest, straightforward advice from a guy who genuinely knows
his craft. No pushy sales tactics."
Dave K. — Napoleon, OH
```

Card styles:
- `background: var(--white); border-top: 5px solid var(--navy-dark); padding: 28px 24px`
- Stars: `color: var(--gold); font-size: 13px; letter-spacing: 2px`
- Quote text: `font-family: 'Libre Baskerville'; font-size: 14px; font-style: italic; line-height: 1.75; color: var(--ink)`
- Author: `font-family: 'Teko'; font-size: 13px; font-weight: 600; color: var(--navy); letter-spacing: 0.05em; text-transform: uppercase`
- Large decorative `"` mark: `position: absolute; top: 8px; right: 16px; font-family: 'Fjalla One'; font-size: 64px; color: var(--navy); opacity: 0.05`

**Aggregate bar:**
```
Background: var(--navy-dark); border-top: 3px solid var(--red); padding: 24px 32px
Items: 5.0 / ★★★★★ / BBB A+ / 100%
Separators: 1px rgba(255,255,255,0.08) lines
```
Apply `[data-reveal]` to all three cards — stagger 0.12s

---

### SECTION 10 — Service Area

```
Background: var(--white)
Padding: 80px 64px desktop / 48px 18px mobile
Desktop: grid-template-columns: 1fr 1fr; gap: 72px; align-items: center
Side stripe: 8px RWB stripe on RIGHT edge (mirrors About section)
```

**Left: map placeholder**
```html
<div class="area-map">
  <div class="area-map-icon">🗺️</div>
  <div class="area-map-label">[ Interactive Service Area Map ]</div>
  <!-- 5 red dot pins scattered across the map area -->
</div>
```
- `height: 340px; background: linear-gradient(160deg, #1B2A4A, #0F1825); border-radius: 2px; border: 2px solid rgba(27,42,74,0.2)`
- Map pins: `width: 11px; height: 11px; border-radius: 50%; background: var(--red); box-shadow: 0 0 0 3px rgba(192,57,43,0.2); position: absolute`

**Right: copy — exact text:**
```
Section tag (red): "Where We Work"
H2: "Proudly serving"
H2 em (red): "Northwest Ohio."
Body: "We'll come to you — no travel fees, no out-of-town crews. Just
local folks doing quality work in the communities we call home."
```

**Town pills:**
```
Defiance · Hicksville · Bryan · Paulding · Montpelier · Wauseon · Napoleon · Archbold · + More
```
- `background: var(--off); border: 1px solid rgba(27,42,74,0.12); border-radius: 2px; font-family: 'Teko'; font-size: 13px; letter-spacing: 0.08em; text-transform: uppercase; color: var(--navy)`

**CTA:** `"★ View Full Service Area Map"` → `href="contact.html"` (navy dark bg)

---

### SECTION 11 — Contact CTA

```
Background: var(--navy)
Padding: 80px 64px desktop / 48px 18px mobile
Desktop: grid-template-columns: 1fr 1fr; gap: 80px; align-items: start
RWB stripe footer: position: absolute; bottom: 0; left: 0; right: 0; height: 7px
  → red 33% / white 33% / navy-dark 33%
```

**Left — copy — exact text:**
```
Section tag (gold): "Get Started"
H2 (white): "Ready for new"
H2 em (red): "windows?"
Body: "We'll come to you, measure your windows, walk you through your
options, and give you a clear price — no pressure, no sales tactics.
Just a neighbor giving you an honest answer."
```

**Contact options:**
```html
<a href="tel:4192310111" class="contact-btn">
  <div class="contact-icon">📞</div>
  <div>
    <div class="contact-label">Call or Text</div>
    <div class="contact-value">(419) 231-0111</div>
  </div>
</a>
<a href="mailto:smalltownwindows@gmail.com" class="contact-btn">
  <div class="contact-icon">✉️</div>
  <div>
    <div class="contact-label">Email</div>
    <div class="contact-value">smalltownwindows@gmail.com</div>
  </div>
</a>
<div class="contact-btn">
  <div class="contact-icon">⏱</div>
  <div>
    <div class="contact-label">Hours</div>
    <div class="contact-value">Mon–Sat, 7am–7pm</div>
  </div>
</div>
```
Contact btn: `background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.12); border-left: 4px solid var(--red); border-radius: 0 2px 2px 0; padding: 14px 16px`

**Right — estimate form:**
```
Container: background var(--navy-dark); border-top: 5px solid var(--gold); padding: 36px 32px
Title: "★ Request a Free Estimate"  (Fjalla One, white)
Subtitle: "No commitment. No pressure. Just an honest quote."  (Teko, slate)

Fields:
- First Name / Last Name  (2-col grid)
- Phone Number
- City / Town
- Tell Us About Your Project  (textarea, min-height 88px)

Submit: "★ Send My Estimate Request →"
  background: var(--red); width: 100%
  Add CTA pulse rings around button (see Animation 3.5)
```

Form input styles:
```css
.form-input, .form-textarea {
  width: 100%;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 2px; padding: 11px 13px;
  color: var(--white);
  font-family: 'Source Sans 3', sans-serif; font-size: 14px;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.form-input:focus, .form-textarea:focus {
  border-color: var(--gold);
  box-shadow: 0 0 0 3px rgba(212,175,55,0.1);
}
```
Label: `font-family: 'Teko'; font-size: 11px; letter-spacing: 0.14em; text-transform: uppercase; color: rgba(212,175,55,0.55)`  
Label on focus: `color: var(--gold)` (use `.form-group:focus-within .form-label`)

---

### SECTION 12 — Footer

```
Background: var(--near-black) → #1A1008
Border-top: 5px solid var(--red)
Padding: 52px 64px 28px desktop / 40px 18px 24px mobile
```

**RWB stripe rule at top:**
```html
<div class="footer-rwb">
  <span></span><span></span><span></span>
</div>
```
`display: flex; height: 4px; border-radius: 2px; overflow: hidden` — red / white / navy thirds

**Top grid (desktop: 4-col, mobile: 2-col):**
```
Col 1 (2fr): Logo + tagline + short description
Col 2:       Services links
Col 3:       Company links
Col 4:       Contact links
```

**Brand column copy:**
```
Logo: SmallTown Windows  (Fjalla One, white — "Town" gold)
Tagline: ★ Installed once. Trusted for life. ★  (Teko, gold/50%)
Description: "Serving Northwest Ohio with pride. Licensed & Insured · BBB Accredited."
(Libre Baskerville italic, white/28%)
```

**Services links:** Window Replacement · Energy-Efficient Glass · New Construction · Free Estimates → `services.html`  
**Company links:** About Jeramie · Our Work · Reviews · Service Areas → respective pages  
**Contact links:** (419) 231-0111 · smalltownwindows@gmail.com · ★★★★★ Google · BBB Accredited

**Bottom bar:**
```
Left: © 2025 Small Town Windows · All Rights Reserved
Right: 🛡 BBB Accredited · A+ · ★ Lifetime Warranty
Font: Teko, 11px, letter-spacing 0.1em, rgba(245,240,232,0.15)
Border-top: 1px solid rgba(255,255,255,0.05)
```

---

## 6. Mobile Responsiveness Rules

Apply these breakpoints:

```css
/* Mobile-first base styles: single column, full-width */
/* Tablet: 768px */
@media (min-width: 768px) {
  /* Activate all desktop splits and grids */
  /* Hero becomes 50/50 */
  /* About becomes 50/50 */
  /* Services becomes 3-col grid */
  /* Promise becomes 2-col */
  /* Portfolio becomes asymmetric masonry */
  /* Reviews becomes 3-col */
  /* CTA becomes 2-col */
  /* Footer becomes 4-col */
}
/* Large desktop: 1100px */
@media (min-width: 1100px) {
  /* Max content width: 1200px centered */
  /* Increase hero H1 to 62–72px */
}
```

**Mobile-specific rules:**
- All CTAs: `display: block; width: 100%; text-align: center`
- Trust strip: `overflow-x: auto; scrollbar-width: none; -webkit-overflow-scrolling: touch`
- Portfolio filters: horizontal scroll, no wrap
- Town pills: `flex-wrap: wrap`
- Hamburger: 48px tap target minimum
- Hide custom cursor entirely on mobile: `@media (pointer: coarse) { * { cursor: auto; } #cur-dot, #cur-ring { display: none; } }`

---

## 7. Image Placeholders → Real Photos

All images are placeholders for the pitch. When real photos are provided, swap these:

| Placeholder | Replace With | Notes |
|-------------|-------------|-------|
| Hero right panel | Best completed job site photo | Bright, sharp, full window visible |
| About left panel (top) | Photo of Jeramie on a job | Approachable, in work gear |
| About left panel (bottom) | Photo of finished install | Shows clean work, proud homeowner optional |
| Portfolio item 1 (featured) | Best full-home project | Defiance Victorian |
| Portfolio items 2–5 | Individual project shots | Match towns listed |

All `<img>` tags: `width: 100%; height: 100%; object-fit: cover; display: block`

---

## 8. Performance & Accessibility

- All images need `alt` attributes: `alt="Window installation by Small Town Windows in [City], Ohio"`
- Form inputs need `id` + `<label for="">` associations
- Nav needs `aria-label="Main navigation"`
- Mobile menu needs `role="dialog" aria-modal="true"`
- Color contrast: all text on dark backgrounds passes WCAG AA (tested against `--navy-dark`)
- Fonts load via `display=swap` to prevent FOIT
- Preconnect to Google Fonts: `<link rel="preconnect" href="https://fonts.googleapis.com">`
- No unused JS libraries — vanilla JS only for this project

---

## 9. File Structure

```
/smalltownwindows/
├── index.html              ← homepage (build now)
├── services.html           ← future
├── portfolio.html          ← future
├── contact.html            ← future
├── about.html              ← future
├── css/
│   └── styles.css          ← shared styles (nav, footer, buttons, tokens)
│                              OR inline styles in each HTML file for pitch phase
├── js/
│   └── main.js             ← shared JS (cursor, scroll reveal, mobile menu)
│                              OR inline scripts for pitch phase
└── images/
    ├── hero-photo.jpg      ← replace placeholder
    ├── jeramie-photo.jpg   ← replace placeholder
    └── projects/
        ├── defiance-victorian.jpg
        ├── hicksville-ranch.jpg
        ├── bryan-newbuild.jpg
        ├── paulding-rewindow.jpg
        └── napoleon-energy.jpg
```

> **Pitch phase:** All CSS and JS can be inline within `index.html`. Split into separate files before handing off to client or going live.

---

## 10. Build Checklist — index.html

Work through this in order. Do not move to the next item until the current one renders correctly.

- [ ] `<head>` — title, meta, viewport, Google Fonts preconnect + import, favicon placeholder
- [ ] CSS variables in `:root` + reset + body base
- [ ] Animation CSS (`[data-reveal]`, keyframes, cursor styles)
- [ ] Mobile menu overlay
- [ ] Navigation — mobile first, then `@media (min-width: 768px)` desktop
- [ ] Hero — mobile stack first, then desktop split
- [ ] Trust band — scrollable mobile, flex desktop
- [ ] About section — mobile stack, desktop split
- [ ] Services — mobile tap-list, desktop panel
- [ ] Promise — mobile stack, desktop 2-col + stripe wipe
- [ ] Portfolio — mobile 2-col, desktop masonry
- [ ] Reviews — mobile stack, desktop 3-col
- [ ] Service area — mobile stack, desktop split
- [ ] Contact CTA + form — mobile stack, desktop split
- [ ] Footer — mobile 2-col, desktop 4-col
- [ ] JS: mobile menu open/close
- [ ] JS: hero load sequence
- [ ] JS: scroll reveal IntersectionObserver
- [ ] JS: stat counters
- [ ] JS: CTA idle pulse
- [ ] JS: gold cursor trailer
- [ ] Final: `prefers-reduced-motion` media query wrapping all animations
- [ ] Final: all `alt` attributes, form `label` associations, aria labels
- [ ] Final: test at 375px, 768px, 1200px widths

---

## 11. What NOT to Build (Yet)

The following are scoped out of this phase. Do not build, stub, or reference in code:

- Blog / news section
- Live chat widget
- Google Maps embed (use static placeholder)
- Photo gallery lightbox (just link to `portfolio.html`)
- Email form backend (form submits to `#` for pitch — note in comments)
- Animations beyond what's listed in Section 3
- Any page other than `index.html`

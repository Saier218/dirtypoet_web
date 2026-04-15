# DESIGN.md · 脏诗 dirtyPoet
**Version 1.3 · April 2026**
Single-file website: `dirtyPoet_website.html`

> This is the single source of truth for all visual and interaction decisions.
> Any person or AI agent working on this codebase must read this file first.
> When in doubt, return to the Brand North Star.

---

## Brand North Star

**脏诗 dirtyPoet** — niche Chinese outdoor/climbing lifestyle brand, Hangzhou.
Registered under 杭州念界造物科技有限公司.

**Manifesto:** 把手弄脏的人，在写诗
("Those who get their hands dirty are writing poems.")

**Action slogan:** Get your hands dirty. Write your way out.

**This design feels like:**
A day on a mountain — morning mist through rock, golden hour, campfire at night,
first light before dawn. Shot on film. Rendered in watercolor. Grain of old paper.

**This design does NOT feel like:**
Tech product. Luxury fashion. Clean SaaS. Futuristic. Neon. Digital mesh.

**Four aesthetic values:** rough · poetic · real · organic
**Four anti-values:** tech · futuristic · polished · promotional

---

## Website Purpose & Scope

Brand mothership. Not a commerce platform.

| Function | This site | Where instead |
|----------|-----------|---------------|
| Brand storytelling | Yes | — |
| Pre-sale intent collection | Yes (email form) | — |
| Community photo showcase | Yes — 4 featured, links to XHS | Xiaohongshu #脏诗社 |
| Product display pages | Future (no purchase UI) | — |
| Purchasing | Never | Xiaohongshu / Taobao |
| Cart / checkout | Never | External platforms |

**Rule:** Any CTA leading to a purchase must link to an external platform and
indicate this with ↗ or a label ("前往小红书"). Never build checkout logic here.

---

## 1. Color System

### 1a. Global Tokens

```css
--paper:      #E8E4DC   /* dawn white — primary bg, lightest surface */
--ink:        #44321D   /* warm dark brown — primary text */
--coal:       #4A4A4A   /* neutral charcoal — secondary text */
--rust:       #9B6A4A   /* burnt rust — brand accent, CTAs, eyebrows */
--moss:       #445340   /* dirty moss — secondary brand, nature */
--green-deep: #213722   /* forest green — dark section base */
--blue-deep:  #003740   /* deep teal — reserved, darkest night */
--paper-dark: #D9D4CA   /* one step darker than paper */
```

### 1b. Semantic Surface Tokens

Choose by emotional role, not page position.
When adding any new section or page, pick the token matching its emotional intent.

| Token | Base | Emotional role | Use for |
|-------|------|----------------|---------|
| `--surface-dawn` | `#E8E4DC` | Beginning, clarity, return | Hero, product detail pages, any re-entry moment |
| `--surface-morning` | `#C4845A` warm orange | Energy, warmth, activation | Season strip, product accents |
| `--surface-golden` | `#7A3A1A` burnt orange | Richness, peak energy, tension | Manifesto, key brand message sections |
| `--surface-dusk` | orange→green collision | Transition, complexity, coexistence | Product showcase, bridging sections |
| `--surface-night` | `#0A1A0F` deep green | Depth, calm, focus | Presale form, long-form content |
| `--surface-deep` | `#091830` blue-black | Mystery, community, belonging | Community section, darkest areas |
| `--surface-return` | `#3A2818` warm brown | Completion, full circle | Footer, closing sections |

**Critical rules:**
- Adjacent sections must never share the same surface token
- Content length never determines color — only emotional role does
- Never use the same token twice in a row, regardless of section size

### 1c. Section Backgrounds — Full Specification

Every background is `radial-gradient`. Never flat solid fills.
Every section has an internal light source — brighter zone + darker zone = breathing room.

**NAV (fixed)**
```css
background: rgba(232, 228, 220, 0.94);
backdrop-filter: blur(10px);
border-bottom: 1px solid rgba(68, 50, 29, 0.10);
```

**#hero — Dawn**
```css
background: var(--paper); /* #E8E4DC — plain paper, no gradient on section itself */

/* Right side: real photo .hero-photo */
.hero-photo {
  position: absolute; right: 0; top: 0; bottom: 0; width: 68%;
  background-image: url('/* base64 photo */');
  background-size: cover;
  background-position: center right;
}

/* Left fade overlay .hero-photo-fade */
.hero-photo-fade {
  position: absolute; left: 0; top: 0; bottom: 0; width: 72%; z-index: 2;
  background: linear-gradient(90deg,
    #E8E4DC 0%, #E8E4DC 22%,
    rgba(232,228,220,0.97) 35%, rgba(232,228,220,0.85) 48%,
    rgba(232,228,220,0.55) 62%, rgba(232,228,220,0.20) 76%,
    transparent 100%);
}

/* Static ambient .hero-bg (subtle, behind photo fade) */
background:
  radial-gradient(ellipse 60% 80% at 75% 30%, rgba(68,83,64,0.12) 0%, transparent 70%),
  radial-gradient(ellipse 40% 60% at 20% 70%, rgba(155,106,74,0.09) 0%, transparent 60%);

/* Mouse-tracking orbs via JS — see Section 7 */
/* Orb 1: rgba(180,100,50,0.28)  Orb 2: rgba(90,110,70,0.18)  lerp: 0.05 */
```

**All text in #hero uses dark ink (light background):**
```css
.hero-eyebrow:    color: rgba(68, 50, 29, 0.38)
.hero-title:      color: var(--ink)
.hero-title em:   color: var(--rust)
.hero-tagline:    color: rgba(68, 50, 29, 0.62)
.hero-scroll:     color: rgba(68, 50, 29, 0.28)
```

**season-strip — Dawn to Morning**
```css
background: linear-gradient(90deg, #E0D8CC 0%, #C4845A 45%, #9B5A35 100%);
```

**#manifesto — Morning warmth**
```css
background: radial-gradient(ellipse 130% 80% at 50% -10%,
  #D4956A 0%, #A85C30 30%, #6B3318 60%, #3D1C0A 100%);
/* Light source: top center, radiating downward */
```

**#world — Golden hour**
```css
background: radial-gradient(ellipse 90% 110% at 15% 60%,
  #7A3A1A 0%, #5A3818 25%, #3A3A20 55%, #253020 80%, #182818 100%);
/* Light source: left side, orange into dark green */
```

**#product — Orange-green collision (visual centerpiece)**
```css
background: radial-gradient(ellipse 70% 100% at 5% 45%,
  #B84A25 0%, #7A3018 22%, #3D3A28 45%, #1E3A2A 68%, #0D2018 100%);
/* Light source: far left, dramatic warm-cool split */
```

**#presale — Into night, base shifts blue**
```css
background:
  radial-gradient(ellipse 50% 40% at 75% 20%,
    rgba(50,80,55,0.9) 0%, transparent 55%),
  radial-gradient(ellipse 40% 30% at 15% 85%,
    rgba(100,60,25,0.25) 0%, transparent 60%),
  linear-gradient(to bottom,
    #0A1A0F 0%, #0A1A0F 55%, #091520 78%, #0D244D 100%);
/* Bottom bleeds into blue — connects to Community */
```

**#community — Deep night**
```css
background: radial-gradient(ellipse 80% 60% at 50% 0%,
  #0D244D 0%, #091830 45%, #04090A 100%);
/* Top matches Presale bottom. Light sinks from top. */
```

**#connect — Pre-dawn split**
```css
/* Left .connect-left */
background: radial-gradient(ellipse 120% 100% at -10% 50%,
  rgba(140,65,25,0.95) 0%, rgba(80,35,12,0.90) 40%, rgba(30,18,8,1) 100%);

/* Right .connect-right */
background: radial-gradient(ellipse 120% 100% at 110% 50%,
  rgba(45,70,45,0.95) 0%, rgba(25,42,25,0.90) 50%, rgba(12,20,12,1) 100%);
```

**footer — Full circle, dawn returns**
```css
background: radial-gradient(ellipse 80% 70% at 20% 110%,
  #C4845A 0%, #7A4A25 30%, #3A2818 60%, #1A140A 100%);
/* Light from bottom-left: first mountain light, echoes Hero */
```

### 1d. Color Journey

```
Hero        #E8E4DC paper + real photo right    dawn
strip       #E0D8CC → #9B5A35                   warming
Manifesto   #D4956A → #3D1C0A                   morning orange
World       #7A3A1A → #182818                   golden hour, orange→green
Product     #B84A25 → #0D2018                   collision, peak
Presale     #0A1A0F → #0D244D                   night, shifts blue
Community   #0D244D → #04090A                   deep night
Connect     warm orange → dark moss             pre-dawn
Footer      #C4845A → #1A140A                   full circle, dawn
```

**Continuity rule:** Bottom edge of each section connects visually to
top edge of the next. Check this after every background change.

### 1e. Text on Dark Sections

```css
Headlines:        var(--paper) #E8E4DC — always full opacity
Body text:        rgba(232, 228, 220, 0.55) — never below this
Labels/eyebrows:  var(--rust) #9B6A4A
Decorative/meta:  rgba(232, 228, 220, 0.20) minimum
```

Never use pure `#FFFFFF`. Always use paper tone.

---

## 2. Typography

### Font Stack — all four always loaded

```html
<link href="https://fonts.googleapis.com/css2?
  family=Playfair+Display:ital,wght@0,400;0,700;1,400;1,700
  &family=Special+Elite
  &family=Space+Mono:ital,wght@0,400;0,700;1,400
  &family=Noto+Serif+SC:wght@300;400;700
  &display=swap" rel="stylesheet">
```

Never substitute system fonts. Never omit any of the four families.

### Font Roles

| Font | Role | Where used |
|------|------|-----------|
| Playfair Display | Display / brand | Headlines, brand name, pull quotes, pillar keywords |
| Special Elite | Typewriter / tagline | Hero taglines only — the handmade, worn feeling |
| Noto Serif SC | Body Chinese | Chinese body text, poetry, brand narratives |
| Space Mono | UI / labels | Navigation, tags, labels, specs, form inputs, all UI text |

### Type Hierarchy

| Level | Font | Size | Weight | Style | Letter-spacing | Line-height |
|-------|------|------|--------|-------|---------------|-------------|
| Hero Title | Playfair Display | `clamp(72px,11vw,140px)` | 700 | normal | `-4px` | `0.92` |
| Section Title | Playfair Display | `clamp(36px,5vw,54px)` | 700 | normal | `-1px` | `1.1` |
| Display Quote | Playfair Display | `clamp(32px,5vw,52px)` | 400 | italic | `-0.5px` | `1.25` |
| Subheading | Playfair Display | `clamp(22px,3vw,38px)` | 700 | normal | `-0.5px` | `1.15` |
| Pillar Keyword | Playfair Display | `clamp(42px,6vw,72px)` | 700 | italic | `-2px` | `0.95` |
| Hero Tagline ZH | Special Elite | `15px` | 400 | normal | `3px` | `1.9` |
| Hero Tagline EN | Special Elite | `11px` | 400 | italic | `2px` | `1.8` |
| Body Chinese | Noto Serif SC | `15px` | 300–400 | normal | `0` | `2.1` |
| Body English | Space Mono | `13px` | 400 | normal | `0` | `1.8` |
| UI Label | Space Mono | `10px` | 400 | uppercase | `3–5px` | `1.7` |
| Caption / meta | Space Mono | `10px` | 400 | normal | `1px` | `1.8` |

**Minimums — never go below:**
- Any body text: `14px`
- UI labels: `9px`
- Footer meta: `10px`
- Body text opacity on dark: `0.55`

### Typography Rules

- Brand name always: `dirty` (normal, `var(--ink)` or `var(--paper)`) + `Poet` (italic, `var(--rust)`)
- Special Elite is used **only** for Hero taglines — nowhere else in the site
- Chinese and English always coexist — never show only one
- Labels always `text-transform: uppercase`, letter-spacing ≥ 3px
- `font-style: italic` only on Playfair Display — never on Special Elite or Noto Serif SC

---

## 3. Grain Texture

Mandatory everywhere. Grain makes backgrounds feel physical, not digital.

**Global grain (body::after)**
```css
body::after {
  content: ''; position: fixed; inset: 0;
  pointer-events: none; z-index: 998; opacity: 0.055;
  background-image: url("data:image/svg+xml,...fractalNoise...");
}
```

**Section-level grain (.has-grain)**
Add to: `#manifesto`, `#world`, `#product`, `#presale`, `#community`, `#connect`, `footer`

```css
.has-grain { position: relative; }
.has-grain > * { position: relative; z-index: 1; }
.has-grain::before {
  content: ''; position: absolute; inset: 0;
  pointer-events: none; z-index: 0; opacity: 0.045;
  background-image: /* same fractalNoise SVG */;
}
```

---

## 4. Components

### Cards on gradient sections
Always semi-transparent — gradient must show through. Never opaque.

```css
background: rgba(255, 255, 255, 0.05);
border: 1px solid rgba(255, 255, 255, 0.08);

/* warm accent card */  background: rgba(163, 81, 57, 0.35);
/* cool accent card */  background: rgba(68, 83, 64, 0.40);
```

### Buttons

**Primary (dark backgrounds)**
```css
background: var(--rust); color: var(--paper);
font-family: 'Space Mono', monospace;
font-size: 10px; letter-spacing: 3px; text-transform: uppercase;
padding: 14px 28px; border: none;
transition: background 0.2s;
/* hover: #b07856 */
```

**Ghost (dark backgrounds)**
```css
background: rgba(255,255,255,0.08);
border: 1px solid rgba(255,255,255,0.15);
color: var(--paper);
/* hover: background rgba(255,255,255,0.15) */
```

**External CTA rule:** Any button leading to purchase includes ↗ or "前往小红书".

### Form inputs
```css
background: rgba(255,255,255,0.07);
border: 1px solid rgba(255,255,255,0.12);
color: var(--paper);
font-family: 'Space Mono', monospace;
font-size: 12px; padding: 14px 20px;
/* placeholder: rgba(232,228,220,0.25) */
/* focus border: rgba(255,255,255,0.30) */
```

### Section anatomy
```
[section-label]    Space Mono 10px uppercase 4px letter-spacing var(--rust)
[section-title]    Playfair Display clamp size 700 weight
[content]
```
Light sections: section-title has `border-bottom: 1px solid rgba(68,50,29,0.12)`.

### Navigation
```css
position: fixed; top: 0; left: 0; right: 0; z-index: 100;
padding: 18px 48px;
background: rgba(232,228,220,0.94);
backdrop-filter: blur(10px);
border-bottom: 1px solid rgba(68,50,29,0.10);
```
Links: Space Mono 9px uppercase 2.5px letter-spacing, `var(--coal)` default, `var(--rust)` hover.
Active nav link (current section): `var(--rust)` + 1px underline `rgba(155,106,74,0.6)`.

---

## 5. Layout & Spacing

**Content width:** `max-width: 1100px; margin: 0 auto`

**Section padding rhythm** — vary intentionally, never uniform:
```
Hero:                min-height: 100vh, padding: 0 48px 80px
Manifesto:           padding: 140px 48px
World, Product:      padding: 120px 48px
Presale, Community:  padding: 120px 48px
Connect:             padding: 100px 48px (split columns, full width)
Footer:              padding: 80px 48px 48px
Season strip:        padding: 12px 48px
```

**Mobile (≤768px):** all padding becomes `80px 20px`, strip `12px 20px`.

**Grid gap:** Always `2px`. Intentional mosaic/tiled quality. Do not increase.

**Grids:**
- Two-column standard: `grid-template-columns: 1fr 1fr; gap: 2px`
- Community photos: `grid-template-columns: repeat(4, 1fr); gap: 2px`
  (4 featured photos — mobile collapses to `1fr 1fr`)

---

## 6. Cursor System — Unified Flashlight

A single custom SVG flashlight cursor (32×32, hotspot 30 7) is used globally.
Source file: `手电光标.svg`. Embedded as base64 in CSS.

```css
body {
  cursor: url("data:image/svg+xml;base64,...") 30 7, crosshair;
}
@media(max-width:768px) { body { cursor: auto; } }
```

**Cursor orb — light on all dark sections (everywhere except #hero):**
A soft radial glow `#cursor-orb` (fixed, 220×220px) follows the cursor across all non-hero sections.
Feels like a flashlight beam landing on a dark surface. Hidden in hero, active everywhere else.

```css
#cursor-orb {
  position: fixed; width: 220px; height: 220px; border-radius: 50%;
  pointer-events: none; z-index: 9997; transform: translate(-50%, -50%);
  background: radial-gradient(circle,
    rgba(240,220,180,0.18) 0%, rgba(210,170,110,0.10) 35%,
    rgba(170,130,80,0.05) 60%, transparent 72%);
  mix-blend-mode: screen; opacity: 0; transition: opacity 0.5s ease;
}
#cursor-orb.active { opacity: 1; }
```

**Section ambient light (on #manifesto and #product):**
An additional slow-moving ambient glow div inside these two sections, following the cursor.
Opacity 0.07 max, CSS transition 0.8s — more subtle than the orb, just warmth.

```css
.section-ambient {
  position: absolute; width: 400px; height: 400px; border-radius: 50%;
  pointer-events: none; z-index: 0 !important;
  transform: translate(-50%, -50%);
  background: radial-gradient(circle, rgba(155,106,74,0.07) 0%, transparent 70%);
  mix-blend-mode: screen;
  transition: left 0.8s ease, top 0.8s ease, opacity 0.4s ease;
  opacity: 0;
}
```

**Hero mouse-tracking orbs (inside #hero only):**
Two warm radial orbs follow cursor with lerp inertia (factor 0.05).
```javascript
// Orb 1: rgba(180,100,50,0.28) — primary position
// Orb 2: rgba(90,110,70,0.18) — offset ~180px / 120px from orb 1
```

**Rules:**
- Single cursor SVG applies globally — no per-section switching
- Cursor orb active everywhere except #hero
- Mobile: `cursor: auto`, orb hidden

---

## 7. Motion & Interaction

### Scroll reveal — bidirectional

Elements animate in when entering viewport AND reset when leaving.

```css
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}
.reveal.visible { opacity: 1; transform: none; }

.reveal-delay-1 { transition-delay: 0.12s; }
.reveal-delay-2 { transition-delay: 0.24s; }
.reveal-delay-3 { transition-delay: 0.38s; }
```

```javascript
const revealObs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); }
    else { e.target.classList.remove('visible'); }
  });
}, { threshold: 0.12 });
document.querySelectorAll('.reveal').forEach(el => revealObs.observe(el));
```

**Stagger assignment within each section:**
```
section-label (eyebrow):  .reveal              (no delay, first)
section-title:            .reveal .reveal-delay-1
body text / subtitle:     .reveal .reveal-delay-2
CTA / form / button:      .reveal .reveal-delay-3
grid cards:               each card gets delay-1, delay-2, delay-3 in order
```

### Nav active state
```javascript
const navObserver = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting && e.intersectionRatio >= 0.3) {
      document.querySelectorAll('.nav-links a').forEach(l => l.classList.remove('nav-active'));
      const link = document.querySelector(`.nav-links a[href="#${e.target.id}"]`);
      if (link) link.classList.add('nav-active');
    }
  });
}, { threshold: 0.3 });
document.querySelectorAll('section[id]').forEach(s => navObserver.observe(s));
```

```css
.nav-links a.nav-active { color: var(--rust); }
.nav-links a.nav-active::after {
  content: ''; display: block;
  width: 100%; height: 1px;
  background: var(--rust); margin-top: 2px; opacity: 0.6;
}
```

### Season strip parallax
```javascript
window.addEventListener('scroll', () => {
  strip.style.transform = `translateX(-${window.scrollY * 0.15}px)`;
}, { passive: true });
```

### Section top-edge softening
Uses `::after` to avoid conflict with `.has-grain::before` grain texture.

```css
.section-fade-top::after {
  content: ''; position: absolute;
  top: 0; left: 0; right: 0; height: 80px;
  background: linear-gradient(to bottom, rgba(0,0,0,0.15) 0%, transparent 100%);
  pointer-events: none; z-index: 0;
}
```
Apply `.section-fade-top` to: `#manifesto`, `#world`, `#product`, `#presale`, `#community`.

---

## 8. Community Section

**Design decision:** 4 featured photos + jump to Xiaohongshu #脏诗社 topic page.

**Photo grid:** `repeat(4, 1fr)`, `gap: 2px`, mobile `1fr 1fr`

**Each card:**
```css
aspect-ratio: 1 / 1.2;
background: rgba(255,255,255,0.04);
border: 1px solid rgba(255,255,255,0.07);
/* on hover: transform: scale(1.01); transition: 0.3s */
```

**CTA below grid:** Links to Xiaohongshu #脏诗社 topic page, not brand account.

**Photo refresh cadence:** Every 3–4 weeks. Replace image sources only — no layout changes.
No stock photos ever.

**XHS strategy (A+C model):**
- Brand account: brand-authored content only
- #脏诗社 tag: user submissions
- Website links to topic page — shows full community, not brand-curated feed

---

## 9. Bilingual System

```html
<span class="zh">中文内容</span>
<span class="en">English content</span>
```
```css
body.lang-zh .en { display: none; }
body.lang-en .zh { display: none; }
```

Default: `body.lang-zh`. Toggle button in nav switches states.

**Rules:**
- Never add text in only one language — always both spans
- English is a parallel brand-voice expression, not a direct translation
- Chinese: Noto Serif SC. English UI: Space Mono. Hero taglines: Special Elite.

---

## 10. Responsive

**Single breakpoint:** `768px`

| Element | Desktop | Mobile |
|---------|---------|--------|
| Hero photo | right 68%, fade left | bottom 45vh, fade upward |
| All content grids | `1fr 1fr` | `1fr` |
| Community photos | `repeat(4,1fr)` | `1fr 1fr` |
| Nav links | visible | hidden |
| Lang toggle | visible | visible |
| Section padding | see Section 5 | `80px 20px` |
| Decorative side text | visible | `display: none` |
| Cursor system | global flashlight SVG | `cursor: auto` |
| Touch targets | — | min `44px` height |

---

## 11. Do's and Don'ts

### Always
- `radial-gradient` backgrounds with a defined light source — never flat fills
- `.has-grain` on all dark sections
- `.section-fade-top` on dark sections (uses `::after` — no conflict with grain)
- Check bottom-edge color continuity between adjacent sections
- `clamp()` for all display type sizes
- Both `.zh` and `.en` for every text element
- Semi-transparent cards on gradient sections
- `2px` grid gap — it is intentional
- `var(--rust)` for section eyebrow labels
- Vary section height/padding for rhythm
- Bidirectional scroll reveal (remove `.visible` on exit)
- Single global cursor (flashlight SVG, 48×48, hotspot 45 11)

### Never
- Flat solid fills for any section background
- Pure `#FFFFFF` — always `var(--paper)` `#E8E4DC`
- Same surface token for adjacent sections
- System fonts (Arial, Inter, Roboto, system-ui)
- High-saturation colors — everything is weathered, slightly desaturated
- Neon glow, hard drop shadows, sci-fi effects
- `font-style: italic` on Special Elite or Noto Serif SC
- Body text below `14px` or below `opacity: 0.55` on dark backgrounds
- Opaque cards on gradient backgrounds
- Special Elite font outside of Hero taglines
- Purchase / cart / checkout UI of any kind
- Promotional copy: 超级好用、限时特惠、强烈推荐、大主角款

---

## 12. Adding New Sections or Pages

**Checklist:**

1. **Color** — Identify emotional role → pick surface token → confirm differs from neighbors → write `radial-gradient` with light source point → verify edge continuity
2. **Typography** — Use hierarchy table only, no new sizes. Respect minimums and opacity floors. Add both `.zh` and `.en`.
3. **Layout** — Choose padding from rhythm scale. `max-width: 1100px`. Test at `768px`.
4. **Texture** — Dark section → add `.has-grain` and `.section-fade-top`.
5. **Motion** — Add `.reveal` + stagger delays to content hierarchy. No new animation types.
6. **Cursor** — Global flashlight cursor is always active. No extra code needed per section.

---

## 13. TODO — Pending

- [ ] **Hero photo** — Embed `网站图.jpg` as base64 in `.hero-photo`. Brief already written.
- [ ] **More outdoor photos** — When available, consider additional photo sections (岩壁, 粉尘, 营地特写).
- [ ] **Product detail page** — When chalk bag sample photos arrive. Token: `--surface-dusk`.
- [ ] **XHS profile URL** — Confirm live URL for `dirtypoet218`
- [ ] **Contact email** — Confirm and replace `hello@dirtypoet.cn`
- [ ] **Community real photos** — Replace placeholder cards with #脏诗社 submissions. Refresh every 3–4 weeks.
- [ ] **Taobao** — When store launches, add as secondary CTA.
- [ ] **Season 02** — Define new seasonal accent color. Day-to-night arc stays, accent shifts.
- [ ] **Video integration** — Short looping clip as optional Hero background variant.

---

## 14. Technical Reference

**File:** Single HTML. All CSS, JS, base64 assets inline. External: Google Fonts only.

**Cursor SVG:** `手电光标.svg` — 32×32, hotspot 30 7, embedded as base64.

**Ibex PNG:** Embedded as base64. Do not remove.
Decorative use: `opacity: 0.12–0.20`, `mix-blend-mode: multiply`, max `120px` wide.

**Key classes and IDs:**
```
body.lang-zh / .lang-en     language state
.zh / .en                   bilingual text spans
.reveal                     scroll animation base
.reveal-delay-1/2/3         stagger timing
.reveal.visible             active animation state
.has-grain                  section noise texture
.section-fade-top           soft top-edge on dark sections (uses ::after)
.section-ambient            slow ambient mouse glow (manifesto, product)
#hero-light                 mouse-tracking orbs container (hero only)
.nav-active                 current section nav link state
```

**Git (PowerShell):**
```powershell
git add .; git commit -m "$(Get-Date -Format 'yyyy-MM-dd') 说明"; git push
```

**Stack:** Single HTML → GitHub → Vercel auto-deploy. No build step.

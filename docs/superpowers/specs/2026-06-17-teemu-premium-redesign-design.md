# Teemu PT — Premium Redesign Spec
**Date:** 2026-06-17  
**Scope:** Visual upgrade of `index.html` — no copy changes, no structural changes, no new sections  
**Direction:** B (cinematic, full-bleed, dramatic) with C elements (section contrast breaks, yellow discipline)

---

## Core Principle

Every section has exactly **one yellow thing**. Yellow is a pointer, not a decoration. Current state has yellow on eyebrows, headings, stats, borders, checkmarks, stars, and buttons simultaneously — this cheapens the palette. After this redesign, yellow appears once per section and stops.

---

## Yellow Discipline — Section Map

| Section | The one yellow thing | Everything else |
|---|---|---|
| Nav | Logo circle `T` | Links white, CTA button yellow fills (counts as nav's one accent) |
| Hero | Eyebrow pill badge | Name white, subtitle `rgba(255,255,255,0.7)` italic, stats white, CTA button white-on-black or outlined |
| Hook | `MIKSI TEEMU` eyebrow label | Heading white, pills outlined white, button outlined white |
| Services | `01`/`02`/`03`/`04` numbers | Top-border rule removed from individual items; one full-width yellow rule spans above grid |
| Process | Step numbers | Stat figures white, step titles white |
| Testimonials | 60px accent line below heading | Stars `rgba(255,255,255,0.5)` |
| Pricing | `SUOSITUIN` badge | Checkmarks white `✓`, featured card uses subtle border not yellow |
| FAQ | Nothing yellow | Eyebrow label removed, clean dark text |
| Bio (off-white) | Blockquote left-border `3px solid #E8FF00` | All else dark text on `#F2EFE9` |
| CTA | Submit button | All other text white |

---

## Typography

### Heading scale increases

| Location | Current | After |
|---|---|---|
| Hero name `.hero-name` | `clamp(80px, 15vw, 180px)` | `clamp(90px, 16vw, 200px)` |
| Section titles `.section-title` | `clamp(48px, 8vw, 100px)` | `clamp(56px, 9vw, 120px)` |
| Hook heading `.hook-heading` | `clamp(36px, 5vw, 64px)` | `clamp(40px, 6vw, 72px)` |
| Testimonial quote `.testi-quote` | `clamp(1.2rem, 2.8vw, 2rem)` | `clamp(1.4rem, 3vw, 2.2rem)` |
| Process/CTA stat figures | `clamp(2.5rem, 5vw, 4rem)` | `clamp(3rem, 6vw, 5rem)` |

### Hero subtitle
Change from yellow to: `color: rgba(255,255,255,0.7); font-style: italic`  
Remove yellow colour entirely from `.hero-sub`.

---

## Section-by-Section Changes

### Nav
- Logo circle: yellow — unchanged
- Nav links: add underline reveal on hover:
  ```css
  .nav-links a {
    position: relative;
  }
  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    width: 100%;
    height: 2px;
    background: #E8FF00;
    transform: scaleX(0);
    transform-origin: left;
    transition: transform .3s ease;
  }
  .nav-links a:hover::after { transform: scaleX(1); }
  ```
- Remove `.nav-links a:hover { color: #fff }` — the underline does the work
- CTA button stays yellow

### Hook (S2)
- Eyebrow label `MIKSI TEEMU`: stays yellow
- Heading `vahvempi` span: remove `color:var(--y)`, make it the same white as rest of heading — the eyebrow carries the yellow accent
- Pills: change from yellow-tinted to `border: 1px solid rgba(255,255,255,0.2); color: rgba(255,255,255,0.6)` — no yellow tint
- Button: keep `btn-out-y` → change to `btn-out` (white outlined)
- Photo column: `object-position: center 20%` for a taller editorial crop. **Note:** this is a Pexels gym/weights stock photo (ID 1552252), not Teemu's face — "tighter crop" means editorial framing of the equipment, not face-cropping. No special `object-position` magic needed; `center` is correct. If/when Teemu provides a real headshot, revisit this column.

### Services (S3)
- Remove `border-top: 2px solid var(--y)` from individual `.svc-item`
- Add a single `<div class="svc-rule">` spanning full width above the grid: `height:2px; background:var(--y); margin-bottom:0`
- `.svc-num` stays yellow
- `.svc-item:hover .svc-title`: change from `color:var(--y)` to `color:#fff` with `opacity:1` (title brightens, not yellows)
- Item hover: keep `translateX(6px)` slide

### Process (S4)
- `.step-num` stays yellow (opacity .15 — more ghost, less decoration). Change from `.35` to `.15`
- Stat figures `.pstat-n`: change from `color:var(--y)` to `color:#fff`
- Background attachment: see Parallax section

### Testimonials (S5)
- `.testi-accent` line stays yellow — this is the one accent
- `.testi-attr-stars`: change from `color:var(--y)` to `color:rgba(255,255,255,0.5)`
- Quote size increase per typography table above

### Pricing (S6)
- `.price-feats li::before` checkmarks: change from `color:var(--y)` to `color:rgba(255,255,255,0.8)`
- `.price-badge` stays yellow
- `.price-block.feat` border: change from `2px solid var(--y)` to `border-top: 3px solid rgba(255,255,255,0.2); border-left:none; border-right:none; border-bottom:none` — no yellow border on the card
- `.price-block:hover` border-color yellow: remove — hover lift stays (`translateY(-8px)`), glow remove

### FAQ (S7)
- Remove `.eyebrow` from the left column heading — no yellow label here
- `.faq-q:hover`: keep `color:var(--y)` — interactive yellow is fine on hover states

### Coach Bio (S8) — contrast break
- Section background: `background: #F2EFE9` (warm off-white)
- All text: `color: #0D0D0D`
- `.bio-name`, `.bio-subtitle`, `.bio-text`, `.bio-label`, `.cred-title`, `.cred-sub`, `.cred-tag`: dark
- `.cred-block`: `background: #1a1a1a; color: #fff` — dark credential blocks pop against light section
- `.bio-photo`: add `box-shadow: 0 40px 80px rgba(0,0,0,0.12)`
- `.bio-quote`: `border-left: 3px solid #E8FF00; color: #444` — this is the one yellow
- Bio CTA button: `background: #0D0D0D; color: #fff; border: 1px solid rgba(0,0,0,0.15)` — dark fill, white text, subtle border for definition against `#F2EFE9`
- `.eyebrow` colour in this section: override to `color: #888` (no yellow label here, use dim)

### CTA / Booking (S9)
- Photo background stays (3836861)
- Submit button stays yellow — the one accent
- Form inputs: `border: 1px solid rgba(255,255,255,0.12)` — more subtle than current
- `.eyebrow` label: remove or grey — no yellow above the heading here

---

## Parallax

### Desktop approach — JS translateY
Drop `background-attachment: fixed` from `#services`. Use a JS scroll listener instead (matches hero approach, works everywhere):

```js
window.addEventListener('scroll', () => {
  const offset = window.scrollY;
  document.querySelectorAll('.parallax-bg').forEach(el => {
    const section = el.closest('section');
    const rect = section.getBoundingClientRect();
    if (rect.top < window.innerHeight && rect.bottom > 0) {
      const progress = (window.innerHeight - rect.top) / (window.innerHeight + rect.height);
      el.style.transform = `translateY(${(progress - 0.5) * 60}px)`;
    }
  });
}, { passive: true });
```

Add class `parallax-bg` to the background image divs in `#process` and the `#cta` `.cta-bg`, and to the `#services::before` — or convert services to use a dedicated `<div class="parallax-bg">` element the same way process/CTA do.

### Mobile fallback
```css
@media (max-width: 768px) {
  #services, #process, #cta {
    background-attachment: scroll;
  }
  .parallax-bg {
    transform: none !important;
  }
}
```
This is a hard requirement — `background-attachment: fixed` on iOS Safari renders white. The JS listener does nothing harmful on mobile since `parallax-bg` transform is overridden, but explicitly clear it for safety.

---

## Hover Micro-interactions

These are the only place A-style refinement enters. Apply to hover states only — not as the primary visual language.

| Element | Hover behaviour |
|---|---|
| Nav links | Yellow underline grows from left (specified above) |
| Service items | `translateX(4px)` + title brightens (already exists, keep) |
| Pricing cards | `translateY(-6px)` lift — keep, remove yellow glow |
| Bio photo | `transform: scale(1.02)` on hover, `transition: transform .5s ease` |
| Hook photo | Already has `scale(1.03)` — keep |
| CTA button (all) | `background: #fff` or `background: #ccc` on hover — keep existing patterns |

---

## What Does NOT Change

- All Finnish copy — not one word changes
- Section order and structure
- Video hero implementation
- Nav links and CTA text
- All photo URLs
- Scroll animation system (`.anim`, `.anim-left`, `.anim-right`, `.anim-scale`, `.anim-line`)
- Counter animation
- Testimonial slider
- FAQ accordion
- Form and mailto action
- Mobile responsive breakpoints (only minor additions)
- Footer

---

## Spec Self-Review

- No TBDs or placeholders
- Hook photo note is explicit: stock gym photo, not a face crop
- `background-attachment: fixed` iOS bug is addressed with both a CSS fallback and JS override
- Yellow discipline table is unambiguous — one thing per section
- Bio section dark/light contrast: credential blocks intentionally stay dark (`#1a1a1a`) against the off-white to create internal contrast within the section
- No contradictions found between sections

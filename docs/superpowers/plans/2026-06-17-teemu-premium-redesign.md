# Teemu PT — Premium Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply B+C premium redesign to `index.html` — yellow discipline (one accent per section), enlarged typography, nav hover underlines, bio off-white contrast break, JS parallax on background sections, and hover micro-interactions.

**Architecture:** All changes are in a single `index.html` file with inline CSS and JS. No build step, no dependencies beyond the already-loaded Google Fonts. Tasks are ordered CSS-before-HTML within each concern to avoid mid-task broken states. Commit after each task.

**Tech Stack:** Vanilla HTML/CSS/JS. Single file at `C:\Users\Admin\teemu-pt\index.html`. Deployed via Vercel connected to `https://github.com/Altin990/teemu-pt` — `git push` triggers auto-deploy.

## Global Constraints

- Single file `index.html` — all CSS inline in `<style>`, all JS inline in `<script>`
- Zero copy changes — Finnish text is untouched in every task
- Zero structural changes — section order, nav, form, footer unchanged
- `background-attachment: fixed` must NOT appear in mobile CSS — iOS Safari renders it white
- Yellow (`#E8FF00` / `var(--y)`) appears exactly once per section after all tasks complete
- Never add new CDN links, libraries, or `<script src="">` tags

---

### Task 1: Typography scale + hero de-yellowing

**Files:**
- Modify: `index.html` — CSS section (search for `.hero-name`, `.section-title`, `.hook-heading`, `.testi-quote`, `.pstat-n`, `.hero-sub`, `.hero-eyebrow`)
- Modify: `index.html` — HTML hero section (search for `class="btn btn-y anim d4"`)

- [ ] **Step 1: Increase hero name size**

Find in `<style>`:
```css
font-size:clamp(80px,15vw,180px);font-weight:800;
line-height:.92;color:#fff;letter-spacing:-.04em;
margin-bottom:20px;
```
Replace with:
```css
font-size:clamp(90px,16vw,200px);font-weight:800;
line-height:.92;color:#fff;letter-spacing:-.04em;
margin-bottom:20px;
```

- [ ] **Step 2: Increase section title size**

Find:
```css
font-size:clamp(48px,8vw,100px);
font-weight:800;line-height:1;
letter-spacing:-.03em;margin-bottom:0;
```
Replace:
```css
font-size:clamp(56px,9vw,120px);
font-weight:800;line-height:1;
letter-spacing:-.03em;margin-bottom:0;
```

- [ ] **Step 3: Increase hook heading size**

Find:
```css
font-size:clamp(36px,5vw,64px);font-weight:800;
line-height:1.1;letter-spacing:-.03em;margin-bottom:28px;
```
Replace:
```css
font-size:clamp(40px,6vw,72px);font-weight:800;
line-height:1.1;letter-spacing:-.03em;margin-bottom:28px;
```

- [ ] **Step 4: Increase testimonial quote size**

Find (inside `.testi-quote`):
```css
font-size:clamp(1.2rem,2.8vw,2rem);
```
Replace:
```css
font-size:clamp(1.4rem,3vw,2.2rem);
```

- [ ] **Step 5: Increase process stat figure size**

Find (inside `.pstat-n`):
```css
font-size:clamp(2.5rem,5vw,4rem);font-weight:800;color:var(--y);display:block;line-height:1
```
Replace:
```css
font-size:clamp(3rem,6vw,5rem);font-weight:800;color:#fff;display:block;line-height:1
```
(Also removes yellow — stat figures become white here)

- [ ] **Step 6: Hero subtitle — remove yellow, add italic**

Find (the full `.hero-sub` rule):
```css
.hero-sub{
  color:var(--y);font-size:clamp(.85rem,1.4vw,1rem);
  font-weight:600;margin-bottom:32px;opacity:.9;max-width:500px;
  line-height:1.6;
}
```
Replace:
```css
.hero-sub{
  color:rgba(255,255,255,0.7);font-style:italic;
  font-size:clamp(.85rem,1.4vw,1rem);
  font-weight:600;margin-bottom:32px;opacity:.9;max-width:500px;
  line-height:1.6;
}
```

- [ ] **Step 7: Hero eyebrow — remove yellow**

Find (the full `.hero-eyebrow` rule):
```css
.hero-eyebrow{
  color:var(--y);font-size:clamp(.85rem,1.5vw,1.1rem);
  font-weight:700;letter-spacing:.18em;text-transform:uppercase;
  margin-bottom:12px;
}
```
Replace:
```css
.hero-eyebrow{
  color:rgba(255,255,255,0.75);font-size:clamp(.85rem,1.5vw,1.1rem);
  font-weight:700;letter-spacing:.18em;text-transform:uppercase;
  margin-bottom:12px;
}
```

- [ ] **Step 8: Hero CTA button — swap yellow fill for outlined**

Find in the HTML hero section:
```html
<a href="#cta" class="btn btn-y anim d4">Varaa ilmainen tunti →</a>
```
Replace:
```html
<a href="#cta" class="btn btn-out anim d4">Varaa ilmainen tunti →</a>
```
(The hero's single yellow is now the `.hero-badge` pill only. The CTA becomes a clean outlined white button.)

- [ ] **Step 9: Verify in browser**

Open `index.html`. Check:
- Hero name is visibly larger
- "Harjoittele kuin urheilija." is white (not yellow)
- Subtitle "Ammattitutkinnon suorittanut..." is dim white italic
- "Varaa ilmainen tunti →" CTA has white outline, not yellow fill
- Only the "PERSONAL TRAINER · HELSINKI" pill badge has yellow
- Section headings (Services, Process, etc.) appear larger on scroll

- [ ] **Step 10: Commit**
```bash
git add index.html
git commit -m "style: typography scale up, hero de-yellowed — badge is only yellow in hero"
```

---

### Task 2: Nav hover underline

**Files:**
- Modify: `index.html` — CSS only (`.nav-links a` rules)

- [ ] **Step 1: Add underline reveal to nav links**

Find the exact two rules:
```css
.nav-links a{color:var(--dim);font-size:.88rem;font-weight:500;transition:color .2s}
.nav-links a:hover{color:#fff}
```
Replace with:
```css
.nav-links a{
  position:relative;
  color:var(--dim);font-size:.88rem;font-weight:500;transition:color .2s;
}
.nav-links a::after{
  content:'';
  position:absolute;
  bottom:-4px;left:0;
  width:100%;height:2px;
  background:var(--y);
  transform:scaleX(0);
  transform-origin:left;
  transition:transform .3s ease;
}
.nav-links a:hover::after{transform:scaleX(1)}
```
(The `hover { color:#fff }` rule is intentionally removed — the underline is the hover signal.)

- [ ] **Step 2: Verify in browser**

Open `index.html`. Scroll down until nav goes opaque. Hover over "Palvelut", "Prosessi", etc. Check:
- A yellow 2px line grows from left to right on hover
- Links stay at their default dim colour (not white) — the underline alone provides the hover signal
- The T logo circle and "Aloita nyt" button are unaffected

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "style: nav links — yellow underline reveal on hover, remove colour change"
```

---

### Task 3: Yellow discipline — hook, services, testimonials, pricing

This task removes yellow from 4 sections via CSS changes and 3 small HTML edits.

**Files:**
- Modify: `index.html` — CSS (`.pill`, `.svc-item`, `.svc-item:hover .svc-title`, `.testi-attr-stars`, `.price-feats li::before`, `.price-block.feat`, `.price-block.feat:hover`, `.price-block:hover`)
- Modify: `index.html` — HTML (hook heading span, hook button, svc-rule div)

- [ ] **Step 1: Hook heading — remove yellow on "vahvempi"**

Find in HTML:
```html
Tapaa <span style="color:var(--y)">vahvempi</span><br>versio itsestäsi.
```
Replace (remove the span entirely):
```html
Tapaa vahvempi<br>versio itsestäsi.
```

- [ ] **Step 2: Hook button — swap yellow outlined for white outlined**

Find:
```html
<a href="#cta" class="btn btn-out-y">Aloita matka →</a>
```
Replace:
```html
<a href="#cta" class="btn btn-out">Aloita matka →</a>
```

- [ ] **Step 3: Hook pills — more subtle**

Find (the full `.pill` rule):
```css
.pill{
  background:rgba(255,255,255,.05);
  border:1px solid rgba(255,255,255,.12);
  color:rgba(255,255,255,.7);
  font-size:.78rem;font-weight:600;
  padding:8px 20px;border-radius:50px;letter-spacing:.04em;
}
```
Replace:
```css
.pill{
  background:rgba(255,255,255,.04);
  border:1px solid rgba(255,255,255,.2);
  color:rgba(255,255,255,.6);
  font-size:.78rem;font-weight:600;
  padding:8px 20px;border-radius:50px;letter-spacing:.04em;
}
```

- [ ] **Step 4: Services — remove yellow top-border from items, add single rule div**

Find (the `.svc-item` rule):
```css
.svc-item{
  padding:48px 40px;
  border-top:2px solid var(--y);
  transition:background .25s;
}
```
Replace:
```css
.svc-item{
  padding:48px 40px;
  border-top:none;
  transition:background .25s;
}
.svc-rule{height:2px;background:var(--y);margin-bottom:0}
```

- [ ] **Step 5: Add svc-rule div in HTML**

Find:
```html
    <div class="svc-grid">
```
Replace:
```html
    <div class="svc-rule anim"></div>
    <div class="svc-grid">
```

- [ ] **Step 6: Services — title hover goes bright white, not yellow**

Find:
```css
.svc-item:hover .svc-title{color:var(--y)}
```
Replace:
```css
.svc-item:hover .svc-title{color:#fff}
```

- [ ] **Step 7: Testimonials — dim the stars**

Find:
```css
.testi-attr-stars{color:var(--y);letter-spacing:2px;font-style:normal}
```
Replace:
```css
.testi-attr-stars{color:rgba(255,255,255,0.45);letter-spacing:2px;font-style:normal}
```

- [ ] **Step 8: Pricing — checkmarks to white**

Find (inside `.price-feats li::before` rule):
```css
.price-feats li::before{content:'✓';color:var(--y);font-weight:800;font-size:.85rem;flex-shrink:0}
```
Replace:
```css
.price-feats li::before{content:'✓';color:rgba(255,255,255,0.75);font-weight:800;font-size:.85rem;flex-shrink:0}
```

- [ ] **Step 9: Pricing featured card — remove yellow bg tint and add subtle top accent**

Find:
```css
.price-block.feat{background:#0d0d00}
.price-block.feat:hover{background:#111100}
```
Replace:
```css
.price-block.feat{background:#111;border-top:3px solid rgba(255,255,255,0.18)}
.price-block.feat:hover{background:#161616}
```

- [ ] **Step 10: Pricing hover — remove yellow glow**

Find:
```css
.price-block:hover{
  transform:translateY(-8px);
  border-color:var(--y);
  box-shadow:0 0 30px rgba(232,255,0,0.15);
}
```
Replace:
```css
.price-block:hover{
  transform:translateY(-8px);
}
```

- [ ] **Step 11: Verify in browser**

Check:
- Hook: "vahvempi" is plain white (same as surrounding text). Both buttons are white-outlined. Pills are subtly white-bordered.
- Services: Individual items have no yellow top border. One yellow line spans the full width above the 2×2 grid. Numbers still yellow. Title on hover goes bright white (not yellow).
- Testimonials: Stars are dim white (≈50% opacity), not yellow.
- Pricing: Checkmarks are white. Featured card has a subtle white top accent, not a yellow border. No yellow glow on hover.

- [ ] **Step 12: Commit**
```bash
git add index.html
git commit -m "style: yellow discipline — hook, services, testimonials, pricing de-yellowed"
```

---

### Task 4: FAQ and CTA eyebrow de-yellowing

**Files:**
- Modify: `index.html` — HTML (FAQ left column) and CSS (CTA eyebrow override)

- [ ] **Step 1: Remove yellow eyebrow label from FAQ**

Find in HTML (inside the FAQ section left column):
```html
    <div class="anim-left">
      <span class="eyebrow">Kysymyksiä</span>
      <h2 class="faq-left-title">Kaikki mitä sinun täytyy tietää</h2>
```
Replace (delete the eyebrow span):
```html
    <div class="anim-left">
      <h2 class="faq-left-title">Kaikki mitä sinun täytyy tietää</h2>
```

- [ ] **Step 2: Dim the CTA section eyebrow via CSS**

Find (anywhere inside the `<style>` block, add after the `.eyebrow` rule):
```css
.eyebrow{
  display:inline-block;color:var(--y);
  font-size:.72rem;font-weight:700;letter-spacing:.22em;
  text-transform:uppercase;margin-bottom:1.2rem;
}
```
Append immediately after that rule:
```css
#cta .eyebrow{color:rgba(255,255,255,0.35)}
```
(Do not modify the base `.eyebrow` rule — only override inside `#cta`.)

- [ ] **Step 3: Verify in browser**

Check:
- FAQ left column: no yellow "Kysymyksiä" label — heading starts immediately with "Kaikki mitä sinun täytyy tietää"
- CTA section: "Ota yhteyttä" label is dim white (~35% opacity), not yellow
- FAQ question hover still shows yellow (`.faq-q:hover{color:var(--y)}` unchanged — interactive yellow on hover is fine)

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "style: FAQ eyebrow removed, CTA eyebrow dimmed — no section-level yellow in FAQ or CTA header"
```

---

### Task 5: Bio section — off-white contrast break + photo hover

**Files:**
- Modify: `index.html` — CSS (`#bio` and all child element rules)
- Modify: `index.html` — HTML (bio CTA button class)

- [ ] **Step 1: Flip bio section background to warm off-white**

Find (the full `#bio` rule):
```css
#bio{padding:var(--sec-pad);background:#060606}
```
Replace:
```css
#bio{padding:var(--sec-pad);background:#F2EFE9;color:#0D0D0D}
```

- [ ] **Step 2: Override eyebrow colour inside bio**

Add after the `#cta .eyebrow` rule you added in Task 4:
```css
#bio .eyebrow{color:#888}
```

- [ ] **Step 3: Dark text for bio content elements**

Find the `.bio-label` rule and replace:
```css
.bio-label{font-size:.7rem;font-weight:700;color:var(--dim);letter-spacing:.2em;text-transform:uppercase;margin-bottom:8px}
```
Replace:
```css
.bio-label{font-size:.7rem;font-weight:700;color:#888;letter-spacing:.2em;text-transform:uppercase;margin-bottom:8px}
```

Find `.bio-name`:
```css
.bio-name{
  font-size:clamp(2.5rem,4vw,3.5rem);font-weight:800;
  color:#fff;letter-spacing:-.03em;line-height:1;margin-bottom:8px;
}
```
Replace:
```css
.bio-name{
  font-size:clamp(2.5rem,4vw,3.5rem);font-weight:800;
  color:#0D0D0D;letter-spacing:-.03em;line-height:1;margin-bottom:8px;
}
```

Find `.bio-subtitle`:
```css
.bio-subtitle{font-size:.95rem;color:var(--dim);margin-bottom:24px;font-weight:500}
```
Replace:
```css
.bio-subtitle{font-size:.95rem;color:#666;margin-bottom:24px;font-weight:500}
```

Find `.bio-text`:
```css
.bio-text{font-size:.98rem;color:rgba(255,255,255,.7);line-height:1.85;margin-bottom:28px}
```
Replace:
```css
.bio-text{font-size:.98rem;color:#333;line-height:1.85;margin-bottom:28px}
```

- [ ] **Step 4: Dark credential blocks that pop against off-white**

Find `.cred-block`:
```css
.cred-block{padding:28px 24px;background:#0f0f0f;border-top:2px solid var(--y)}
```
Replace:
```css
.cred-block{padding:28px 24px;background:#1a1a1a;border-top:2px solid rgba(255,255,255,0.06)}
```

Find `.cred-tag`:
```css
.cred-tag{font-size:.65rem;font-weight:800;color:var(--y);letter-spacing:.18em;text-transform:uppercase;margin-bottom:8px}
```
Replace:
```css
.cred-tag{font-size:.65rem;font-weight:800;color:rgba(255,255,255,0.45);letter-spacing:.18em;text-transform:uppercase;margin-bottom:8px}
```

Find `.cred-title`:
```css
.cred-title{font-size:1.05rem;font-weight:700;color:#fff;margin-bottom:4px}
```
This is already white — no change needed.

Find `.cred-sub`:
```css
.cred-sub{font-size:.82rem;color:var(--dim)}
```
Replace:
```css
.cred-sub{font-size:.82rem;color:rgba(255,255,255,0.45)}
```

- [ ] **Step 5: Bio photo — shadow and hover scale**

Find `.bio-photo`:
```css
.bio-photo{height:560px;overflow:hidden;border-radius:12px;position:relative}
```
Replace:
```css
.bio-photo{height:560px;overflow:hidden;border-radius:12px;position:relative;box-shadow:0 40px 80px rgba(0,0,0,0.12)}
```

Find `.bio-photo img`:
```css
.bio-photo img{width:100%;height:100%;object-fit:cover;object-position:center top;display:block}
```
Replace:
```css
.bio-photo img{width:100%;height:100%;object-fit:cover;object-position:center top;display:block;transition:transform .5s ease}
.bio-photo:hover img{transform:scale(1.02)}
```

- [ ] **Step 6: Bio blockquote — yellow left border (the one yellow in this section)**

Find `.bio-quote`:
```css
.bio-quote{
  font-style:italic;color:var(--y);font-size:1rem;
  line-height:1.7;border-left:3px solid var(--y);
  padding-left:16px;margin-bottom:32px;
}
```
Replace:
```css
.bio-quote{
  font-style:italic;color:#444;font-size:1rem;
  line-height:1.7;border-left:3px solid var(--y);
  padding-left:16px;margin-bottom:32px;
}
```
(Border stays yellow — this is the one yellow. Quote text changes from yellow to dark grey.)

- [ ] **Step 7: Add dark bio CTA button style**

Add after the `.btn-out-y:hover` rule in CSS:
```css
.btn-dark{background:#0D0D0D;color:#fff;border:1px solid rgba(0,0,0,0.15)}
.btn-dark:hover{background:#2a2a2a;transform:translateY(-2px)}
```

- [ ] **Step 8: Swap bio CTA button class in HTML**

Find in the bio section HTML:
```html
<a href="#cta" class="btn btn-y">Varaa ilmainen tunti →</a>
```
Replace:
```html
<a href="#cta" class="btn btn-dark">Varaa ilmainen tunti →</a>
```

- [ ] **Step 9: Verify in browser**

Scroll to the coach bio section. Check:
- Section background is warm off-white (`#F2EFE9`) — strong contrast break from surrounding dark sections
- "Teemu" name and body text are dark `#0D0D0D`
- Three credential blocks are dark `#1a1a1a` — they visually pop against the off-white background
- Photo has a subtle shadow and scales gently on hover
- The blockquote has a yellow left border, grey quote text — yellow appears exactly once in this section
- "Varaa ilmainen tunti →" button is dark fill with white text; hover lifts slightly

- [ ] **Step 10: Commit**
```bash
git add index.html
git commit -m "style: bio section — off-white contrast break, dark credential blocks, yellow quote border only"
```

---

### Task 6: Parallax refactor

Replaces `background-attachment:fixed` (broken on iOS Safari) with a JS scroll-driven `translateY` approach on elements with class `parallax-bg`. Applies to three sections: services, process, CTA.

**Files:**
- Modify: `index.html` — CSS (`#services`, new `.svc-bg` and `.parallax-bg` rules, mobile override)
- Modify: `index.html` — HTML (add `.svc-bg.parallax-bg` div in services; add `parallax-bg` class to `.process-bg` and `.cta-bg`)
- Modify: `index.html` — JS (add parallax scroll listener)

- [ ] **Step 1: Strip background-image from #services CSS, add overflow:hidden**

Find the `#services` CSS rule block:
```css
#services{
  position:relative;
  background-image:url("https://images.pexels.com/photos/1552252/pexels-photo-1552252.jpeg?auto=compress&cs=tinysrgb&w=1920&h=1080&dpr=1");
  background-size:cover;
  background-position:center;
  background-attachment:fixed;
}
```
Replace:
```css
#services{
  position:relative;
  overflow:hidden;
}
```

- [ ] **Step 2: Add .svc-bg styles and .parallax-bg base**

Find (the `#services::before` rule):
```css
#services::before{
  content:"";
  position:absolute;
  inset:0;
  background:rgba(0,0,0,0.82);
  z-index:0;
}
```
Replace (keep `::before` for overlay, add new rules below it):
```css
#services::before{
  content:"";
  position:absolute;
  inset:0;
  background:rgba(0,0,0,0.82);
  z-index:0;
}
.svc-bg{
  position:absolute;inset:0;
  background:url("https://images.pexels.com/photos/1552252/pexels-photo-1552252.jpeg?auto=compress&cs=tinysrgb&w=1920&h=1080&dpr=1") center/cover no-repeat;
  z-index:-1;
}
.parallax-bg{will-change:transform}
```

- [ ] **Step 3: Add mobile parallax override CSS**

Find the `@media(max-width:768px)` block. Inside it, add at the end (before the closing `}`):
```css
  .parallax-bg{transform:none !important}
```

- [ ] **Step 4: Add .svc-bg div in services HTML**

Find (the opening of the services section):
```html
<section id="services">
  <div>
```
Replace:
```html
<section id="services">
  <div class="svc-bg parallax-bg"></div>
  <div>
```

- [ ] **Step 5: Add parallax-bg class to process background div**

Find:
```html
  <div class="process-bg"></div>
```
Replace:
```html
  <div class="process-bg parallax-bg"></div>
```

- [ ] **Step 6: Add parallax-bg class to CTA background div**

Find:
```html
  <div class="cta-bg"></div>
```
Replace:
```html
  <div class="cta-bg parallax-bg"></div>
```

- [ ] **Step 7: Add JS parallax scroll listener**

Find in the `<script>` block the existing hero parallax code:
```js
/* ── Hero video parallax ── */
const heroVid=document.querySelector('#hero video');
window.addEventListener('scroll',()=>{
  if(scrollY<window.innerHeight){
    heroVid.style.transform=`translateY(${scrollY*.4}px)`;
  }
},{passive:true});
```
Replace with (keeps hero parallax, adds section parallax below it):
```js
/* ── Hero video parallax ── */
const heroVid=document.querySelector('#hero video');
window.addEventListener('scroll',()=>{
  if(scrollY<window.innerHeight){
    heroVid.style.transform=`translateY(${scrollY*.4}px)`;
  }
},{passive:true});

/* ── Section background parallax ── */
const parallaxBgs=document.querySelectorAll('.parallax-bg');
function updateParallax(){
  if(window.innerWidth<=768)return;
  parallaxBgs.forEach(el=>{
    const section=el.closest('section');
    if(!section)return;
    const rect=section.getBoundingClientRect();
    if(rect.top<window.innerHeight&&rect.bottom>0){
      const progress=(window.innerHeight-rect.top)/(window.innerHeight+rect.height);
      el.style.transform=`translateY(${(progress-.5)*60}px)`;
    }
  });
}
window.addEventListener('scroll',updateParallax,{passive:true});
```

- [ ] **Step 8: Verify in browser — desktop**

Open `index.html` in a desktop browser. Slowly scroll through. Check:
- Services section shows the gym weights photo with dark overlay — photo moves at a slightly different rate than the page (parallax effect)
- Process section photo (gym training) moves with parallax
- CTA section photo (ice rink) moves with parallax
- None of the section backgrounds go white or disappear

- [ ] **Step 9: Verify on mobile (or dev tools mobile emulation)**

In Chrome DevTools, toggle a mobile device (iPhone 14). Check:
- All three photo backgrounds are visible and static (no parallax movement — `transform:none !important` in mobile CSS)
- No white backgrounds appear on services, process, or CTA sections

- [ ] **Step 10: Commit**
```bash
git add index.html
git commit -m "style: parallax — JS translateY on .parallax-bg, replaces broken background-attachment:fixed"
```

---

### Task 7: Final push to GitHub (triggers Vercel deploy)

**Files:**
- `C:\Users\Admin\teemu-pt\index.html` — already committed in Tasks 1–6

- [ ] **Step 1: Confirm all tasks committed**
```bash
git log --oneline -7
```
Expected output (6 commits from this plan, newest first):
```
<hash> style: parallax — JS translateY on .parallax-bg, replaces broken background-attachment:fixed
<hash> style: bio section — off-white contrast break, dark credential blocks, yellow quote border only
<hash> style: FAQ eyebrow removed, CTA eyebrow dimmed — no section-level yellow in FAQ or CTA header
<hash> style: yellow discipline — hook, services, testimonials, pricing de-yellowed
<hash> style: nav links — yellow underline reveal on hover, remove colour change
<hash> style: typography scale up, hero de-yellowed — badge is only yellow in hero
```

- [ ] **Step 2: Push to GitHub**
```bash
git push origin master
```
Expected: `master -> master` with no errors.

- [ ] **Step 3: Verify Vercel deployment**

Open `https://vercel.com/altins-projects-2b1dd4c1/teemu-pt` and wait for deployment to reach "Ready" status (typically 30–60 seconds after push).

- [ ] **Step 4: Final live check**

Open `https://teemu-pt.vercel.app` in browser and verify the complete yellow discipline:
- Hero: only the "PERSONAL TRAINER · HELSINKI" pill badge is yellow
- Hook: only the "MIKSI TEEMU" eyebrow is yellow
- Services: only the 01/02/03/04 numbers and the single rule line above the grid are yellow
- Process: only the ghost step numbers are yellow
- Testimonials: only the 60px accent line below the heading is yellow
- Pricing: only the "SUOSITUIN" badge is yellow
- FAQ: nothing yellow (hover on questions shows yellow — that's fine, it's interactive)
- Bio: only the blockquote left border is yellow
- CTA: only the submit button is yellow

---

## Self-Review

**Spec coverage:**
- Typography scale ✓ (Task 1)
- Hero subtitle italic + de-yellowed ✓ (Task 1)
- Hero CTA button outlined ✓ (Task 1)
- Hero eyebrow de-yellowed ✓ (Task 1)
- Nav underline hover ✓ (Task 2)
- Hook heading span removed ✓ (Task 3)
- Hook button → btn-out ✓ (Task 3)
- Hook pills updated ✓ (Task 3)
- svc-rule div + no item border-top ✓ (Task 3)
- Service title hover → white ✓ (Task 3)
- Testimonial stars dimmed ✓ (Task 3)
- Pricing checkmarks white ✓ (Task 3)
- Pricing featured card border changed ✓ (Task 3)
- Pricing hover glow removed ✓ (Task 3)
- FAQ eyebrow removed ✓ (Task 4)
- CTA eyebrow dimmed ✓ (Task 4)
- Bio background #F2EFE9 ✓ (Task 5)
- Bio text dark ✓ (Task 5)
- Credential blocks dark on light bg ✓ (Task 5)
- Bio photo shadow + hover scale ✓ (Task 5)
- Bio quote border yellow, text dark ✓ (Task 5)
- Bio CTA btn-dark ✓ (Task 5)
- parallax-bg JS approach, no background-attachment:fixed ✓ (Task 6)
- Mobile parallax override ✓ (Task 6)
- Push + deploy ✓ (Task 7)
- Process stat figures white → covered in Task 1 Step 5 ✓
- Step-num opacity .15 → NOT yet covered — add below

**Gap found:** `.step-num` opacity was spec'd to change from `.35` to `.15`. It is not in any task above.

**Fix:** Add to Task 3 after Step 6:

- [ ] **Task 3, Step 6b: Process step numbers — reduce opacity**

Find in CSS:
```css
.step-num{font-size:2.2rem;font-weight:800;color:var(--y);opacity:.35;line-height:1;margin-bottom:20px;display:block;}
```
Replace:
```css
.step-num{font-size:2.2rem;font-weight:800;color:var(--y);opacity:.15;line-height:1;margin-bottom:20px;display:block;}
```

**Placeholder scan:** No TBDs, TODOs, or vague steps. All steps contain exact find/replace strings.

**Type consistency:** No function signatures across tasks — this is CSS/HTML, no cross-task type dependencies.

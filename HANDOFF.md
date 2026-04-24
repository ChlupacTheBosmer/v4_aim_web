# V4·AIM Landing — Screen-by-Screen Handoff

This document walks through the page top-to-bottom, documenting every
section's desktop layout, mobile override, and the design decisions behind
them. Read it alongside `design/index.html` — line numbers in the HTML are
referenced where useful.

---

## Global: shell, nav, container

### Container

- Single `.wrap` container: `max-width: 1200px`, horizontal `padding: 24px`
  desktop / `16px` mobile, centered with `margin: 0 auto`.
- Section pattern: `<section class="chap" id="…">` with an optional `.band`
  modifier that applies the alternate surface color (`--surface-2`) for
  visual rhythm. Alternating bands = About (plain) → Conference (band) →
  Open Call (plain) → Partners (band) → Contact (plain) → Funding (dark).
- Section vertical padding: `128px 0` desktop, `56px 0` mobile.
- Section header (`.chap-h`): 2-column grid on desktop
  (`grid-template-columns: 160px 1fr` — eyebrow left, H2 right), collapses
  to single column on mobile. **H2 `max-width: 44ch`** — do not shrink this;
  the previous 20ch value caused titles like "Three days. Four keynotes.
  Sixteen panelists." to wrap into tall stacks on desktop.

### Top nav (desktop)

```
[V4·AIM mark]  Visegrad / Conference / AI Methods      About / Conference / Open call / Partners / Contact    Funded · 2026
```

- Fixed at top with subtle bottom border.
- Left: 16×16 crosshair mark (SVG) + wordmark `V4·AIM · Visegrad · AI
  Methods` in JetBrains Mono.
- Right: horizontal link list + status pill `Funded · 2026` with a red dot
  prefix (`::before`).

### Top nav (mobile, ≤720px)

- Links + status pill hide (`display: none`).
- Hamburger button `.nav-burger` appears (2-line icon, animates to an ✕
  when open).
- Tapping opens `.mobile-menu` — a full-viewport overlay with large
  numbered list (`01 About`, `02 Conference`, …) in JetBrains Mono, each
  row ~44 px tall. Closes on link click or ✕.
- `padding-top: env(safe-area-inset-top)` on the sticky nav so it drops
  below the iOS notch / status bar. **Keep this.**

---

## 1. Hero

The hero is a **full-bleed dark slab** (`background: var(--v4-onyx)`
which is `#0A0A0A`) with bone/white type. It's the only dark section
above the fold and sets the tone.

### Desktop layout

```
                          [animated crosshair mark]

                 A cross-disciplinary Visegrad
                 conference on AI methods.
                 ─────────────────────────
                 (red accent on "conference on AI methods")

                 V4·AIM convenes researchers from the Czech
                 Republic, Slovakia, Poland, and Hungary…

                 [Notify me when the call opens →]  [Read the brief ↓]
```

- Everything center-aligned on desktop (`.hero-grid` uses `justify-items:
  center`).
- H1 font: JetBrains Mono 500, `clamp(30px, 4.2vw, 56px)`, `line-height:
  1.1`, `letter-spacing: -0.015em`, `max-width: 22ch`, `text-wrap:
  balance`.
- Lede: serif italic (var(--v4-font-serif)), 15–18 px responsive, color
  `#C9CAD0` (dimmer than the headline for hierarchy).
- Two versions of the lede in the DOM:
  - `.lede.desktop` — long version, shown by default
  - `.lede.mobile` — short version, shown only below 720 px (different
    copy, NOT a CSS truncation).
- `.hero h1 .hl { color: var(--v4-accent); }` — the red wraps the phrase
  "conference on AI methods". There is **no SVG underline** — previous
  versions used one under "AIM"; it's been removed.
- Meta row above the hero: three cells (Event / Dates / Venue) in
  monospace with 10-px uppercase keys + 13-px values, separated by a
  1-px rule.
- **Animated ring-pulse** on the crosshair mark: three concentric red
  circles that scale + fade over 3.6 s with staggered delays (0 s /
  1.2 s / 2.4 s). `@keyframes ringpulse`. Disabled under
  `prefers-reduced-motion: reduce`. Keep this — it's the one piece of
  brand motion on the page.

### Mobile overrides

- `.hero .hero-grid { justify-items: start; }` — flips everything back
  to left-aligned.
- H1, lede, CTA group all `text-align: left; margin-left: 0`.
- `.hero .mark-stage { justify-content: flex-start; }` — crosshair moves
  to the top-left.
- `.hero .mark-stage svg { width: 56px; }` (from 72 px).
- H1 font becomes `clamp(40px, 10vw, 52px)`; `line-height: 1.02` for a
  tighter block.
- The animated ring-pulse stays on mobile.

### CTAs

Two buttons:
- **Primary**: `.btn` — black pill (`background: var(--v4-ink)`, white
  text), hovers red (`background: var(--v4-accent)`).
- **Secondary**: `.btn.ghost` — transparent with 1 px bone-colored
  border, bone text on the dark hero.
- Both have an arrow span (`→` / `↓`) that translates `3px` on hover.

---

## 2. About

`#about`, plain background.

- Eyebrow: `01 / About`
- H2: **"A method-centric conference for Central European research."**
  (Note: "conference", not "network" — do not revert.)
- Body: single column (`.about-grid.single`), max-width 900 px.
- Three fact blocks in a `<dl class="fact-blocks">` — each row is a
  2-column grid (180 px label + 1fr body) with a 0.5 px hairline between
  rows. Collapses to stacked single-column on ≤720 px.

  | Label          | Body                                                                                                |
  |----------------|-----------------------------------------------------------------------------------------------------|
  | 01 What it is  | A cross-disciplinary, cross-border knowledge conference grounded in the Visegrad region.            |
  | 02 How it runs | In-person conference on two method families: **computer vision** and **LLMs**, followed by a shared digital repository and online working groups. |
  | 03 Who can join| Researchers, doctoral candidates, and advanced Master's students from V4 universities. Travel and lodging covered for selected participants from partner countries. |

- The previous `<aside>` "Objectives" block and the long italic lede
  paragraph have been **intentionally removed**. Do not re-introduce
  them.

---

## 3. Conference

`#conference`, **band** background (`--surface-2`). Title: "Three days.
Four keynotes. Sixteen panelists."

### Desktop: two-column grid

- Left column (`.dateplate`): dark block (onyx bg, bone text),
  `border-radius: 8px`, contains:
  - Big `23–25` in JetBrains Mono (tabular-nums), 72–96 px
  - `APRIL 2027` eyebrow beneath
  - Meta rows: Venue, Region, Format, Language
- Right column (`.program`): 4-row list of `.prog-item` cards. Each card
  is a 3-column grid: `56px 1fr auto` (idx / body / count). Cards have
  0.5 px borders, 8 px radius, hover lifts border + swaps bg to
  `var(--surface)`.

  | idx | title + body                              | count         |
  |-----|-------------------------------------------|---------------|
  | 01  | Keynote lectures (intro copy)             | 4 SPEAKERS    |
  | 02  | Panel discussions (intro copy)            | 16 PANELISTS  |
  | 03  | Method workshops (intro copy)             | 2 DAYS        |
  | 04  | Network formation (intro copy)            | 40+ ATTENDEES |

### Mobile: horizontal scroll-snap carousel

- `.program { display: flex; overflow-x: auto; scroll-snap-type: x
  mandatory; gap: 16px; padding-right: 16px; }`
- Cards bleed to the edge (no wrap padding on the right side).
- Each `.prog-item` becomes a **3-row grid**:
  - Row 1: idx (col 1) + title (col 2)
  - Row 2: description (col 2)
  - Row 3: count label, **full-width with a top hairline** — this was
    the fix for the "4 SPEAKERS overlaps the heading" bug. **Don't
    revert to the 2-row layout.**
- `::-webkit-scrollbar { display: none; }` hides the native scrollbar.
- Card width `flex: 0 0 78%`, `min-width: 260px`.

### No Participation note

Previously a red-bordered `.conf-note` appeared below the program with
participation eligibility copy. It has been **removed** (still in the
DOM with `hidden` attribute for easy restoration if the coordinator
changes their mind — feel free to delete entirely in the port).

---

## 4. Open Call

`#open-call`, plain background. Title: **"Call opens late 2026."** —
intentionally short so it doesn't wrap on desktop.

- Two-column grid desktop (`.call`), stacked mobile.
- Left (`.status-block`): small `Status · TBD` tag, h3 "Registration not
  yet open", paragraph explaining dates are being finalized, then the
  **waitlist form**.
- Right: ordered list of what applicants will submit (CV, motivation
  statement, institutional affiliation).

### Waitlist form — the one thing that needs backend

```html
<form class="mail-wait" id="waitlist" novalidate>
  <label for="email-wait" style="position:absolute;left:-9999px;">Email</label>
  <input id="email-wait" type="email" placeholder="name@institution.edu" required />
  <button type="submit">Notify me</button>
</form>
```

Current JS (bottom of `index.html`, `// Waitlist faux-submit` comment):
client-side regex validation, swaps button to "Added ✓", adds
`.ok` class, re-enables after 4 s. **Replace with a real POST.** See
`README.md` § *What needs a backend* for endpoint contract.

Mobile: input + button stack vertically, 52 px min-height each, 16 px
font on the input to prevent iOS zoom-on-focus.

---

## 5. Partners

`#partners`, **band** background. Title: "Six institutions across four
countries."

### Desktop: 3×2 grid of `.partner` cards

Each card: eyebrow `01`–`06`, institution name, department, country,
external-link arrow in the top-right. Entire card is a link (`<a
class="partner">`). Hover: red arrow + 3 px translate up-right.

Six partners:
1. Charles University (Faculty of Science) · Czechia
2. Czech Technical University · Czechia
3. Nicolaus Copernicus University Toruń · Poland
4. University of Wrocław (Philological School of Higher Education) · Poland
5. Comenius University Bratislava · Slovakia
6. Eötvös Loránd University · Hungary

### Mobile: horizontal scroll-snap carousel

Same pattern as the Conference program — flex row, scroll-snap, cards
~78% viewport width.

---

## 6. Contact

`#contact`, plain background. Title: "Questions about the conference or
project."

- Two-column grid: `.contact-card` (left) + `.contact-aux` (right).
- `.contact-card` uses `background: var(--surface)` (= `#F3F4F6`, cool
  grey). Card padding 40 px desktop / 24 px mobile, 8 px radius.
  - Name: **Mgr. Peter Kutsos**
  - Role: Project coordinator · V4·AIM
  - `<dl class="dl">` list of Email / Affiliation / Location
- `.contact-aux`: three blocks on the right — Lead organization,
  Technical contact (Mgr. et Mgr. Petr Chlup — chlupp@natur.cuni.cz),
  IVF contact (Zuzana Pavlíková — pavlikova@visegradfund.org).

Links in the contact card get a red underline (`border-bottom: 1px
solid var(--v4-accent)`) and hover red.

---

## 7. Funding

`#funding`, **dark** (`background: var(--v4-onyx)`, bone text). Full-
bleed black slab mirroring the hero.

- Left: IVF logotype (white wrapper card with dark logo PNG) + caption.
- Right: copy block — project title, funding statement, project ID
  `22610317`, grant amount, period, role line-items.

**IVF logotype requirement:** the image `brand/ivf-logotype-black.png`
must appear **unmodified** — no recolor, no recrop, no filter. This is
an IVF visual-identity compliance requirement. If the codebase has a
centralized image-optimization pipeline, exclude this file from
automatic processing.

---

## 8. Footer

Dark, three columns on desktop (Site / Project / IVF on social),
stacked on mobile. Bottom credit line: "Designed & built for V4·AIM · 2026".

---

## Floating mobile CTA (mobile only)

`<a class="cta-float" id="cta-float" href="#open-call">Join waitlist →</a>`
appears as a fixed pill at `bottom-right` once the hero is scrolled past,
and hides again when the Open Call section is in view. Respects
`env(safe-area-inset-bottom)` for iOS home-indicator clearance.

Controlled by the IIFE at the bottom of `index.html` (`// Floating CTA
visibility` comment). Logic:

```js
const pastHero = heroBottom < 80;
const inForm = ocRect.top < innerHeight * 0.6 && ocRect.bottom > innerHeight * 0.2;
cta.classList.toggle('show', pastHero && !inForm);
```

Preserve this behavior. If the port uses a framework, wrap in a
`useEffect` / `onMount` with the same scroll listener (throttled or
via `IntersectionObserver` if you want to be fancier).

---

## Hamburger / mobile menu JS

Small IIFE (`// Mobile menu toggle` comment) handles:
- Click on `.nav-burger` toggles `aria-expanded` + adds `.open` class to
  `.mobile-menu`.
- Click on any `[data-nav-link]` inside the menu closes it (so hash-
  navigation doesn't leave the menu open).
- `Escape` key closes it.

Port this 1:1. It's ~20 lines.

---

## Design checks before shipping

1. Open at 1440 px and 390 px side by side. The hero should be centered
   on the large viewport and left-aligned on the small one. If the
   desktop hero hugs the left edge, `.hero-grid` is missing
   `justify-items: center`.
2. Trigger the waitlist with a valid and invalid email. Valid should
   show "Added ✓" and clear the field; invalid should focus the input
   and shake (the placeholder does no shake — optional to add).
3. Scroll on mobile: the floating CTA should fade in after the hero,
   then fade out while the Open Call section is centered.
4. Open the mobile menu: the background should block the page scroll.
5. Check iOS Safari: the sticky nav should sit below the status bar,
   not under it. 16 px input font prevents focus-zoom.
6. `prefers-reduced-motion`: the hero ring-pulse must stop.
7. Lighthouse: targets are Performance > 95, Accessibility > 95.
   Largest text block is the H1 so LCP should be trivial.

---

## What NOT to change

- Palette is **cool neutral grey + red accent only**. No warm beige
  undertones. Tokens live in `brand/tokens.css`. If the dev server
  shows something that looks beige, you've introduced a warm hex
  somewhere — audit.
- H2 titles are **not** capped at 20ch. Keep `max-width: 44ch`.
- About section does **not** have an Objectives aside or an italic
  lede paragraph. Both were removed intentionally.
- Hero does **not** have an SVG underline under "AIM". The red accent
  is the phrase-color treatment on "conference on AI methods".
- The `Participation` conf-note is hidden; don't restore it without
  asking the coordinator.
- Mobile lede copy is shorter and different from desktop. Don't merge
  them into a single truncated string.

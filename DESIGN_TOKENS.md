# Design Tokens

**Source of truth:** `design/brand/tokens.css`. Everything here is a
mirror for documentation. If the values here and the file disagree,
**the file wins** — update this doc.

---

## Colors

| Token            | Hex                         | Usage                                              |
|------------------|-----------------------------|----------------------------------------------------|
| `--v4-ink`       | `#0F1115`                   | Primary text color on light ground                 |
| `--v4-paper`     | `#F3F4F6`                   | Page ground (cool light grey)                      |
| `--v4-white`     | `#FFFFFF`                   | Pure white — card backgrounds on banded sections  |
| `--v4-onyx`      | `#0A0A0A`                   | Dark slabs: hero, funding, footer                  |
| `--v4-bone`      | `#E8EAED`                   | Text on dark (cool light grey, not warm)           |
| `--v4-accent`    | `#E63946`                   | The one red — used sparingly                       |
| `--v4-muted`     | `#6B7280`                   | Muted body / eyebrow text                          |
| `--v4-rule`      | `rgba(15,17,21,0.10)`       | Hairlines, dividers, borders                       |
| `--surface`      | `#F3F4F6` (in index.html)   | Card backgrounds (non-banded sections)             |
| `--surface-2`    | `#E9EAEE` (in index.html)   | Banded section background (Conference, Partners)   |

### Palette rules

- **No warm / beige / cream.** The palette was deliberately retuned
  away from warm off-whites. If something reads as `#FAF8F3`,
  `#F5F3EE`, `#F6F3EC`, or `#F2F2F2`, it's legacy and needs to go.
- **Red is rare.** Reserved for: eyebrow number prefixes, the hero
  headline accent phrase, button hover, link underlines, a couple of
  hairlines on the dateplate, and the floating CTA hover. Don't add
  more red accents without asking.
- **No gradients.** Flat colors only.
- Dark-mode overrides exist in `tokens.css` but the site currently
  uses light-mode throughout. If dark mode becomes a requirement, the
  tokens flip but a lot of per-component CSS assumes light — treat
  dark-mode as a new scope of work.

---

## Typography

### Families

| Token                 | Value                                                                 |
|-----------------------|-----------------------------------------------------------------------|
| `--v4-font-display`   | `'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, Consolas, monospace` |
| `--v4-font-body`      | `'Inter', ui-sans-serif, system-ui, -apple-system, 'Segoe UI', sans-serif` |
| `--v4-font-serif`     | `'Source Serif 4', Georgia, 'Iowan Old Style', serif`                  |

- **Display (JetBrains Mono)** — all headings, eyebrows, metadata,
  nav, button labels, numeric displays. Never below 10 px when used
  for eyebrows; usually uppercase with 0.06–0.1em tracking.
- **Body (Inter)** — all running body copy, form inputs, most card
  bodies. 14–17 px.
- **Serif (Source Serif 4 if you import it, else Georgia fallback)**
  — used only for the italic hero lede. If you don't want to add
  another web font, Georgia reads fine. The italic contrast against
  the mono display type is the point.

### Scale (informational)

The tokens file declares `--v4-size-*` but the site uses inline
`clamp()` calls for responsive typography rather than consuming the
step tokens. Match these values if you port to a scale system:

| Role               | Desktop                     | Mobile (≤720px)            |
|--------------------|-----------------------------|----------------------------|
| Hero H1            | `clamp(30px, 4.2vw, 56px)`  | `clamp(40px, 10vw, 52px)`  |
| Hero lede          | `clamp(15px, 1.3vw, 18px)`  | `17px`                     |
| Section H2         | `clamp(26px, 3vw, 38px)`    | `clamp(28px, 8vw, 38px)`   |
| Section H3         | `17px`                      | `16px`                     |
| Body               | `16px`                      | `15–16px`                  |
| Eyebrow / caption  | `10–12px` uppercase         | `10–12px` uppercase        |
| Form input         | `15–16px`                   | **16px min** (iOS zoom)    |

`line-height` defaults: display = 1.05–1.12, body = 1.55.

---

## Spacing (4 px base)

| Token           | Value   |
|-----------------|---------|
| `--v4-space-1`  | 4 px    |
| `--v4-space-2`  | 8 px    |
| `--v4-space-3`  | 12 px   |
| `--v4-space-4`  | 16 px   |
| `--v4-space-5`  | 24 px   |
| `--v4-space-6`  | 32 px   |
| `--v4-space-7`  | 48 px   |
| `--v4-space-8`  | 64 px   |
| `--v4-space-9`  | 96 px   |
| `--v4-space-10` | 128 px  |

Section padding-Y: 128 px desktop, 56 px mobile.
Container max-width: 1200 px, gutter 24 px / 16 px.

---

## Radius

| Token                | Value    | Usage                                  |
|----------------------|----------|----------------------------------------|
| `--v4-radius-xs`     | 4 px     | Tiny status tags                       |
| `--v4-radius-sm`     | 6 px     | —                                      |
| `--v4-radius-md`     | 8 px     | Cards, dateplate, contact card, inputs |
| `--v4-radius-lg`     | 12 px    | —                                      |
| `--v4-radius-pill`   | 999 px   | Buttons, floating CTA, status pill     |

---

## Shadows

Used very sparingly. Two live values, both on the floating mobile CTA:

```css
box-shadow: 0 10px 32px rgba(17,17,17,0.22), 0 2px 6px rgba(17,17,17,0.12);
```

Elsewhere the design relies on hairline borders (0.5 px
`var(--v4-rule)`) for separation, not shadow.

---

## Motion

| Where                                   | Transition                                         |
|-----------------------------------------|----------------------------------------------------|
| Button bg/border                        | `transform 150ms var(--ease), background 150ms var(--ease)` |
| Button arrow on hover                   | `transform 150ms var(--ease)` (translateX 3 px)    |
| Program card border on hover            | `border-color 150ms var(--ease), background 150ms var(--ease)` |
| Partner card arrow on hover             | `transform 150ms var(--ease), color 150ms var(--ease)` (translate 3 px up-right) |
| Hamburger icon transform                | `transform 180ms var(--ease)`                      |
| Floating CTA show/hide                  | `opacity 200ms var(--ease), transform 240ms var(--ease)` |
| Hero ring-pulse                         | `ringpulse 3.6s infinite var(--ease)` with 1.2 s stagger |

- `--ease` is defined at the top of `index.html` — it's a standard
  cubic-bezier curve. Port as-is.
- All motion respects `prefers-reduced-motion: reduce`. The
  ring-pulse explicitly stops; other transitions are fast enough
  they don't need special-casing.

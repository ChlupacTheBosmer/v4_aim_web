# V4·AIM — Conference Website

Single-page website for **V4·AIM**, a cross-disciplinary Visegrad conference on
AI methods (Kostelec nad Černými Lesy, Czech Republic, 23–25 April 2027).
Funded by the International Visegrad Fund (Project ID 22610317).
Lead org: Charles University, Faculty of Science.

Published via **GitHub Pages** (branch: `main`, root `/`).

---

## Setup: Email waitlist (Formspree)

The "Notify me" form on the Open Call section submits to [Formspree](https://formspree.io).

1. Create a free account at https://formspree.io
2. Click **+ New Form**, give it a name (e.g. `v4aim-waitlist`)
3. Copy the form ID from the endpoint URL (looks like `xabc1234`)
4. In `index.html`, find the line:
   ```js
   const FORMSPREE_ENDPOINT = 'https://formspree.io/f/YOUR_FORM_ID';
   ```
   Replace `YOUR_FORM_ID` with your actual form ID.
5. Commit and push — submissions will appear in your Formspree dashboard.

Free tier: 50 submissions/month. Upgrade if needed.

---

## GitHub Pages

Set **Settings → Pages → Source** to:
- Branch: `main`
- Folder: `/ (root)`

The site will be live at `https://chlupacthebosmer.github.io/v4_aim_web/`.

---

## Partners

Partner institutions 02–06 are shown as **TBD** until officially announced.
To reveal them, update the relevant `<div class="partner">` blocks in `index.html`
with the institution names, departments, countries, and URLs, and change `<div>`
to `<a href="..." target="_blank" rel="noopener">`.

---

## About the design files

The `design/` folder contains the original design handoff (static HTML reference).
The published site is `index.html` at the repo root.

---

## Fidelity

**High-fidelity.** Final colors, typography, spacing, and interactions are in
place. Build the codebase version pixel-perfect against the HTML reference.

---

## What needs a backend

There is exactly **one** form on the page today: the waitlist.

```html
<form class="mail-wait" id="waitlist" novalidate>
  <input id="email-wait" type="email" required />
  <button type="submit">Notify me</button>
</form>
```

The current JS handler in `index.html` (search for `// Waitlist faux-submit`)
is a placeholder — it validates an email client-side and swaps the button text
to "Added ✓". **Replace the placeholder with a real POST.**

### Backend requirements

- `POST /api/waitlist` accepting `{ email: string }`
- Validate email server-side (RFC 5322 lite is fine; the client uses the
  common `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` check)
- Store in a simple `waitlist` table: `id, email, created_at, ip, user_agent`
- Rate-limit per IP (10 req / hour is plenty)
- Unique constraint on `email`; return 200 even on duplicate (don't leak who's
  already subscribed)
- Respond with `{ ok: true }` on success, `{ ok: false, error: "..." }` on
  failure; the existing UI toggles `form.classList.add('ok')` on success and
  focuses the input on failure — keep those hooks working

The site does **not** send transactional email on signup. When the call opens
(late 2026 / early 2027), the project coordinator will send a one-time
broadcast to the stored emails. A simple admin export (CSV) is enough.

### Optional follow-ups the coordinator may ask for later

- Double-opt-in (confirmation email). Not required for v1.
- GDPR consent checkbox + privacy-policy link. Recommended — this is an EU
  academic project. Add a checkbox below the input; don't split into a
  multi-step form.
- Language switcher (CS / SK / PL / HU / EN). Out of scope for v1 but design
  all strings through a localization layer from day one so it's trivial
  later.

---

## Critical: preserve both desktop AND mobile behaviour

The design is fully responsive with a **single `@media (max-width: 720px)`
break** that flips several components into dedicated mobile layouts. A
naïve port that only ships the desktop version is not acceptable.

**See `HANDOFF.md`** for the complete breakdown of what changes at the
breakpoint, but the headline items:

| Area                    | Desktop                          | Mobile (≤720px)                                                 |
| ----------------------- | -------------------------------- | --------------------------------------------------------------- |
| Hero layout             | Centered mark + headline + CTAs  | Left-aligned, full-width                                        |
| Hero lede copy          | Long version (`.lede.desktop`)   | Short version (`.lede.mobile`) — different text, not truncation |
| Top nav                 | Inline link list + status pill   | Hamburger → full-screen typographic overlay                     |
| Conference program      | 4-row vertical list              | Horizontal scroll-snap carousel                                 |
| Partners grid           | 3×2 grid                         | Horizontal scroll-snap carousel                                 |
| Floating waitlist CTA   | Hidden                           | Sticky pill bottom-right, appears after hero scroll             |
| Waitlist form layout    | Inline input + button            | Stacked, 52 px min hit targets (avoids iOS zoom: 16 px font)    |
| Section headline scale  | `clamp(26px, 3vw, 38px)`         | `clamp(28px, 8vw, 38px)`                                        |

When refactoring into components, **do not replace the two-lede pattern with
a single string + truncation** — the mobile lede is deliberately different
copy, not a shortened version of the desktop lede.

---

## Assets

All assets live in `design/brand/`. These are final and should be copied
into the production codebase's asset pipeline as-is.

- `brand/tokens.css` — **single source of truth** for color + type tokens.
  Ship this file (or port it to your framework's theme system verbatim).
- `brand/favicon.svg` — favicon, already linked in the HTML `<head>`.
- `brand/ivf-logotype-black.png` — International Visegrad Fund logotype
  (black), used in the Funding section footer. **Do not modify** — the IVF
  visual identity guidelines require the logotype to appear unchanged.
- `brand/ivf-logotype-white.png`, `brand/ivf-black.png`, `brand/ivf-white.png`,
  `brand/mark.svg`, `brand/mark-white.svg`, `brand/primary-*.svg`,
  `brand/wordmark-*.svg` — alternate IVF marks and V4·AIM variants, kept for
  completeness; only `ivf-logotype-black.png` is currently referenced from
  `index.html`.

Fonts are loaded from Google Fonts via `<link>` in the `<head>`:
- **JetBrains Mono** (400, 500, 700) — display / UI
- **Inter** (400, 500, 600) — body
- *(Optional)* **Newsreader** or similar serif for the italic hero lede —
  currently falls back to the system serif stack via `var(--v4-font-serif)`
  in `tokens.css`. If you want the italic lede to have more character, add
  the Google Fonts import for a proper editorial serif.

If the target codebase self-hosts fonts for GDPR reasons, use the
`@fontsource/inter` and `@fontsource/jetbrains-mono` packages.

---

## Files in this bundle

```
design_handoff_v4aim_landing/
├── README.md                  ← this file
├── HANDOFF.md                 ← detailed screen-by-screen spec
├── DESIGN_TOKENS.md           ← complete token reference
└── design/
    ├── index.html             ← full page reference (desktop + mobile)
    └── brand/                 ← logo + favicon + token file
        ├── tokens.css
        ├── favicon.svg
        ├── ivf-logotype-black.png
        └── (alternate marks)
```

**Start with `HANDOFF.md`.** It has every screen section, every design
decision with its rationale, and the complete list of breakpoint overrides.

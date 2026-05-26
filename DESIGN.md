# RAD Engineering — Design System

Reference for keeping the site visually consistent across future edits.
All tokens live as CSS variables in `styles.css:8-28` — change them
there to propagate sitewide.

---

## 1. Aesthetic intent

Clean, technical, engineering-leaning. Off-white "paper" base with a
faint blueprint grid behind everything; navy + cyan as the only
saturated colors; mono type used as accent against the sans body.

Hierarchy is built from **type weight + size + numbering + thin
rules**, not from boxes or backgrounds. When we add interactive
treatment (hover, focus), prefer animation of existing elements
(slide, fade, opacity) over introducing new fill or stroke.

---

## 2. Palette

| Token | Hex | Usage |
|---|---|---|
| `--c-bg` | `#FAFAF8` | Page base ("paper") |
| `--c-bg-alt` | `#F4F4F1` | Reserved — currently unused on sections (services lost its tint so the grid flows through); keep available for any future subtle differentiation |
| `--c-ink` | `#1A1F2A` | Body text |
| `--c-ink-soft` | `#4A5462` | Secondary text (body paragraphs, eyebrows, meta) |
| `--c-muted` | `#8A93A1` | Reserved for further-de-emphasized text |
| `--c-rule` | `#E5E5E1` | Hairline borders (header underline, section-alt borders, service-card grid borders) |
| `--c-navy` | `#2B3D51` | Headings, primary button, contact section background |
| `--c-navy-dark` | `#1F2D3C` | Primary button hover |
| `--c-cyan` | `#4BC9EF` | Accent only — never for body color. Used for: brand "Engineering" wordmark, section-label numbers, hover underlines, the recurring "." motif (see §6) |
| `--c-cyan-dark` | `#2BB3DC` | Reserved for future use |

**Rules of thumb**
- Cyan is precious. Use it on one or two elements per section maximum, always small.
- Body copy is `--c-ink-soft`, never `--c-ink`. Headings are `--c-navy`, never `--c-ink`.
- Never apply opaque section backgrounds that block the grid unless the section is intentionally heavy (contact's navy is the only case).

---

## 3. Typography

Two families, both via Google Fonts CDN with preconnect already wired
up in `index.html`.

- **`--ff-sans`** — IBM Plex Sans. Weights: 400, 500, 600, 700.
  Body text, headings, button labels, partner names.
- **`--ff-mono`** — IBM Plex Mono. Weights: 400, 500.
  Used as accent: eyebrows, section labels, nav, buttons, location
  meta, service card numbers.

**The mono/sans contrast is the type system.** Any new "label,
metadata, system-y" text should be mono. Any new "content, prose,
heading" text should be sans.

**Scale (clamp values let everything breathe responsively)**

| Element | Size |
|---|---|
| `.hero-title` | `clamp(32px, 6vw, 68px)`, weight 700, letter-spacing `-0.025em`, line-height 1.05 |
| `.section-title` | `clamp(28px, 4vw, 44px)`, weight 700 |
| `.section-contact .section-title` | `clamp(25px, 3.6vw, 40px)` — intentionally 10% smaller than other section titles so the section reads as a quieter close |
| `.contact-email` | `clamp(22px, 4vw, 42px)`, weight 600 |
| `.service-title` | 21px, weight 600 |
| Body / `.about-body p` | 17px, line-height 1.7 |
| `.hero-sub` | `clamp(16px, 1.6vw, 19px)`, line-height 1.55 |
| `.contact-lede` | 18px, line-height 1.6, color at 72% alpha |
| `.eyebrow`, `.section-label` | 12px mono, uppercase, wide letter-spacing |
| `.btn`, `.site-nav`, `.contact-meta` | 13px mono |
| `.service-num` | 12px mono |

**Letter-spacing conventions**
- Sans display sizes (titles): negative tracking (`-0.02em` to `-0.025em`).
- Mono labels: positive tracking (`0.08em`–`0.18em`).
- `.contact-meta` uses tightened mono — `letter-spacing: 0.056em` and
  `word-spacing: -0.4em` to keep the location line from looking stretched.

---

## 4. Spacing and the grid

### The blueprint grid

Site background is a fixed-attachment quad grid rendered as four
linear-gradients on `body::before` (`styles.css` around line 56).

- **Minor lines** every **12px** at `rgba(43, 61, 81, 0.015)`
- **Major lines** every **48px** at `rgba(43, 61, 81, 0.06)`
- **Vignette**: radial mask fades opacity to ~17% at the edges, so
  the pattern reads strongest in the page center
- Layer sits at `z-index: -1` behind all content; `position: fixed`
  so it stays anchored to the viewport while content scrolls past

### Spacing rhythm

The 12px minor / 48px major grid is the unit system. New content
should land on these multiples where practical (vertical padding,
margins, component sizes). This isn't enforced strictly, but engineers
notice misalignment, so try to:

- Section vertical padding: `clamp(80px, 12vh, 140px)` (currently a
  fluid value; 96px and 144px are the nearest 48 multiples if you
  ever need to lock it)
- Container max width: 1180px, horizontal pad `clamp(20px, 4vw, 48px)`
- Service cards: `min-height: 260px`, padding `36px 32px 40px`
- Partner cells: `min-height: 140px`, padding `32px 24px`

### Borders

Always `1px solid var(--c-rule)`. Used sparingly:
- Header bottom border
- `.section-alt` top + bottom (visual separator between sections even
  though the section has no background)
- Service card grid (each card has right + bottom; the grid container
  has top + left to close the perimeter)

---

## 5. Component patterns

### Section structure

Every content section follows this pattern (see About / Services /
Partners / Contact in `index.html`):

```html
<section id="..." class="section [section-alt|section-contact]">
  <div class="container">
    <div class="section-label">
      <span class="label-num">/ NN</span>
      <span class="label-rule"></span>
      <span class="label-text">Section name</span>
    </div>
    <h2 class="section-title">Headline.</h2>
    <!-- content -->
  </div>
</section>
```

**The `/ NN` numbering convention is core to the aesthetic.** Always
zero-padded (`/ 01`, `/ 02`), monospace, cyan number + navy text
separated by a 56px hairline rule. New sections continue the numbering
(currently up to `/ 04`).

The `.js-ready .section` rule adds a subtle fade-in-on-scroll via
IntersectionObserver. Don't override it.

### Buttons

Two variants:
- **`.btn-primary`** — solid navy fill, off-white text, lifts 1px on hover
- **`.btn-ghost`** — transparent fill, navy border + text, inverts on hover

Both use mono type, 13px, `letter-spacing: 0.04em`, `border-radius: 2px`
(the global `--radius`). Keep it that way — softer radii would feel
off-brand.

### Service cards (transparent tiles, hover-to-reveal)

Cards have no fill — the body grid shows through. Hairline borders
form the tile grid. On hover or keyboard focus:

1. The `.service-head` (number + title) slides from vertical center
   to the top
2. The `.service-body` fades in below
3. A cyan rule animates across the top edge (`::before`)

On touch devices (`@media (hover: none)`), all bodies stay visible
in normal flow position — no hover state to depend on.

Each card has `tabindex="0"` so keyboard users can reveal the body.

### Partner logos

Transparent cells, no borders, no fills. Each logo is wrapped in an
`<a target="_blank" rel="noopener">` linking to the partner site,
with `display: inline-block` (not flex — see the next bullet for
why) and a `opacity: 0.65` hover for affordance.

**Important: any new partner SVG must have explicit `width` and
`height` attributes on the root `<svg>` element**, not just
`viewBox`. SVGs with only viewBox collapse to 0×0 inside the `<a>`
wrapper because the layout can't compute intrinsic size. PNG/JPG
logos are fine either way.

### Contact section

Inverts the palette: navy background, off-white text. Section-label
text and rule use `--c-bg` instead of `--c-navy`. The section-title
is 10% smaller than other section titles (see §3). The email is a
link with cyan hover state. Location meta uses tightened mono kerning.

---

## 6. The cyan period (`.accent`)

A recurring punctuation motif: a cyan dot at the end of a phrase
where the surrounding text is navy. The class `.accent` is defined
simply as `color: var(--c-cyan)`.

Current uses:
- Hero title: "From prototype to production**.**"
- Contact title: "Let's talk about your project**.**" (the period
  here is just the title text, navy — only the dot in the email
  domain gets the cyan)
- Email: "info@radengineer**.**com" — the dot between domain and TLD

When using it, place a single cyan dot at a meaningful boundary
(sentence end, domain separator). Never multiple per phrase. Never on
non-punctuation glyphs.

### Email hover — the "dot jump"

The contact email has a hover state where the cyan accent moves from
the domain period to the dot above the "i" in "info" (the domain
period simultaneously turns off-white). To enable this, the "i" is
actually a Turkish dotless ı (U+0131) wrapped in `<span class="ci">`,
and the dot is drawn via a `::after` pseudo-element so its color can
be controlled independently. The pseudo is tuned to match Plex Sans's
natural i-dot shape (`width/height: 0.18em`, `border-radius: 0.02em`,
`top: 0.16em`). If the font ever changes, re-tune those values.

For accessibility, the anchor carries `aria-label="info@radengineer.com"`
and every visible span has `aria-hidden="true"` so screen readers
announce the proper email instead of "ınfo".

---

## 7. Interaction conventions

- **Transitions**: standard duration is `--transition` (220ms,
  custom easing curve). Slide-and-reveal animations (service cards)
  go a little longer at 320–360ms for legibility.
- **Hover**: prefer color or opacity shifts on the affected element
  itself, not separate hover overlays. Cyan underlines on links and
  nav items animate width from 0 to 100%.
- **Focus**: visible outlines use cyan inset (`outline: 2px solid
  var(--c-cyan); outline-offset: -2px`). Only `:focus-visible`, not
  `:focus`, so it doesn't show for mouse users.
- **Touch awareness**: any hover-only reveal must have a
  `@media (hover: none)` fallback that shows the hidden content by
  default. Touch users can't hover.

---

## 8. Responsive breakpoints

Used breakpoints, in order of how often they apply:

- `768px` — about grid collapses to single column
- `900px` — services grid drops from 3 columns to 2
- `560px` — services grid drops from 2 to 1
- `700px` — partners row drops from 3 columns to 1

Add new breakpoints sparingly. Most components scale fluidly via
`clamp()` and don't need media queries at all. When you do add one,
match an existing value rather than inventing a new one.

---

## 9. Accessibility checklist for new components

- All interactive cells (cards, logos, links) must be keyboard-reachable
- Use `aria-label` on links whose visible content is just an image
- Hover-only state changes must have a `@media (hover: none)` fallback
- Decorative SVGs use `aria-hidden="true"`
- Color contrast: body text on `--c-bg` and white text on `--c-navy`
  both clear AA. Don't introduce new text colors without checking
  contrast.

---

## 10. What to avoid

- New saturated colors. The palette is intentionally tiny.
- Box shadows. The site uses borders + grid + type for hierarchy; a
  shadow would feel like a different design system.
- Border-radius larger than `--radius` (2px). The aesthetic is
  drafted, not soft.
- Opaque section backgrounds that block the grid (services lost its
  tint for exactly this reason).
- Decorative emoji or icons. The numbering and rules carry the
  visual rhythm.
- New font families. Plex Sans + Plex Mono is the system.

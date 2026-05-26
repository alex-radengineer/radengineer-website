# radengineer-website

Static one-page marketing site for RAD Engineering. Served via GitHub Pages with custom domain `radengineer.com`.

## Structure

```
.
├── index.html              # The entire site (one-pager)
├── styles.css              # Stylesheet
├── CNAME                   # GitHub Pages custom domain config
├── assets/
│   ├── rad-logo.png        # Original logo source (not used in HTML, kept for reference)
│   └── partners/
│       ├── hasa.svg
│       └── sani-marc.svg
└── README.md
```

## Stack

- Pure HTML/CSS — no build step, no JS framework
- Fonts: IBM Plex Sans + IBM Plex Mono via Google Fonts
- Hosting: GitHub Pages
- DNS: Squarespace (Google Cloud DNS nameservers, A records pointed at GitHub)

## Local preview

Open `index.html` in a browser. No server needed.

For a slightly more accurate preview (proper MIME types, no file:// quirks):
```
cd /path/to/repo
python3 -m http.server 8000
# then open http://localhost:8000
```

## Updating content

All content lives in `index.html`. Edit, commit, push — GitHub Pages auto-deploys on push to `main`. Propagation is usually <1 minute.

### Common edits

- **Tagline / hero copy** — search for `class="hero-title"` in `index.html`
- **About paragraph** — search for `class="about-body"`
- **Services** — each is an `<article class="service-card">` block
- **Partners** — `<ul class="partners-row">`. Drop new logos into `assets/partners/` and add an `<li>` entry.
- **Contact email** — search for `info@radengineer.com`

### Adding a partner logo

```
1. Save the logo as SVG (preferred) or PNG to assets/partners/
2. Add an entry inside <ul class="partners-row"> in index.html:

   <li class="partner" aria-label="Company Name">
     <img src="assets/partners/company-name.svg" alt="Company Name" loading="lazy">
   </li>

3. Adjust grid-template-columns in styles.css if you go beyond 3 partners
```

### Adding a Projects section later

The site is structured to drop in a `/ 05 Projects` section between Partners and Contact when client approvals come through. Pattern matches the existing sections.

## Brand palette

- Navy: `#2B3D51` (extracted from logo)
- Cyan: `#4BC9EF` (extracted from logo)
- Background: `#FAFAF8`
- Body text: `#1A1F2A`

Defined as CSS variables at the top of `styles.css` — edit there to propagate sitewide.

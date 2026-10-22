# Col·lecció Ortín-Escudé — Decisions

Static site for GitHub Pages that renders the art collection live from a public Google Sheet. Plain HTML/CSS/JS, single file, no build step.

## Data source

- Sheet: `1f8HHAOD0xZ0JKr1LHRxvbPEkplWMtI6hWt6jC8_ebDs`, tab `gid=0`, read via the gviz JSON endpoint on every page load.
- Columns: Títol, Autor, Any Compra, Any Producció, Estil, Tipus, Descripció, Foto, Preu, Lloc, Wiki, Nota, Preu de venta.
- Config (`SHEET_ID`, `GID`, `EMAIL_PARTS`) lives at the top of the `<script>` in `index.html`.

## Privacy

- **Not shown on the site:** Preu, Preu de venta, Nota.
- **Accepted risk:** the sheet is public, so these columns remain readable by anyone who opens the sheet/gviz URL from the page source. Hiding is cosmetic only. (Rejected alternative: a public tab built with `QUERY` excluding private columns.)

## Images

- Foto cell holds a URL. If it starts with `http(s)://` it's used as the image.
- Google Drive share links: ID extracted from `/d/<ID>` or `?id=<ID>`, then tried in order: `https://drive.google.com/thumbnail?id=<ID>&sz=w1000` → `https://lh3.googleusercontent.com/d/<ID>=w1000` → placeholder. Images use `referrerpolicy="no-referrer"`; failures are logged to the console (`Imatge no carregada:`).
- Anything else (empty, bare filename) → placeholder tile in a palette colour with text "Foto no disponible". Same placeholder if an image URL fails to load.
- Gallery images: fixed 4:5 ratio, `object-fit: cover`. Detail image: natural ratio.

## Fields on the detail page

- Descripció as the lead paragraph.
- Fact table: Títol, Autor, Any de producció, Any de compra, Estil, Tipus, **Procedència** (from `Lloc`), Enllaç Wiki (link).
- Empty fields are omitted from the table.
- Missing Títol → "Sense títol". Autor empty or `N/A` → "Autor desconegut".

## "Fes una oferta"

- `mailto:` to `jordi.escude.godia@gmail.com`, pre-filled subject (`Oferta: <títol> — <autor>`) and body template (offer amount, name, phone).
- Address assembled in JS from parts (light anti-scraper).
- Fallback "Copia l'adreça" button copies the email to the clipboard.

## Structure & routing

- Single `index.html` with hash routing:
  - `#/` → gallery
  - `#/obra/<slug>` → detail
- Slug = title slugified; falls back to row number if no title; duplicates get `-2`, `-3`…
- Renaming a title in the sheet changes its URL (old shared links fall back to the gallery).
- Gallery scroll position is restored when going back.

## Layout & design

- Based on the Claude Design bundle (now in `_design/`): Cormorant Garamond / Inter / JetBrains Mono (Google Fonts), paper/ink palette, terracotta accent.
- Gallery as a **grid**: 3 columns desktop, 2 tablet (≤1080px), 1 mobile (≤560px). Sheet order, no filters.
- Header: OE logo + "Col·lecció Ortín-Escudé" only. Nav: Obres, Llibres (disabled, "Properament").
- No hero / h1 on the gallery; just a small "N obres" count line.
- Footer: "© Col·lecció Ortín-Escudé" only.
- Catalan only.

## Repo layout

- `index.html` — the site.
- `_design/` — reference only: Claude Design bundle, old `index.html` (as `index-old.html`), `index2.html`. GitHub Pages (Jekyll) skips `_`-prefixed folders, so these aren't served. **Don't add `.nojekyll`** or they'll be published.
- `.gitignore` — `.DS_Store`.

## Later

- Llibres (books) section — currently disabled in the nav.
- Fill in missing photos in the sheet.

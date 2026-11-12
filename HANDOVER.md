# DNS Lookup – Handover

## Projekt
- **Lokaler Pfad:** `/Users/lakestudio/localhost/claude/apps/dns-lookup/`
- **Repo:** https://github.com/b0li/dns-lookup
- **Live:** https://b0li.github.io/dns-lookup/
- **Stack:** Single `index.html` – Vanilla JS, kein Build-Step, keine Dependencies, kein Backend

---

## Zweck

DNS-Record-Abfrage direkt im Browser über **DNS-over-HTTPS** (DoH). Kein Server nötig –
die Abfragen laufen clientseitig gegen öffentliche Resolver.

---

## Features

- **Record-Abfrage** per Chip-Auswahl: A, AAAA, CNAME, MX, TXT, NS, SOA, CAA
- **Preset „Alle Records"** – fragt das komplette Standard-Set in einem Durchgang ab
- **Preset „E-Mail prüfen"** – SPF (TXT @), DMARC (`_dmarc`), DKIM (CNAME + TXT) mit frei wählbarem Selector
- **Plain-Text-Bewertung** der Mail-Records:
  - SPF: erkennt Policy (`-all` hart / `~all` soft / neutral)
  - DMARC: erkennt `p=none/quarantine/reject`
  - DKIM: Hinweis bei fehlendem Eintrag
- **Provider-Erkennung** aus den NS-Records (Strato, IONOS, Cloudflare, Hetzner, OVH,
  All-Inkl, netcup, dd24, goneo, AWS Route 53, Azure DNS, Google Domains …)
- **Kopieren** jedes Record-Werts per Button (Clipboard API)
- Domain-Normalisierung (entfernt `https://`, Pfade, trailing dot, lowercase)

### Resolver (Fallback-Kette)
```
1. https://dns.google/resolve
2. https://cloudflare-dns.com/dns-query   (Accept: application/dns-json)
```

---

## Design System (LS-CI, identisch zu den anderen Apps)

### CSS Custom Properties
```css
:root {
	--bg: #1a1d23;
	--surface: #22262e;
	--surface-2: #2a2f39;
	--border: #333844;
	--accent: #d4a55a;
	--accent-dim: rgba( 212, 165, 90, 0.15 );
	--green: #6abf69;
	--red: #e06c6c;
	--text: #e8eaf0;
	--text-dim: #8a909e;
	--radius: 10px;
}
```

### Fonts
```html
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=DM+Sans:wght@400;600;700&display=swap');
```
- **h1:** `DM Serif Display`, 2rem, weight 400, `var(--accent)`, zentriert
- **Body:** `DM Sans`, system-ui Fallback
- **Mono (Werte):** `ui-monospace, 'SF Mono', Menlo, Consolas, monospace`

### Komponenten
- **Karten:** `var(--surface)`, 1px `var(--border)`, `var(--radius)`, padding 18px 20px
- **Buttons primär:** `var(--accent)` BG, Text `#1a1d23`
- **Buttons secondary:** `var(--surface-2)`, `var(--text-dim)`, Hover → Accent-Border
- **Chips:** Pill (border-radius 999px), aktiv = Accent-BG + Text `#1a1d23`
- **Notes:** `--ok` grün, `--warn` accent, `--err` rot (auf `--accent-dim` / rotem BG)

---

## Code-Style (LS-Standard)

- **Tabs** für Einrückung
- **Spaces inside Parentheses:** `document.querySelector( '.foo' )`
- **Space nach `!`:** `if ( ! el ) return;`
- **Closing Callbacks:** `});` (kein Space vor `}`)
- Leerzeilen zwischen logischen Blöcken, Kommentare auf Englisch
- Vanilla JS, kein Framework
- Alles in einer `index.html` (inline `<style>` + `<script>`)

---

## Deployment

```bash
# GitHub Pages: main branch, root /
git add index.html
git commit -m "..."
git push
# → live auf https://b0li.github.io/dns-lookup/
```

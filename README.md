# aeris

Scrollable world weather dashboard — live conditions across 12 cities with animated glass cards.

**Visual Style:** Light — Breeze (pale sky blue, soft lavender/pink accents, glass cards with temperature-tinted gradients)

**Data Source:** Open-Meteo (api.open-meteo.com) — current weather for 12 world cities

**How it works:**
- Cards animate in as you scroll (Anime.js scroll-triggered entrance animations)
- Temperature controls card background tint (warm = hot city, cool = cold city)
- Humidity displayed as dot opacity
- Wind displayed as bar width
- Weather codes mapped to emoji icons (clear, cloudy, rain, snow)
- Tap any card for detailed breakdown in quiet zone panel
- Scrolling ticker at top shows all city highlights

**GitHub:** https://github.com/parthchandak02/aeris

**Live:** https://aeris.parthchandak.info

**Tech:** Single HTML + Anime.js CDN (kit tier). Hosted on Cloudflare Pages.

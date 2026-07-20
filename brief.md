# aeris — Creative Brief

## Concept
Live weather conditions across 12 world cities. Animated glass cards with scroll-triggered entrance animations (Anime.js). Each card shows temperature, humidity, wind, and conditions with a gradient tint reflecting the local weather. A scrolling ticker at top shows a live weather highlight. Tap a card for detailed breakdown in a quiet zone panel.

**Render tier:** kit — single HTML file + Anime.js CDN
**Narrative Pattern:** Flow — scroll-driven city-by-city exploration
**Tier 2 Patterns (pick 2):** Hero Number + Quiet Zone

## Data Source
- Primary: Open-Meteo current weather for 12 cities
  URL: `https://api.open-meteo.com/v1/forecast?latitude={lat}&longitude={lng}&current_weather=true&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,weather_code,wind_speed_10m,surface_pressure`
  - 12 cities: San Francisco (37.77,-122.42), New York (40.71,-74.01), London (51.51,-0.13), Tokyo (35.68,139.69), Sydney (-33.87,151.21), Mumbai (19.08,72.88), Dubai (25.20,55.27), Singapore (1.35,103.82), Paris (48.86,2.35), Berlin (52.52,13.41), Toronto (43.65,-79.38), São Paulo (-23.55,-46.63)
- Fallback: embedded static weather data for all 12 cities (reasonable defaults: 20-25°C, clear)

## Visual Style
- **Preset:** Light — Breeze (pastel sky blue, soft pinks/lavenders)
- **Palette (locked):**
  ```js
  const PALETTE = {
    bg: '#e8f4f8',         // pale sky blue
    card: 'rgba(255,255,255,0.35)',  // glass card bg
    cardBorder: 'rgba(255,255,255,0.5)',
    accent1: '#81d4fa',    // soft blue
    accent2: '#ce93d8',    // soft lavender
    accent3: '#f48fb1',    // soft pink
    accent4: '#fff176',    // pale yellow highlight
    text: 'rgba(0,0,0,0.7)',
    textMuted: 'rgba(0,0,0,0.4)',
    warm: '#ff8a65',       // warm cities
    cool: '#4fc3f7',       // cool cities
  };
  ```
- **Font:** system-ui, sans-serif everywhere. No Inter, Roboto, Open Sans.
- **Glass card style:** backdrop-filter: blur(16px), border: 1px solid rgba(255,255,255,0.5), border-radius: 20px, background: rgba(255,255,255,0.25)

## Encoding Contract
| Data Field | Visual Channel | Range | Inverse | "Bigger means..." | Legend? |
|---|---|---|---|---|---|
| temperature_2m (°C) | card background gradient (warm→cool tint) | -5 to 45°C → rgba hex | scale(temp, -5,45, warmHex,coolHex) | hotter city → warmer card tint | no (visual is intuitive) |
| weather_code | emoji icon + description | 0-99 | lookup table | higher code = worse weather | yes (legend: ☀️ clear ⛅ cloudy 🌧️ rain ❄️ snow) |
| wind_speed_10m (km/h) | wind indicator bar width | 0-60 km/h → 0-80px | scale(kmh, 0,60, 0,80) | stronger wind → wider bar | no (bar width is physical) |
| relative_humidity_2m (%) | humidity dot opacity | 0-100% → 0.2-1.0 | scale(hum, 0,100, 0.2,1.0) | more humid → more opaque dot | no (opacity is intuitive) |

## Interaction
- **Scroll:** gradually reveals city cards from bottom (Anime.js scroll-triggered entrance)
- **Tap on card:** opens Quiet Zone panel showing detailed breakdown (temperature, feels like, humidity, wind, pressure, precipitation) + "Data: open-meteo.com" link. Fades after 4s idle.
- **Auto-animation:** ambient pulse on the temperature value (gentle breathing)

## Communication Requirements
**Tier 1 (mandatory):**
1. Persistent Data Stamp — "Open-Meteo · 12 cities · live · Xs ago" at bottom
2. Data Process Honesty — "fallback" if API fails, never "live"
3. One-to-One Sensory Mapping — warm tint = warm temp, wider wind bar = stronger wind

**Tier 2 (2 patterns):**
4. Hero Number — 56px temperature of first/nearest city at top-left, with city name label
5. Quiet Zone — tap card reveals detailed weather breakdown, fades after 4s idle

**Coherence:**
6. Generative-art axes: hairline borders (0.5px), no fills on charts, rounded corners everywhere, one accent color palette
7. Motion tokens: entrance=360ms ease-out-expo, feedback=160ms, ambient=240ms
8. AI Slop Score < 30 — system-ui font only, no purple gradient, layered glass background

## Code Requirements
1. Single HTML file `index.html` with Anime.js via CDN: `https://cdn.jsdelivr.net/npm/animejs@4.0.2/lib/anime.min.js`
2. Config object `C` at top with all tunable values (city list, animation durations, palette)
3. DPR-aware canvas NOT needed (this is DOM-based, not canvas)
4. Mobile-first responsive: cards stack in 1 col on mobile (<640px), 2 cols on tablet, 3-4 cols on desktop
5. Touch guard for tap interactions
6. First frame shows 2-3 visible cards + ticker (pre-rendered, no scroll needed)
7. Realistic fallback data if API fails
8. Scroll-triggered entrance: anime({ targets, translateY: [100,0], opacity: [0,1], delay: anime.stagger(80) })
9. `#desc` div at bottom center: "Scroll to explore weather across 12 cities · Tap a card for details"
10. `.nojekyll` file at project root

## Interaction Affordances
- Cards have a subtle hover lift (translateY(-4px), box-shadow increase)
- On first visit, first city card pulses gently to indicate interactivity
- After 4s idle, quiet zone fades to clean state
- Tappable cards show a subtle glow border on tap

## Chrome Positions (no overlap)
- Hero number: position fixed, top 24px left 24px, z-index 10
- Scrolling ticker: position fixed, top 100px left 0 right 0, z-index 5, horizontal scroll of weather highlights
- Data stamp: position fixed, bottom 20px left 16px, z-index 10, 10px system-ui, opacity 0.4
- Legend: position fixed, bottom 44px center, z-index 5, 10px system-ui, opacity 0.35
- Desc: position fixed, bottom 68px center, z-index 5, 11px system-ui, opacity 0.45, max-width 65ch
- Quiet Zone: position fixed, bottom 140px center, z-index 100, width 320px, fades after 4s

## No Em-Dashes
Use hyphens for all placeholder values and descriptions. No em-dashes anywhere.

## Tech Stack
- Single HTML file + Anime.js CDN (kit tier)
- Open-Meteo free API (no auth)
- Deployed to Cloudflare Pages via wrangler
- URL: https://aeris.parthchandak.info

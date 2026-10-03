# Janam Kundali (Vedic Birth Chart)

A single-file, offline-capable web app that computes a Vedic birth chart in the browser. No server, no build step, no dependencies. Birth data never leaves the device.

> **Disclaimer:** The app calculates astronomical positions and applies traditional Jyotiṣa rules. Interpretations of such charts have no established scientific predictive validity. Do not base medical, financial or legal decisions on them.

## Quick start

1. Open `janam-kundali.html` in any modern browser (Chrome, Edge, Firefox, Safari).
2. Enter date, local time, place (preset or custom latitude/longitude) and IANA time zone.
3. Click **Calculate Kundali**.

## Features

| Area | Details |
|---|---|
| Inputs | Name (optional), date, local time, latitude/longitude, IANA time zone |
| Summary | Lagna, Moon sign, Nakshatra + pada, tithi, current daśā, ayanāṃśa, UTC offset |
| Grahas | Sun, Moon, Mars, Mercury, Jupiter, Venus, Saturn, Rahu, Ketu: sign, degree, nakshatra/pada, bhava, speed (°/day), retrograde, exalted/debilitated/own sign |
| Charts | North Indian and South Indian SVG layouts for D1, D2, D3, D7, D9, D10, D12 |
| Interaction | Tap a house or graha for details (lord, themes, kendra/trikona/dusthana, nakshatra lord, houses ruled) |
| Daśā | Vimshottari mahādaśā timeline (proportional), antardaśā tables, current period highlighted |
| Storage | Save/load/delete profiles in `localStorage` (last 50, tagged with calculation version) |
| Self-test | 10 checks run on every page load and show pass/fail |

## Calculation model

- **Zodiac:** sidereal, Lahiri ayanāṃśa (23.857092° at J2000 plus general precession). Mean ayanāṃśa, no nutation.
- **Houses:** whole-sign, counted from the Lagna sign.
- **Time:** local time → UTC via the browser's IANA database (handles historical offsets and DST) → Julian Day. ΔT from Espenak/Meeus polynomials.
- **Sun and planets:** JPL approximate Keplerian elements (1800–2050), Kepler equation solved by Newton iteration, geocentric ecliptic longitude, precessed to date.
- **Moon:** Meeus truncated series (30 longitude terms plus additive terms).
- **Rahu/Ketu:** mean lunar node.
- **Speed/retrograde:** central difference over ±0.05 day. Sun and Moon are never marked retrograde; nodes are always retrograde.
- **Lagna:** from local sidereal time, obliquity and latitude: `atan2(cos θ, −(sin θ cos ε + tan φ sin ε))`.
- **Nakshatra:** 27 × 13°20′, each split into 4 padas of 3°20′.
- **Vimshottari:** 120-year cycle in the order Ketu, Venus, Sun, Moon, Mars, Rahu, Jupiter, Saturn, Mercury. The first daśā balance comes from the Moon's elapsed fraction of its nakshatra. A year is 365.25 days. Antardaśā length = mahādaśā years × sub-lord years ÷ 120.
- **Vargas:** D9 = `floor(λ·9/30) mod 12`. D2, D3, D7, D10 and D12 follow Parāśari rules.

## Accuracy and limits

- Planets: about 0.02° typical. Moon: about 0.01°. Sun: about 0.002° against Meeus's example.
- No nutation, aberration or light-time correction. Mean nodes only.
- Charts with a planet or ascendant within a few arcminutes of a sign or nakshatra boundary may differ from Swiss Ephemeris.
- Years 1800–2100 and latitudes within ±66° only. The ascendant is unreliable near the poles.
- Pre-1970 births: confirm the time zone, since the IANA database may not match local historical practice.

## Project layout

The deliverable is one file, built from three parts:

```
janam-kundali.html
├── <style>        design tokens, light/dark theme, responsive layout
├── engine.js      calculation engine (no DOM, Node-testable)
└── ui.js          form, SVG charts, tables, daśā UI, storage, self-tests
```

The engine is independent of the UI and can be extracted and tested in Node via `module.exports` (`compute`, `julian`, `gmst`, `moonTrop`, `tropical`, `ascFromRamc`, `varga`, `dashas`, `ayanamsa`, `tzOffsetMin`).

## Validation

Built-in self-tests (shown at the bottom of the page):

- Julian Day for J2000
- GMST against Meeus example 12.b
- Sun position against Meeus example 25.a
- Moon position against Meeus example 47.a
- Ascendant geometry
- Daśā years sum to 120
- Navāmśa boundaries
- Sidereal Sun at J2000
- Daśā continuity and 120-year length
- Asia/Kolkata offset = +330 minutes

## Roadmap

Not yet implemented:

- SQLite-WASM with IndexedDB persistence (currently `localStorage`)
- Web Worker for heavy calculations, and PWA/service worker
- Yogas, dṛṣṭi (aspects), Sade Sati, transits (gochara)
- Aṣṭakūṭa Guṇa Milan and Maṅgala Doṣa
- Remaining vargas (D4, D16, D20, D24, D27, D30, D40, D45, D60)
- East Indian chart style
- City search/geocoder
- Alternative ayanāṃśas and house systems
- Print stylesheet and PDF export
- Reference-chart regression suite against Swiss Ephemeris

## Privacy

All calculation and storage happen locally. Saved profiles live in the browser's `localStorage`. Clear them with the **Delete** buttons or by clearing site data.

## Credits

- Algorithms: Jean Meeus, *Astronomical Algorithms*
- Planetary elements: JPL, *Keplerian Elements for Approximate Positions of the Major Planets*
- Domain model: Vedic astrology guide and architecture notes supplied with the project

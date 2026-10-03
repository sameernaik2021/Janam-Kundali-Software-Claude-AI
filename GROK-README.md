# Janam Kundali — Local-First Vedic Birth Chart Application

A production-quality **JavaScript + HTML + CSS** Janam Kundali (Vedic Birth Chart) web application that runs entirely on the user's device.

## Features

- **Accurate core calculations**
  - Julian Day, Lahiri / Raman / KP ayanāṃśa
  - Sidereal planetary positions (Sun & Moon high accuracy; other planets approximate)
  - Lagna (Ascendant)
  - Whole-sign & Equal house systems
  - 27 Nakṣatras + Pada
  - Vimśottarī Mahādaśā timeline with balance at birth
  - Divisional charts: D1 (Rāśi), D9 (Navāṃśa), D10 (Daśāṃśa)

- **Interactive UI**
  - North Indian (diamond) & South Indian chart styles
  - Detailed planetary table
  - Dasha timeline + table
  - Responsive design (mobile → desktop)
  - Dark / light theme
  - Print / PDF friendly

- **Local-first storage**
  - SQLite via `sql.js` (WASM) persisted in `localStorage`
  - Preferences in `localStorage`
  - No backend required
  - Offline capable

## Architecture (layered)

```
HTML/CSS (Presentation)
    ↓
UI Controllers / Components
    ↓
KundaliService (Application)
    ↓
Domain + Calculation Engine
    ↓
Repository (SQLite WASM + localStorage)
```

## How to run

1. Open `index.html` in a modern browser (Chrome, Firefox, Edge, Safari).
2. Or serve the folder with any static server:
   ```bash
   npx serve .
   # or
   python -m http.server 8080
   ```
3. Enter birth details → Generate Kundali.

> **Note:** Because the app loads `sql.js` from a CDN, an internet connection is needed on first load. After that it can work offline (service worker can be added for full PWA).

## Calculation accuracy notes

| Body       | Method                          | Typical accuracy |
|------------|---------------------------------|------------------|
| Sun        | Meeus Ch.25                     | ~0.01°           |
| Moon       | Meeus Ch.47 (truncated)           | ~1′              |
| Planets    | Mean elements + equation of centre | ~0.3–0.8°     |
| Lagna      | Sidereal time + obliquity       | ~1–2′            |
| Ayanāṃśa   | Lahiri polynomial               | ~0.01° (1900–2100) |
| Dasha      | Exact proportional from Moon nakṣatra | Exact for given Moon |

For professional arc-second charts, integrate Swiss Ephemeris (`@swisseph/browser` or similar). The architecture is designed so the calculation engine can be swapped without touching the UI.

## Project structure

```
janam-kundali/
├── index.html
├── css/
│   ├── main.css
│   ├── forms.css
│   ├── kundali-chart.css
│   ├── responsive.css
│   └── print.css
├── js/
│   ├── app.js
│   ├── config/constants.js
│   ├── calculations/
│   │   ├── julianDay.js
│   │   ├── ayanamsa.js
│   │   ├── planets.js
│   │   ├── ascendant.js
│   │   ├── houses.js
│   │   ├── nakshatra.js
│   │   ├── dasha.js
│   │   └── divisionalCharts.js
│   ├── services/KundaliService.js
│   ├── storage/SQLiteDatabase.js
│   └── ui/KundaliChart.js
└── README.md
```

## Privacy

All birth data and calculated charts remain on the user's device. Nothing is sent to any server.

## Disclaimer

Astronomical positions are calculated mathematically. Traditional Jyotiṣa interpretations are cultural/symbolic systems and have not been established as scientifically predictive of personality or events. This application is for educational, cultural, and personal study purposes.

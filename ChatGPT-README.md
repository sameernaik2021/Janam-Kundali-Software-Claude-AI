# Janam Kundali — Local First v1.0.0

A local-first Vedic birth-chart web application based on the supplied architecture and Janam Kundali specifications.

## Included
- Birth date/time/place, latitude/longitude and IANA timezone
- Lahiri, Raman and Krishnamurti sidereal settings
- Whole Sign, Equal and Placidus house selections
- Swiss Ephemeris browser/WASM calculation layer with Moshier fallback
- D1/Rashi and deterministic D9/Navamsa presentation
- Nakshatra/pada and Vimshottari Mahadasha
- Rule-based structural patterns
- SVG chart visualization
- SQLite WASM persistence with localStorage fallback
- JSON export and print/PDF workflow
- Responsive UI and deterministic tests

## Run
```bash
npm install
npm run dev
```
Use the Vite URL; do not open `index.html` directly with `file://`.

## Test
```bash
npm test
npm run build
```

## Important scope
This is a production-oriented v1 implementation, not a claim of universal agreement across every Jyotisha school. A full reference-chart corpus should be used for astronomical conformance testing before operational deployment. Yoga interpretation is intentionally limited to explicit structural rules implemented in the engine.

## Licensing
Review the licenses of the Swiss Ephemeris browser package and its ephemeris data before redistribution or commercial deployment. Pin/vendor dependencies for production rather than relying on mutable CDNs.

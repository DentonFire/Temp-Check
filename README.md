# Temp Check

Live Denton conditions and exactly what to do about them, summer and winter. A year-round public safety tool from the Denton Fire Department, Community Risk Reduction.

**Live tool:** https://dentonfire.github.io/Temp-Check/

---

## What it does

Temp Check pulls the current temperature, humidity, heat index, and wind chill for Denton from the National Weather Service and turns those numbers into specific actions a resident can take right now.

- **Reads live conditions** the moment it loads, for Denton by default. A "Use my location" button or a ZIP entry localizes it.
- **Picks the season on its own.** A heat index of 80 or higher opens heat mode. A temperature or wind chill below 40 opens cold mode. In between, it shows a short "no action needed" note.
- **Generates the right actions** for the exact risk band, in four blocks: You, Kids and neighbors, Pets, and Outdoor work in heat; You, Home and pipes, Heat safely, and Pets and neighbors in cold. A red strip lists what is a 911 call.
- **Finds the nearest open sites.** It sorts the City of Denton cooling and warming stations by distance and gives one-tap directions. The label switches between "Cooling stations near you" and "Warming stations near you" with the season.
- **Shares easily.** A QR button puts the tool on anyone's phone, and the page is built to save to a home screen.

The tool never claims a station is "open now." Stations open only during extreme weather and the City announces openings about a day ahead, so Temp Check always links out to the City page for live hours and status.

---

## How it works

This is a single static HTML file. There is no backend, no build step, and no API key.

- **Live data:** the browser calls the National Weather Service API (`api.weather.gov`), which is free, public, CORS-enabled, and needs no key. Temp Check reads the nearest station's latest observation. If the heat index or wind chill is not reported, it calculates the value from temperature, humidity, and wind using the official NWS formulas.
- **Location:** the "Use my location" button uses the browser geolocation prompt. On a phone this asks the user for permission. Geolocation requires HTTPS, which GitHub Pages provides. A Denton-area ZIP entry is the fallback.
- **If the feed is down:** the tool stays usable. A manual slider lets anyone see the actions for any temperature.
- **Styling:** Tailwind via CDN and the Inter font, matching the rest of the DFD tool family (After the Fire, the newsletter, and the others). The navy, yellow, and red are the department's Knox Box palette.
- **Motion** respects the reduced-motion setting in the operating system.

The safety math is the official NWS heat index (Rothfusz) and wind chill formulas, checked against the published NWS charts.

---

## Deploy on GitHub Pages

1. Create a repository named `Temp-Check` under the `DentonFire` organization.
2. Rename `Temp_Check.html` to `index.html` and add it to the repository root.
3. Go to **Settings > Pages**, set the source to deploy from the `main` branch, root folder, and save.
4. The tool goes live at `https://dentonfire.github.io/Temp-Check/` within a minute or two.
5. Set the `TOOL_URL` value in the script to that exact address so the QR code and share links resolve correctly, then commit.

---

## Maintenance

Almost nothing here goes stale. The few things that can are all in the `<script>` block, grouped at the top and easy to find by name.

- **Station list and hours (`SITES`).** Addresses rarely change, but the City updates its cooling and warming page each season. Check it against the City list before summer and before winter. The City page is the source of truth; this list is a convenience for distance and directions. Update at `https://www.cityofdenton.com/470/WarmingCooling-Stations`.
- **ZIP coverage (`ZIPS`).** Covers 76201 through 76210 plus Krum, Aubrey, Argyle, Pilot Point, and Ponder. Add entries if outreach reaches wider.
- **Action copy (`heatActions`, `coldActions`).** Plain safety guidance that stays stable year to year. **The cold-mode content is a working draft pending a January review.** Update it before the first hard freeze and remove this note once it is final.
- **Risk thresholds.** Heat mode starts at a heat index of 80 (`decideMode`). Cold mode starts below 40 (`decideMode`), which matches the tool's "warming stations" switch. The heat bands live in `HEAT_BANDS`; the cold bands live in `pickColdBand`. Both align with the NWS scales.
- **Share URL (`TOOL_URL`).** Must match the deployed address for the QR code to work.

Station coordinates are approximate, so the distances are a sort order, not survey grade. The directions links use the real street address, so navigation is always correct regardless of the coordinates.

---

## Sources

- National Weather Service heat index and wind chill scales, and the live feed at `api.weather.gov`.
- Centers for Disease Control and Prevention, extreme heat and extreme cold guidance.
- OSHA and NIOSH water, rest, and shade standard for outdoor work.
- City of Denton warming and cooling stations.

Temp Check is a preparedness tool, not a substitute for 911. In an emergency, call 911.

---

## Maintainer

Community Risk Reduction, Denton Fire Department
Non-emergency: (940) 349-8840
https://www.dentonfire.com

# Marthapoint

**An interactive map quiz for Indonesia's 38 provinces — learn the geography, meet the capitals, and raise awareness about forest and land fires (_karhutla_).**

[![License: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Status: Beta](https://img.shields.io/badge/status-beta%201.0-orange.svg)](#roadmap)
[![Made with HTML](https://img.shields.io/badge/HTML-single--file-e34f26?logo=html5&logoColor=white)](#tech-stack)
[![Map: D3.js](https://img.shields.io/badge/map-D3.js%20%2B%20TopoJSON-f9a03c?logo=d3&logoColor=white)](#tech-stack)

> **Draft README** — replace the `YOUR-USERNAME` placeholders (demo link, badges, clone URL) before publishing.
>
> Developed by **Episteme Indonesia** · © 2026 Episteme Indonesia · Licensed under the [Apache License 2.0](LICENSE).

---

## Table of contents

- [Overview](#overview)
- [Live demo](#live-demo)
- [Screenshots](#screenshots)
- [Features](#features)
- [Game modes](#game-modes)
- [Scoring and tiers](#scoring-and-tiers)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Tech stack](#tech-stack)
- [Project structure](#project-structure)
- [Getting started](#getting-started)
- [How to play](#how-to-play)
- [Save data](#save-data)
- [Map data pipeline](#map-data-pipeline)
- [Attribution and third-party notices](#attribution-and-third-party-notices)
- [License](#license)
- [Contributing](#contributing)
- [Roadmap and known limitations](#roadmap-and-known-limitations)
- [Credits](#credits)
- [Ringkasan (Bahasa Indonesia)](#ringkasan-bahasa-indonesia)

---

## Overview

**Marthapoint** is a small, dependency-free static web app that turns learning Indonesian geography into a game. It is built as two self-contained HTML pages:

| File | Role |
| --- | --- |
| `index.html` | Landing page: mission, learning goals, the _karhutla_ (forest and land fire) awareness banner, and the call to action that launches the quiz. |
| `mtpo-updated.html` | The quiz app: a real vector map of the 38 provinces, three game modes, scoring, player tiers, profiles, portable save files, Wikipedia links, and a review list for missed regions. |

The interface is in **Indonesian**, the map is rendered from real province boundaries, and no build step, package manager, or backend is required.

The project has two goals beyond the quiz itself:

1. **Geography education** — help people recognise all 38 provinces, their capitals, and the largest city on each major island group.
2. **Ecological awareness** — keep the annual forest and land fires in Kalimantan, Sumatra, and Papua visible, and make sharing the campaign a one-click action.

---

## Live demo

- **Play:** `https://YOUR-USERNAME.github.io/marthapoint/mtpo-updated.html`
- **Landing page:** `https://YOUR-USERNAME.github.io/marthapoint/`

> If GitHub Pages is not enabled yet, use **Settings → Pages → Deploy from a branch → `main` / `root`**.

---

## Screenshots

<!-- Replace these placeholders with real screenshots (docs/landing.png, docs/game.png). -->

| Landing page | Quiz workspace |
| --- | --- |
| _add `docs/landing.png`_ | _add `docs/game.png`_ |

---

## Features

### Learning and gameplay

- **Real vector map of 38 provinces** rendered with D3.js and TopoJSON/GeoJSON geometry — not a static image and not a grid of tiles.
- **Three game modes**: province names, capital cities, and the largest city of each major island group.
- **Island/region filters**: Sumatra, Java, Kalimantan, Sulawesi, Bali & Nusa Tenggara, Maluku, and Papua, or all 38 provinces.
- **Immediate visual feedback**: green for a first-try answer, amber for a retry, red for a revealed answer, plus a pulsing highlight when the quiz reveals a location.
- **Three attempts per question** before the answer is revealed, so a wrong guess is a learning moment rather than an instant fail.
- **Wikipedia hand-off**: every correctly identified province unlocks direct links to its Indonesian Wikipedia article and the article for its capital.
- **Review list**: missed, skipped, or retried regions are collected in a “Pelajari Kembali” panel; tap a card to locate it on the map, or replay only those regions.
- **Live HUD**: progress, accuracy, elapsed time, streaks, and current target, designed to stay readable on small screens.
- **Map camera controls**: zoom in/out, one-click island presets, reset, and a Java shortcut.
- **Mobile-aware labels**: province abbreviations are hidden on phones in most modes to keep the challenge honest, and shown in the capitals mode where they are needed.

### Profiles and persistence

- **Multiple local profiles** with rename, switch, and delete.
- **XP, sessions played, and best time** stored per profile in `localStorage`.
- **Five player tiers**, from _Nolep_ to _Kagak Bisa Diem_.
- **Portable save files**: export the whole profile set as `.json` and import it on another device or browser.
- **Defensive import**: imported JSON is validated and sanitised before it is written to storage.

### Awareness campaign (_karhutla_)

- A **full-screen ecological warning** on every load of the quiz, dismissed with an explicit “I Understand & Will Stay Aware” action.
- A **red/amber gradient accent** for Kalimantan provinces on the map, with a legend entry.
- A dedicated **share button** in the footer using the Web Share API, with clipboard and manual-copy fallbacks.
- A matching **emergency banner** and campaign link on the landing page.

### UX and engineering details

- Fully **responsive**, touch-friendly, and safe on narrow screens.
- **No build step, no framework, no backend** — clone and open.
- **Graceful map fallback**: three remote geometry sources are tried in order; if all fail, a simplified cartographic fallback is drawn so the quiz still works.
- **Keyboard-first option** for desktop play.
- **Zero tracking**: no analytics, no cookies, no accounts. All progress stays in the browser.

---

## Game modes

| Mode | Prompt | Answer | Pool |
| --- | --- | --- | --- |
| **1. Provinsi** | “Provinsi _<name>_” | Click that province on the map | All 38 provinces (or the selected island group) |
| **2. Ibu Kota** | “Ibu Kota: _<capital>_” | Click the province whose capital it is | All 38 provinces (or the selected island group) |
| **3. Kota Terbesar** | “_<city>_ (_<island group>_)” | Click the province where that island group’s largest city is located | One province per major group (7 total) |

The largest-city mode covers Medan (Sumatra), Jakarta (Java), Denpasar (Bali & Nusa Tenggara), Samarinda (Kalimantan), Makassar (Sulawesi), Ambon (Maluku), and Jayapura (Papua).

---

## Scoring and tiers

| Action | XP |
| --- | --- |
| Correct on the **first** attempt | **+100**, plus a streak bonus of `10 × streak`, capped at **+50** |
| Correct on the **2nd or 3rd** attempt | **+50** (streak resets) |
| Hint (highlights the island) | **−25** from the current session (never below 0) |
| Wrong guess | 0 XP; streak resets |
| Skip / third wrong guess | 0 XP, answer revealed, region added to the review list |

Accuracy is `correct hits / total map clicks`. The **best time** record is only saved for a complete, unfiltered, 38-province run in which every province was answered on the first attempt.

| Tier | XP range |
| --- | --- |
| 1. Nolep | 0 – 999 |
| 2. Kang Ngider | 1,000 – 2,999 |
| 3. Kang Ngelayap | 3,000 – 6,999 |
| 4. Kang Travelling | 7,000 – 14,999 |
| 5. Kagak Bisa Diem 👑 | 15,000+ |

---

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `Space` | Start / restart the quiz |
| `S` | Skip the current question |
| `H` | Island hint (−25 XP) |
| `A` | Toggle province abbreviations (desktop only) |
| `1` / `2` / `3` | Switch game mode |
| `J` | Zoom to Java |
| `R` | Reset the map view |
| `P` | Open profile and save management |
| `?` | Open the tier guide |
| `Esc` | Close any open modal |

When the ecological warning is visible, `Enter` or `Space` dismisses it.

---

## Tech stack

| Layer | Choice |
| --- | --- |
| Markup & styling | HTML5 + [Tailwind CSS Play CDN](https://tailwindcss.com/) + a small amount of custom CSS |
| Map rendering | [D3.js v7](https://d3js.org/) (`geoMercator`, `geoPath`, zoom, transitions) |
| Geometry decoding | [topojson-client v3](https://github.com/topojson/topojson-client) |
| Typography | [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) and [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) via Google Fonts |
| Persistence | `localStorage` + JSON file export/import (Blob, FileReader) |
| Sharing | Web Share API, with Clipboard API and `prompt()` fallbacks |
| Hosting | Any static host (GitHub Pages, Netlify, Vercel, or a plain folder) |

There is **no package.json, bundler, or server-side code**. Tailwind is loaded from its CDN for simplicity; see [Roadmap](#roadmap-and-known-limitations) for the production caveat.

---

## Project structure

```text
marthapoint/
├── index.html          # Landing page and campaign entry point
├── mtpo-updated.html   # Quiz application (map, modes, profiles, save system)
├── README.md
└── LICENSE
```

`index.html` links to the quiz with a relative link (`mtpo-updated.html`), so both files must stay in the same directory. Renaming the quiz file is fine as long as the link on the landing page is updated too.

---

## Getting started

### Run it locally

1. Download or clone the repository.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox, or Safari).
3. Click **Mulai Bermain Sekarang** to open the quiz.

No installation is required. If your browser blocks the remote map download when the page is opened from `file://`, serve the folder with any static file server, for example:

```bash
# Python 3
python -m http.server 8000

# or Node.js
npx serve .
```

Then visit `http://localhost:8000/`.

> The app needs an internet connection the first time it loads: Tailwind, D3, TopoJSON, Google Fonts, and the province geometry are fetched from CDNs and public repositories. The geometry is cached by the browser like any other asset; profile data is always local.

### Deploy to GitHub Pages

1. Push both HTML files, `README.md`, and `LICENSE` to a public repository.
2. Go to **Settings → Pages**.
3. Choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
4. Open `https://YOUR-USERNAME.github.io/marthapoint/`.

---

## How to play

1. Read and dismiss the ecological warning.
2. Pick a game mode: **Provinsi**, **Ibu Kota**, or **Kota Terbesar**.
3. Optionally restrict the round to one island group, then press **Mulai**.
4. Click the province on the map that matches the prompt in the sticky HUD.
5. Use the hint button if you are stuck; it zooms to the correct island for a small XP cost.
6. Finish the round to bank your XP. Open **Pelajari Kembali** to revisit anything you missed, or **Uji Ulang** to replay only those regions.
7. Manage profiles and download a `.json` backup from the profile button (person icon) or the **Unduh Save** button under the map.

---

## Save data

Progress is stored under the key `marthapoint_episteme_save_v2`. The exported file has this shape:

```json
{
  "activeProfileId": "default",
  "profiles": {
    "default": {
      "id": "default",
      "name": "Tamu Nusantara",
      "totalXP": 0,
      "gamesPlayed": 0,
      "bestTimeAllProvinces": null
    }
  }
}
```

- `totalXP` and `gamesPlayed` accumulate across sessions.
- `bestTimeAllProvinces` is stored in **seconds** and is `null` until a perfect unfiltered 38-province run is completed.
- Imported files are validated: profile IDs are sanitised, names are trimmed to 24 characters, and numeric fields are coerced or rejected. Invalid files show an error toast instead of overwriting your data.
- Clearing browser site data deletes local profiles — export a backup first if progress matters to you.

---

## Map data pipeline

The quiz does not ship province geometry inside the HTML. At runtime it tries, in order:

1. A TopoJSON file of the 38 provinces.
2. A GeoJSON file of the same dataset.
3. A Gist-hosted GeoJSON fallback.
4. A simplified cartographic fallback drawn from province centre points.

All sources point to the public [denyherianto/indonesia-geojson-topojson-maps-with-38-provinces](https://github.com/denyherianto/indonesia-geojson-topojson-maps-with-38-provinces) dataset. Because the six Papua provinces were reorganised in 2022–2023, the code also splits and classifies multi-polygon Papua features by centroid so that **Papua Barat Daya, Papua Barat, Papua Tengah, Papua Pegunungan, Papua Selatan, and Papua** each map to the correct quiz entry. Property-name aliases (Indonesian and English) and BPS province codes are used to match features to provinces.

If you fork the project for long-term or offline use, consider vendoring the geometry into the repository (with its upstream licence and attribution) and replacing the remote URLs with local paths.

---

## Attribution and third-party notices

This repository's **original code and content** are licensed under Apache 2.0. Third-party components keep their own terms:

- **Province geometry** — [denyherianto/indonesia-geojson-topojson-maps-with-38-provinces](https://github.com/denyherianto/indonesia-geojson-topojson-maps-with-38-provinces). Fetched at runtime and **not** relicensed by this project; check the upstream repository for its licence and attribution requirements before redistributing the data.
- **D3.js** — ISC licence, © Mike Bostock.
- **topojson-client** — ISC licence, © Mike Bostock.
- **Tailwind CSS** — MIT licence, © Tailwind Labs.
- **Plus Jakarta Sans** and **JetBrains Mono** — SIL Open Font License, served by Google Fonts.
- **Wikipedia** — the app only links to `id.wikipedia.org`; no article text is copied. Wikipedia content is available under CC BY-SA 4.0 and linked pages carry their own attribution.
- **Province names, capitals, and city data** — factual public information, compiled for educational use.

Marthapoint is an independent educational project by Episteme Indonesia and is **not** affiliated with Wikipedia, the Wikimedia Foundation, or any government body. Province boundaries are illustrative and must not be used for surveying, legal, or administrative purposes.

If you add a `NOTICE` file, keep the upstream map-data attribution there as well.

---

## License

Copyright 2026 Episteme Indonesia.

Licensed under the **Apache License, Version 2.0**. You may use, modify, and distribute this project, including commercially, provided you keep the copyright and licence notices and state significant changes. The licence also includes an express patent grant and a trademark restriction.

See [LICENSE](LICENSE) for the full text, or read it at <https://www.apache.org/licenses/LICENSE-2.0>.

Unless required by applicable law or agreed to in writing, software distributed under the licence is provided **“AS IS”**, without warranties or conditions of any kind.

---

## Contributing

Contributions that improve accuracy, accessibility, or the learning experience are welcome.

1. Fork the repository and create a branch: `git checkout -b feature/your-change`.
2. Keep the two-file, no-build architecture unless there is a strong reason to change it.
3. Test both pages on desktop and on a narrow mobile viewport (≤ 640 px), including the modal flow and the map fallback.
4. If you change province data, verify the name, capital, island group, and Wikipedia slug for **all 38** entries.
5. Open a pull request describing the change and, for data fixes, include a source.

Please do not commit personal save files, API keys, or tracking scripts. By submitting a contribution you agree that it may be distributed under the Apache 2.0 licence.

### Reporting issues

Include the browser and version, the game mode, the province involved, and steps to reproduce. Screenshots of the HUD and map state are especially helpful.

---

## Roadmap and known limitations

- [ ] Self-host Tailwind (the Play CDN is intended for prototyping, not production) and pin dependency versions.
- [ ] Vendor the province geometry locally for offline use and deterministic builds.
- [ ] Add a formal test pass for the 38-province dataset and the Papua feature-splitting logic.
- [ ] Persist the dismissed state of the ecological warning per session rather than showing it on every load (a deliberate awareness trade-off today).
- [ ] Add English UI strings alongside Indonesian.
- [ ] Improve screen-reader labelling for SVG provinces and modal focus trapping.
- [ ] Consider a service worker for full offline play.
- [ ] Add screenshots and a short demo video to this README.

Current limitations: progress is per-browser only, the first load requires network access, and the map geometry is only as accurate as the upstream dataset.

---

## Credits

- **Design, code, and content:** Episteme Indonesia.
- **Map geometry:** [denyherianto](https://github.com/denyherianto) and contributors to the Indonesian GeoJSON/TopoJSON dataset.
- **Open-source libraries:** D3.js, topojson-client, Tailwind CSS, and the Google Fonts team.
- **Knowledge links:** Indonesian Wikipedia and its contributors.

© 2026 Episteme Indonesia. _Kenal lebih jauh, yuk!_

---

## Ringkasan (Bahasa Indonesia)

**Marthapoint** adalah kuis peta interaktif untuk mengenal **38 provinsi Indonesia**, ibu kota, dan kota terbesar di setiap gugus pulau. Aplikasi ini terdiri dari dua berkas HTML tanpa proses build: `index.html` sebagai halaman pembuka dan `mtpo-updated.html` sebagai permainan.

Fitur utama: peta vektor asli (D3 + TopoJSON), tiga mode permainan, filter pulau, sistem XP dan lima tier pemain, banyak profil dengan penyimpanan lokal, ekspor/impor save `.json`, tautan Wikipedia, daftar “Pelajari Kembali”, pintasan keyboard, serta kampanye kesadaran **karhutla** lewat peringatan darurat dan tombol bagikan.

Cukup buka `index.html` di browser modern. Lisensi kode: **Apache 2.0** (© 2026 Episteme Indonesia); data peta dan pustaka pihak ketiga mengikuti lisensi masing-masing.

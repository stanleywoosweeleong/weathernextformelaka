# Melaka 农友天气 · WeatherNext

A single-file Progressive Web App (PWA) delivering farm weather forecasts for
plantation locations around Melaka. Bilingual interface (中文 / English) with
optional AI-generated farming briefings.

Part of the WeatherNext family of per-region agricultural weather builds.

---

## Live app

Once GitHub Pages is enabled, the app is served at:

```
https://stanleywoosweeleong.github.io/weathernextformelaka/
```

Open that link on a phone and use **"Add to Home Screen"** to install it as an
app. It works offline after the first visit (service-worker cached).

---

## Seeded location

On first launch the app seeds these farms. They are auto-favourited and can be
renamed, edited, or deleted freely afterwards. Add as many more farms as you
like from inside the app.

| English | 中文 | Coordinates |
|---|---|---|
| Machap Baru | 马接峇鲁 | 2.38000, 102.35056 |
| Durian Tunggal Dam | 榴槤洞葛水坝 | 2.35370, 102.31610 |

The app also seeds a default user display name (**Eric Goh**), which stays
editable via **Edit Name** in the app.

---

## API key — bring your own (important)

This app **does not ship with an embedded API key.** AI features (the farming
briefings) are powered by Google's Gemini API, and each user supplies their
own free key.

To enable the AI briefing:

1. Visit https://aistudio.google.com/app/apikey
2. Click **"Create API key"** — it's free.
3. In the app, open the **API Key** modal and paste the key (starts with `AIzaSy...`).

The key is stored only in that device's browser (`localStorage`) and is never
uploaded anywhere or committed to this repo. The core weather forecast works
without a key — only the AI briefing needs one.

**Why no embedded key:** a key bundled into a public web app is visible to
anyone who opens browser DevTools. Google actively scans public repositories
and will automatically disable a key found exposed, which would break the
feature for everyone at once. Keeping keys per-user and per-device avoids that
failure mode entirely.

**Recommended for users:** restrict your key in Google Cloud Console
(Application restrictions -> Websites) to `stanleywoosweeleong.github.io/*`,
and limit it to the Generative Language API. This reduces abuse risk if the
key is ever scraped.

---

## Deploying

All 7 files live in the **repository root** — the service worker and manifest
use relative `./` paths, so a root deploy works with no changes.

```
index.html            — the app (single file: HTML + CSS + JS)
manifest.json         — PWA metadata
sw.js                 — service worker (offline cache)
icon-512.png          — app icon 512x512
icon-192.png          — app icon 192x192
apple-touch-icon.png  — iOS home-screen icon 180x180
favicon-32.png        — browser tab icon 32x32
```

To enable GitHub Pages: **Settings -> Pages -> Source: Deploy from branch ->
`main` / `root`.** Pages serves over HTTPS automatically, which the service
worker requires.

### Updating the app

The service worker caches the app shell. When you push changes, bump the
`CACHE_VERSION` string at the top of `sw.js` so users receive the update on
their next visit. The current value is:

```
wnext-weathernextformelaka-202605280050
```

---

## Tech notes

- **Weather data:** Open-Meteo API (no key required, network-first with cache fallback).
- **AI model:** `gemini-2.5-flash` via the Generative Language API.
- **Storage namespace:** `weathernextformelaka__*` keys in `localStorage`,
  isolated from other WeatherNext regional builds so data never collides.
- **Cloud sync:** Firebase, namespaced under `appId: wnext-ag-v41-weathernextformelaka`.
- **Offline:** full app shell + last-fetched weather cached by the service worker.

---

## App icon

The icon shows the historic **A'Famosa gate (Porta de Santiago)** — Melaka's
landmark Portuguese fortress remnant — set behind an orchard with a farmer,
matching the WeatherNext family's flat-colour illustration style.

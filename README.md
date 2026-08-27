# bvs-web

Big Vibe Studios site — a builder profile and distribution point for everything
built here. Static, hosted on Firebase Hosting under the `bigvibestudios-b9839`
project, hosting site target `bvs-web`.

## Deploy

```
firebase deploy --only hosting:bvs-web
```

## Pages

| Path             | File                | What it is                                    |
| ---------------- | ------------------- | --------------------------------------------- |
| `/`              | `index.html`        | Profile home — who, what, the featured builds  |
| `/builds`        | `builds.html`       | Full catalog: every project, status, how to get it |
| `/resume`        | `resume.html`       | Résumé (prints to PDF cleanly)                 |
| `/tracker`       | `tracker.html`      | Tracker — direct APK download + iOS invite form |
| `/stock-alerts`  | `stock-alerts.html` | stock-alerts — source download + run instructions |
| `/theyo`         | `theyo.html`        | The Yo — store links                           |
| `/snappled`      | `snappled.html`     | Snappled — beta access                         |
| `/contact`       | `contact.html`      | Contact form                                   |

Legal pages (`/privacy`, `/terms`, `/child-safety`, `/delete-account`) and the
Snappled equivalents under `/snappled/` are unchanged.

## Structure

- `public/assets/site.css` — shared stylesheet for the profile pages. The older
  legal and app pages still carry their own inline CSS.
- `public/downloads/` — release binaries and source archives served directly.
  Empty in the repo; drop files in before deploying (see below).
- `public/.well-known/` — iOS/Android deep-link verification files
- `public/theyo/` — legacy `/theyo/*` paths still served here for backwards compat
- `public/snappled/` — Snappled legacy pages

## Publishing a download

Direct downloads live in `public/downloads/` with fixed filenames so the pages
never need editing between releases:

| File                            | Linked from      |
| ------------------------------- | ---------------- |
| `tracker-latest.apk`            | `/tracker`       |
| `stock-alerts.zip`              | `/stock-alerts`  |

Both pages do a `HEAD` request on their file at load. If it isn't there, the
download button turns into a "request it" form instead of handing out a link
that 404s — so it is safe to deploy before the files exist.

`firebase.json` sets the right `Content-Type` for `.apk` and `.zip`, and marks
them `must-revalidate` so a new upload is picked up immediately.

### Building the Tracker APK

```
cd C:\dev\tracker
eas build --profile preview --platform android
```

Download the artifact from the EAS build page, rename it
`tracker-latest.apk`, and drop it in `public/downloads/`.

### Packaging stock-alerts

Zip the source **without** the environment file, caches, or personal rating
data — `.env`, `.user_ratings.json*`, `.muted.json`, `.skipped.json`,
`.sector_*.json`, `.fundamental_cache.json`, `reports/`, and `__pycache__/`.

# bvs-web

Legacy Big Vibe Studios static website. Hosted on Firebase Hosting under
the `bigvibestudios-b9839` project, hosting site target `bvs-web`.

## Deploy

```
firebase deploy --only hosting:bvs-web
```

## Structure

- `public/` — static HTML pages
- `public/.well-known/` — iOS/Android deep-link verification files
- `public/theyo/` — legacy `/theyo/*` paths still served here for backwards compat
- `public/snappled/` — Snappled legacy pages

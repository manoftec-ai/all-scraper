# All Scraper

Scraper providers for the **Vega app** — 61 modules that pull catalog, metadata,
episode lists and playable stream links out of streaming websites.

Forked from [`Zenda-Cross/vega-providers`](https://github.com/Zenda-Cross/vega-providers)
(upstream: `Zenda-Cross`). This fork is maintained as part of the **All Scraper**
project and is where upgrades land.

<a href="https://www.buymeacoffee.com/ZendaX" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me a Coffee" style="height: 60px !important;width: 217px !important;" ></a>

---

## What a provider is

A provider is a folder under `providers/` with up to six TypeScript files:

| File | Required | Purpose |
|---|---|---|
| `catalog.ts` | yes | the category list ("Trending", "Popular Movies") |
| `posts.ts` | yes | list items + search results |
| `meta.ts` | yes | title, synopsis, poster, IMDb ID, `linkList` |
| `stream.ts` | yes | final `.m3u8` / `.mp4` + quality + subtitles + headers |
| `episodes.ts` | no | dynamic season/episode loading |
| `settings.ts` | no | user-facing settings panel |

Types always come from `../types`. Providers run in a sandbox with **no module
resolution** — only the injected `providerContext.axios` / `.cheerio` /
`.commonHeaders` / `.kvStore` / `.Aes` are available.

## Shared host resolvers

`providers/extractors/` holds 16 reusable resolvers that turn an obfuscated page
link into a playable stream: `vegadrive`, `hubcloud`, `gofile`, `gdflix`,
`buzzheavier`, `vidbolt`, `vidstuck`, `zcloud`, `zypter`, `abbasy`, `cinejoy`,
`cineverse`, `iqst`, `movy`, `supeVideo`, `transferit`.

## Build & develop

```bash
npm install
npm run build            # esbuild -> CJS + terser -> dist/
npm run build:dev        # unminified (use while learning)
npm run dev              # Express dev server on :3001
npm run dev:auto         # watch + rebuild + serve
npm run watch            # nodemon rebuild on provider changes
npm test                 # full provider suite (zod-validated)
npm run test:provider -- <name> <fn> --rebuild
```

`dist/` is committed so the Vega app can consume it straight from a CDN.

## Daily automation

- **`.github/workflows/check-urls.yml`** — checks every provider domain in
  `urls.json`, follows redirects, rewrites the file, commits and pings Discord.
  These sites change domains constantly; this is what keeps the pack alive.
- **`.github/workflows/test-providers.yml`** — builds and runs the test suite daily.

## Contributing

See `AGENTS.md` for the full authoring contract — API signatures, `linkList`
rules, sandbox restrictions, testing and bundling.

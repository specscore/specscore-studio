# specscore-studio

Landing page for [SpecScore Studio](https://specscore.studio) — the first-party authoring surface for the open [SpecScore](https://specscore.md) spec format.

Astro static site, deployed to Cloudflare Pages.

## Quick start

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # static output to dist/
npm run preview  # serve the built dist/ locally
npm run check    # astro check (TypeScript + Astro diagnostics)
```

## Stack

| Layer       | Choice                          | Notes                                                         |
|-------------|---------------------------------|---------------------------------------------------------------|
| Framework   | Astro 6                         | Static site, single-page landing for v1                       |
| Styling     | Plain CSS                       | `src/styles/global.css` — dark theme, accent green `#3ddc91`  |
| Hosting     | Cloudflare Pages                | `wrangler.jsonc` serves `dist/` with 404-page handling        |
| TypeScript  | strict                          | `npm run check` for diagnostics                               |

## Deploy

Cloudflare Pages picks up the build from `dist/` per `wrangler.jsonc`. The deploy is triggered by pushing to `main` (assuming the Cloudflare Pages GitHub integration is configured on the `specscore/specscore-studio` repo with build command `npm run build` and output `dist/`).

For manual deploy:

```bash
npx wrangler pages deploy dist
```

## What's on the page

- **Hero** — headline, lede, install snippet for the Claude Code plugin (`/plugin install specstudio@specscore`)
- **What it is** — short explanation of SpecScore (the standard) and SpecScore Studio (the authoring surface)
- **Built on an open standard** — open-format pitch, link to `specscore.md`
- **Footer** — links to source repos, ecosystem repos, and the SpecScore GitHub org

This is the v0 landing page. Future expansions (separate routes for `/install`, `/docs`, `/blog`, the actual web editor at `/app` or similar) follow the same Astro pattern.

## Related

- [`specscore/specstudio-skills`](https://github.com/specscore/specstudio-skills) — the Claude Code plugin this page promotes
- [`specscore/specscore-studio-app`](https://github.com/specscore/specscore-studio-app) — the `/app/` web client
- [`specscore/ai-marketplace`](https://github.com/specscore/ai-marketplace) — the marketplace the install command references
- [`specscore.md`](https://specscore.md) — the standard's home, with format spec and docs

## License

MIT — see [LICENSE](LICENSE).

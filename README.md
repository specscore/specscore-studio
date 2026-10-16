# specscore-studio

Landing page for [SpecScore Studio](https://specscore.studio) — the first-party authoring surface for the open [SpecScore](https://specscore.md) spec format.

Astro static site, built by GitHub Actions and deployed to Cloudflare Workers.

<!-- dev-approach:v1 -->
## Our approach to development

We build with our own tooling:

- **[SpecScore](https://specscore.md)** — specify requirements as `SpecScore.md` artifacts
- **[SpecStudio](https://specscore.studio)** — author & manage specs across their lifecycle
- **[inGitDB](https://ingitdb.com)** — store structured data in Git where applicable
- **[DALgo](https://dalgo.io)** — data access layer for Go
- **[cover100.dev](https://cover100.dev)** — drive toward 100% test coverage
- **[DataTug](https://datatug.io)** — query & explore data
<!-- /dev-approach -->

## SpecScore in product development

[DataTug](https://datatug.app) is a product development example of the SpecScore approach: structured requirements and acceptance criteria guide the work. This site describes Studio's authoring surface; it does not imply DataTug uses the Studio editor. [Explore the tools behind DataTug](https://datatug.app/en/under-the-hood/tools-we-built/#specify).

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
| Hosting     | Cloudflare Workers              | `wrangler.jsonc` serves `dist/` with 404-page handling        |
| TypeScript  | strict                          | `npm run check` for diagnostics                               |

## Deploy

[Site build and deploy](.github/workflows/site-deploy.yml) installs from `pnpm-lock.yaml` with `--frozen-lockfile`, runs Astro checks, and builds `dist/` on pull requests, pushes to `main`, and manual runs. The `specscore-studio-deployment` artifact contains a tar archive of the complete `dist/`, `wrangler.jsonc`, and source `REVISION`.

Only pushes to `main` and manual runs on `main` enter the `production` environment. The deployment job downloads that same run's artifact, verifies its revision, and publishes to the `specscore-studio` Cloudflare Worker using pinned Wrangler with `--no-bundle`. It does not reinstall project dependencies or rebuild the site.

Automatic publication is disabled by default. Set the repository variable `CLOUDFLARE_DEPLOY_ENABLED` to `true` only when credentials and the production environment are ready. Every build still saves the artifact while publication is disabled; the run summary distinguishes the saved artifact from a deployment. An operator can download `site.tar.gz` and `site.tar.gz.sha256`, verify and extract them, and publish the exact packaged output with the pinned Wrangler command below, without rebuilding.

Set `EXPECTED_SOURCE_SHA` to the full `head_sha` from the verified GitHub Actions run that produced the downloaded artifact.

```sh
set -eu
: "${EXPECTED_SOURCE_SHA:?Set to the verified GitHub run head_sha}"
mkdir deployment
shasum -a 256 -c site.tar.gz.sha256
tar -xzf site.tar.gz -C deployment
cd deployment
test "$(cat REVISION)" = "$EXPECTED_SOURCE_SHA"
npm exec --yes --package=wrangler@4.120.1 -- wrangler deploy --config wrangler.jsonc --no-bundle
```

The operator supplies `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` through the existing local environment; credentials are not part of the artifact.

Configure `CLOUDFLARE_API_TOKEN` as a secret available to this repository and `CLOUDFLARE_ACCOUNT_ID` as a variable for the existing account. Restrict the `production` environment to `main` and configure any required approvals. After the first GitHub deployment is verified, retire the legacy Cloudflare Git production build trigger so GitHub Actions owns publication.

For a local build matching CI, use `pnpm install --frozen-lockfile && pnpm build`. The pnpm lockfile is the deployment dependency contract.

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

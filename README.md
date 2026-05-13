# PR Review Dashboard

This repository uses [`marcoieni/revisaurus`](https://github.com/marcoieni/revisaurus) to review the latest two pull requests from:

- [`release-plz/release-plz`](https://github.com/release-plz/release-plz)
- [`marcoieni/gigi`](https://github.com/marcoieni/gigi)

Reviews are generated with Kiro CLI and deployed as a static website to GitHub Pages.

## How It Works

The scheduled workflow in [`.github/workflows/revisaurus.yml`](.github/workflows/revisaurus.yml) runs hourly and can also be started manually from the Actions tab.

The Revisaurus configuration lives in [`revisaurus.toml`](revisaurus.toml). It sets `max_pull_requests = 2` globally and per repository so only the latest two PRs from each repository are reviewed.

Generated review state is cached between workflow runs under `.revisaurus/data` so unchanged PR head commits are not reviewed again.

## Build Locally

Use Node.js 24 or newer and pnpm 11. The local scripts clone Revisaurus into `.revisaurus/revisaurus`, install its dependencies, generate the review data, and build the static Astro site into `site-dist`.

```bash
pnpm run revisaurus:install
GITHUB_TOKEN=... KIRO_API_KEY=... pnpm generate
```

To preview the generated site locally:

```bash
pnpm dev
```

The preview server prints the localhost URL, usually `http://127.0.0.1:4321`.

To rebuild the static site from existing `.revisaurus/data/site.json` without fetching or reviewing pull requests again, run `pnpm build`. To try the site without credentials, run `pnpm demo`.

`GITHUB_TOKEN` needs read access to the configured repositories and pull requests. `KIRO_API_KEY` is required by Kiro CLI headless mode. Generated review state is written to `.revisaurus/data`, and the static website output is written to `site-dist`; both are ignored by git.

## TODO

- [x] In the repository settings, go to **Settings -> Pages** and set **Build and deployment -> Source** to **GitHub Actions**.
- [x] Create a Kiro API key for headless CLI use.
- [x] Add the Kiro API key as a repository secret named `KIRO_API_KEY` under a GitHub environment for extra security.
- [x] Confirm that Actions are enabled for the repository.
- [ ] Run the `Revisaurus` workflow manually once from **Actions -> Revisaurus -> Run workflow**.
- [ ] After the first successful run, open the deployed Pages URL from the workflow summary.
- [ ] If reviews hit GitHub API rate limits, add a fine-grained GitHub token secret and update the workflow/action configuration once Revisaurus supports overriding the provider token.

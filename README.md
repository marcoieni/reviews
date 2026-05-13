# PR Review Dashboard

This repository uses [`marcoieni/revisaurus`](https://github.com/marcoieni/revisaurus) to review the latest two pull requests from:

- [`release-plz/release-plz`](https://github.com/release-plz/release-plz)
- [`marcoieni/gigi`](https://github.com/marcoieni/gigi)

Reviews are generated with Kiro CLI and deployed as a static website to GitHub Pages.

## How It Works

The scheduled workflow in [`.github/workflows/revisaurus.yml`](.github/workflows/revisaurus.yml) runs hourly and can also be started manually from the Actions tab.

The Revisaurus configuration lives in [`revisaurus.toml`](revisaurus.toml). It sets `max_pull_requests = 2` globally and per repository so only the latest two PRs from each repository are reviewed.

Generated review state is cached between workflow runs under `.revisaurus/data` so unchanged PR head commits are not reviewed again.

## TODO

- [ ] Create the GitHub repository that will host this project.
- [ ] Push these files to the default branch.
- [ ] In the repository settings, go to **Settings -> Pages** and set **Build and deployment -> Source** to **GitHub Actions**.
- [ ] Create a Kiro API key for headless CLI use.
- [ ] Add the Kiro API key as a repository secret named `KIRO_API_KEY`.
- [ ] Confirm that Actions are enabled for the repository.
- [ ] Run the `Revisaurus` workflow manually once from **Actions -> Revisaurus -> Run workflow**.
- [ ] After the first successful run, open the deployed Pages URL from the workflow summary.
- [ ] If reviews hit GitHub API rate limits, add a fine-grained GitHub token secret and update the workflow/action configuration once Revisaurus supports overriding the provider token.

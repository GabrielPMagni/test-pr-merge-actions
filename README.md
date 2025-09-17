# Test PR Merge Actions

This repository hosts a minimal playground for validating an automated merge and release workflow that keeps `main` aligned with `staging` on a schedule.

## What the workflow does
- Triggers every Monday at 09:00 UTC or on demand via `workflow_dispatch`.
- Checks out `main`, fetches `staging`, and fast-forwards `main` when `staging` is ahead.
- Installs Node.js 20, runs `npm install`, and executes the release script (`npm run release -- --release-as minor`) so tags and package metadata stay in sync.
- Pushes commits and tags back to GitHub with a personal access token stored in the `PAT_TOKEN` secret.

The workflow definition lives in `.github/workflows/ci-release-merge-staging-to-main.yml`.

## Prerequisites
- A GitHub personal access token with `repo` scope saved as the `PAT_TOKEN` secret in the repository settings.
- A project-level `release` npm script (e.g. powered by semantic-release or standard-version) that can safely run in CI.

## Local validation tips
- Run the release script locally (`npm run release -- --dry-run`) to confirm it completes without prompting for input.
- Use `act` or a similar GitHub Actions runner to rehearse the workflow before pushing changes.

## Customising
- Adjust the cron schedule or merge target branches directly in the workflow file.
- Update the release command or Node.js version if your project requires different tooling.

# Contributing to Untern projects

This guide applies to every repository in the
[`unternai`](https://github.com/unternai) organization that does not ship its own `CONTRIBUTING.md`.

By participating you agree to follow our [Code of Conduct](https://github.com/unternai/.github/blob/main/CODE_OF_CONDUCT.md).

## Before you start

- Security issues: never open a public issue. Follow our [Security Policy](https://github.com/unternai/.github/blob/main/SECURITY.md).
- Questions and help: see [SUPPORT.md](https://github.com/unternai/.github/blob/main/SUPPORT.md).
- Bugs and feature ideas: search existing issues first, then open a new one with the issue forms. For anything
  larger than a small fix, open an issue and agree on the approach before writing code.

## Making a change

1. Fork the repository (external contributors) or create a branch (organization members). Branch names such as
   `fix/short-description` or `feat/short-description`.
2. Keep each pull request focused on one logical change. Add or update tests and documentation with the code.
3. Run the project's checks locally before pushing. For our Python projects this is usually:

   ```sh
   uv sync
   uv run ruff check .
   uv run ruff format --check .
   uv run pytest
   ```

   For TypeScript projects: `pnpm install`, `pnpm lint`, `pnpm typecheck`, `pnpm test`.
4. Open a pull request and fill in the template. Link the issue it resolves (for example `Closes #123`).
5. Target `dev` (see below). A required status check named `ci` must pass, and review conversations must be resolved, before a pull
   request can be merged. Pull requests are squash-merged, so the pull request title becomes the commit
   message. Write it in the imperative mood, for example `Fix token refresh race`.

## Branches: `dev`, then `main`

Every repository with CI has two long-lived branches:

| Branch | What it is | Who merges into it |
|---|---|---|
| `dev` (default) | Integration. Every feature or fix PR targets it. | Any member, once `ci` passes |
| `main` | Released code. A push here deploys or releases (for example untern.ai through Cloudflare Pages; tags `v*` publish packages). | Only a release PR from `dev` |

How it runs:

1. Branch from `dev`, open the PR against `dev` (the default, so `gh pr create` does it). CI runs once on the PR.
2. Merging into `dev` starts no CI or deploy run. Merge as soon as the PR is green; batch freely.
3. When `dev` holds something worth shipping (usually once a day, or when a fix is urgent), open one release PR
   `dev` → `main`. CI runs on it once, and the push to `main` deploys. Merge it with a merge commit (not squash), so
   `dev` and `main` keep the same history and the next release PR shows only new work.
4. Hotfix: branch from `main`, PR into `main`, then merge `main` back into `dev`.

Why: merging every PR straight into `main` ran CI and a deploy for each one. With `dev`, each change is checked once
on its PR, and `main` builds and deploys once per release.

Repositories that already had their own integration branch keep it (for example `untern-jobs-radar` uses
`staging`). Forks of upstream projects follow their upstream flow.

## Commit and pull request titles

We recommend [Conventional Commits](https://www.conventionalcommits.org/) style titles (`feat:`, `fix:`, `docs:`,
`chore:`, `refactor:`, `test:`), since the squashed pull request title is what ends up in the history.

## AI-assisted contributions

You may use AI coding assistants. You are responsible for every line you submit: make sure you understand it, that
it is tested, and that it does not include code or data you are not allowed to share.

## Licensing

Unless a repository says otherwise, contributions are accepted under the license in that repository's `LICENSE`
file. Do not submit code you do not have the right to contribute.

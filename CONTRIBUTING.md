# Contributing to Untern projects

Thanks for your interest in contributing! This guide applies to every repository in the
[`unternai`](https://github.com/unternai) organization that does not ship its own `CONTRIBUTING.md`.

By participating you agree to follow our [Code of Conduct](https://github.com/unternai/.github/blob/main/CODE_OF_CONDUCT.md).

## Before you start

- **Security issues:** never open a public issue — follow our [Security Policy](https://github.com/unternai/.github/blob/main/SECURITY.md).
- **Questions and help:** see [SUPPORT.md](https://github.com/unternai/.github/blob/main/SUPPORT.md).
- **Bugs and feature ideas:** search existing issues first, then open a new one using the issue forms. For anything
  larger than a small fix, please open an issue and get agreement on the approach before writing code.

## Making a change

1. Fork the repository (external contributors) or create a branch (organization members). Branch names such as
   `fix/short-description` or `feat/short-description` are appreciated.
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
5. A required status check named **`ci`** must pass, and review conversations must be resolved, before a pull
   request can be merged. Pull requests are **squash-merged**, so the pull request title becomes the commit
   message — please write it in the imperative mood, for example `Fix token refresh race`.

## Commit and pull request titles

We recommend [Conventional Commits](https://www.conventionalcommits.org/) style titles (`feat:`, `fix:`, `docs:`,
`chore:`, `refactor:`, `test:`), since the squashed pull request title is what ends up in the history.

## AI-assisted contributions

You may use AI coding assistants. You are responsible for every line you submit: make sure you understand it, that
it is tested, and that it does not include code or data you are not allowed to share.

## Licensing

Unless a repository says otherwise, contributions are accepted under the license in that repository's `LICENSE`
file. Do not submit code you do not have the right to contribute.

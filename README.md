# `unternai/.github`: shared org configuration

A special public GitHub repo: GitHub applies its content to the whole org. Anyone on the internet can read it, so
put nothing secret or internal here. Member-only content lives in `unternai/.github-private`.

## What is here

| Path | Effect |
|---|---|
| `profile/README.md` | Public org profile, shown to everyone at `github.com/unternai` |
| `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md` | Default community health files for every org repo that has no file of the same kind |
| `.github/ISSUE_TEMPLATE/` (`bug_report.yml`, `feature_request.yml`, `config.yml`) | Default issue forms; blank issues off, contact links added (security, support, conduct) |
| `.github/pull_request_template.md` | Default PR template |
| `workflow-templates/*.yml` + `*.properties.json` | Workflow templates shown under Actions → New workflow in every org repo |
| `.github/workflows/reusable-python-ci.yml` | Reusable workflow (`on: workflow_call`) for Python CI: `uv sync --locked`, ruff check, ruff format --check, pytest |
| `.github/workflows/lint-workflows.yml` | This repo's own CI: actionlint (+ shellcheck) on workflows and workflow templates; job named `ci` |

## GitHub behavior to know (checked on docs.github.com, 2026-11)

- Default community health files apply to every org repo, private ones included, when the repo has no file of the
  same kind (in `.github/`, the root or `docs/`). The `.github` repo must be public.
  Source: [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).
- Issue templates are all or nothing: if a repo has its own valid `.github/ISSUE_TEMPLATE/`, the whole default
  folder here is ignored (`config.yml` included).
- Labels used in an issue form must exist in both the `.github` repo and the repo using the template. The two forms
  here use `type/bug` and `type/feature` from `templates/labels.json` (bootstrap phase 4 creates the labels in every
  repo, `.github` included). If you change the label set, keep these two labels or edit `labels:` in the forms. A repo
  created after bootstrap (from a template too) has only GitHub's default labels: run bootstrap again or
  `gh label clone unternai/.github --repo unternai/<repo>`.
- An org cannot have a default `LICENSE`. Each public repo adds its own `LICENSE`.
- Org profile: the public README is `profile/README.md` in the public `.github` repo; the member-only README is
  `profile/README.md` in the private `.github-private` repo. Signed-in members see the member version by default.
  Source: [Customizing your organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile).
- Workflow templates in the public `.github` repo work for every repo type (public and private). Each template needs a
  `<name>.properties.json` with the same name (`name`, `description` required; `iconName`, `categories`,
  `filePatterns` optional). `$default-branch` is replaced with the default branch when the workflow is created.
  Sources: [Creating workflow templates](https://docs.github.com/en/actions/how-tos/reuse-automations/create-workflow-templates),
  [Reusing workflow configurations](https://docs.github.com/en/actions/reference/workflows-and-actions/reusing-workflow-configurations).
- A private repo can call a reusable workflow in a public org repo (a private caller can use private and public
  workflows; a public caller only public ones). The caller repo's Actions policy must allow actions and reusable
  workflows from `unternai`.
- Secrets do not pass into a reusable workflow on their own. The caller must pass them (`secrets:`); the template here
  passes exactly one secret, `PRIVATE_DEPS_APP_PRIVATE_KEY`. `GITHUB_TOKEN` in the called workflow can only lose
  permissions.
- Org secrets and variables do not work for private repos on GitHub Free. You need Team or higher, or set the secret
  per repo.

## Required status check `ci`

The org rulesets require exactly one check named `ci`. Jobs in a reusable workflow report as
`<caller job> / <called job>` (for example `python / python-ci`), so the caller always adds a gate job named `ci`
(`needs: [...]`, `if: always()`, fails unless every child job is `success`). See `workflow-templates/python-ci.yml`.

## Changing a reusable workflow

Callers reference `@main` (no SHA pin) because the org owns this repo and it did not exist when the templates were
written. Any change on `main` takes effect at once for every repo:

1. Backward-compatible change (new input with a default, bug fix): merge as usual through a PR.
2. Breaking change: add a new file (`reusable-python-ci-v2.yml`) or a tag (`v1`, `v2`) and let caller repos move over.
3. Pin actions to a full commit SHA with a `# vX.Y.Z` comment; bump versions with Renovate/Dependabot.

Lint locally before you push:

```sh
uvx --with shellcheck-py --from actionlint-py actionlint .github/workflows/*.yml workflow-templates/*.yml
```

## After replacing the placeholders

Replace the description sentence in `profile/README.md`, check that the `security@`, `conduct@` and `support@`
addresses exist (see `docs/03-email-identity/`), and turn on Private vulnerability reporting for public repos.

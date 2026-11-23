<!-- untern:branch-rule -->
> **Branch rule: read this before any git command.** The default branch is `dev`, not `main`.
> 1. Start with `git switch dev && git pull`, then `git switch -c feat/<topic>` (or `fix/`, `docs/`, `chore/`).
> 2. Open the PR **into `dev`**. Never commit or push to `main`: it only receives a reviewed release PR
>    `dev` → `main`. CI runs on pull requests, so one PR is one CI run.
> 3. If `git branch --show-current` prints `main`, stop and switch to `dev` first.
>
> Full flow: UnternAI/untern-org `AGENTS.md` section 3.
<!-- /untern:branch-rule -->

Org rules: UnternAI/untern-org AGENTS.md

# AGENTS.md

Rules specific to `.github`.

# security-checks

Shared security checks for Chiibitsu repos. One workflow, called from each repo.

## Checks
- **secrets**: gitleaks (`gitleaks: false` to skip where `chiibitsu/gates` already scans). On PRs it scans the PR's commits. Otherwise it scans full history.
- **deps**: `npm audit` on production and dev dependencies, failing at `high` by default. Not applicable, and says so, when there is no `package.json`. Red when there is a `package.json` but no `package-lock.json`: npm audit cannot read other lockfiles, and a skip would pass a check that checked nothing.
- **code (semgrep)**: semgrep 1.175.0 with the open-source `p/default`, `p/secrets` and `p/typescript` packs. On by default (`semgrep: false` to skip). Free on private repos, so it is the code scan every repo gets.
- **code (CodeQL)**: Off by default. Turn on with `codeql: true`, public repos only unless the repo has GitHub Advanced Security. Buildless languages only: javascript-typescript, python, ruby, actions.

## Use
Pin `uses:` to a commit SHA of this repo, never `@main`: every `uses:` line must be a SHA, the same rule `chiibitsu/gates` enforces. Copy `templates/caller.yml` to `.github/workflows/security.yml` in the target repo.

## Update
Change the checks here, merge, then bump the pin in each caller. Callers pin a commit SHA, so a change here does **not** reach them until their pin moves:

- Repos with Renovate: its `github-actions` manager opens a PR bumping the `uses:` SHA when this repo changes.
- Repos without Renovate: replace the SHA in `.github/workflows/security.yml` by hand with the new `main` commit.

Security fixes are not applied to a caller until its pin is bumped.

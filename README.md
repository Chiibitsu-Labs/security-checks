# security-checks

Shared security checks for Chiibitsu repos. One workflow, called from each repo.

## Checks
- **secrets**: gitleaks. On PRs it scans the PR's commits. Otherwise it scans full history.
- **deps**: `npm audit` on production dependencies. Skipped if there is no `package-lock.json`.
- **code**: CodeQL. Off by default. Turn on with `codeql: true`, public repos only unless the repo has GitHub Advanced Security. Buildless languages only: javascript-typescript, python, ruby, actions.

## Use
Copy `templates/caller.yml` to `.github/workflows/security.yml` in the target repo.

## Update
Change the checks here. Every caller picks it up on its next run.

# Git Workflow

This document standardizes how we branch, commit, and review code. We use trunk-based development with short-lived branches, Conventional Commits for consistency, and enforce both with git hooks and CI checks.

## How We Branch

Keep the main branch protected. Developers work on short-lived feature branches. All changes go through pull requests with at least one review. Use consistent naming: `feature/TICKET-123`, `fix/TICKET-123`, `chore/TICKET-123`.

## Commit Messages

Use Conventional Commits: `type(scope): subject`, where type is one of:
- `feat` for new features
- `fix` for bug fixes  
- `docs` for documentation changes
- `chore` for maintenance and tooling
- `refactor` for code restructuring
- `test` for test additions or changes
- `ci` for CI/CD updates

Examples:
- `feat(api): add order checkout endpoint`
- `fix(payments): handle capture timeout`

This format makes it easy to scan commit history and auto-generate changelogs.

## Making It Automatic

Use git hooks to lint and format code before committing. Use commitlint in a commit-msg hook to enforce the Conventional Commits format. The CI pipeline runs the full test suite and lint checks; if anything fails, the PR is blocked. We recommend signing commits on the main branch for extra security.

## Before Merging

When submitting a pull request, make sure:
- [ ] Tests are updated or added
- [ ] Documentation is updated if needed
- [ ] No secrets are in the diff
- [ ] The issue is linked as a reference

## Example commitlint config (conceptual)
```json
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "type-enum": [2, "always", ["feat", "fix", "docs", "chore", "refactor", "test", "ci"]]
  }
}
```

## Project-Specific Overrides
- Branch naming: feature/TICKET-123, fix/TICKET-123, chore/TICKET-123.
- Reviews: minimum 1 reviewer for non-prod code, 2 for prod-sensitive (payments/auth).
- Signing: require signed commits on main and release branches; enforce via repository settings.

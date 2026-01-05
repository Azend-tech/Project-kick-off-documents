# Git Workflow

Defaults: trunk or short-lived branches, Conventional Commits, enforced by commitlint/githooks.

Abbreviations: PR (Pull Request), CI (Continuous Integration), CD (Continuous Delivery), DRI (Directly Responsible Individual).

## Branching
- Main branch protected; feature branches short-lived.
- Require PRs with reviews; enforce status checks.
- Suggested naming: feature/TICKET-123, fix/TICKET-123, chore/TICKET-123.

## Commit format (Conventional Commits)
- type(scope): subject where type ∈ feat, fix, docs, chore, refactor, test, ci.
- Examples:
  - feat(api): add order checkout endpoint
  - fix(payments): handle capture timeout

## Enforcement
- Pre-commit: lint/format; optional unit test subset.
- Commit-msg hook: commitlint.
- CI: run full lint/test; block on failure.
- Recommended: signed commits for main; require approvals before merge.

## PR checklist (example)
- [ ] Tests updated/added
- [ ] Docs updated (if needed)
- [ ] No secrets in diff
- [ ] Linked issue/reference

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

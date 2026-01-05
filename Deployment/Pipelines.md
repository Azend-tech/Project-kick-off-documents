# Pipelines (CI/CD)

Purpose: Standardize build/test/deploy with safe rollouts.

Abbreviations: CI/CD (Continuous Integration/Continuous Delivery), SAST (Static Application Security Testing), DAST (Dynamic Application Security Testing).

## Stages (example)
- build → test → security-scan → package → deploy-dev → e2e → deploy-stage → perf → deploy-prod

## Gates
- Lint/unit tests must pass; coverage threshold target ≥ 80%.
- SAST/DAST and dependency scans blocking for high/critical issues.
- Infra plan review for prod (manual approval or policy checks).

## Rollouts
- Blue/green: shift traffic after health verification.
- Canary: progressive rollout by percentage or shard; auto-rollback on SLO errors.

## IaC integration
- Terraform: plan in PR; apply on merge for non-prod; controlled apply for prod.
- Azure-only option: Bicep templates with what-if validation.
- Policy: format/validate/plan as part of PR checks.

## Artifacts and signing
- Store images in registry; sign images (e.g., cosign) and verify on deploy.
- Tag images with git SHA and semantic version if applicable.

## Example pipeline snippet (GitHub Actions-like)
```yaml
jobs:
	build:
		runs-on: ubuntu-latest
		steps:
			- uses: actions/checkout@v4
			- run: npm ci && npm test && npm run lint
			- run: npm run build
			- run: terraform fmt -check && terraform validate
			- run: terraform plan -out=tfplan
```

## Project-Specific Overrides
- Branch policy: protect main; require CI (lint/test/build) + Terraform plan; require approvals.
- Deploy: Azure paths (App Service/AKS) as primary; AWS paths (ECS/EKS/Lambda) as secondary; choose workflow per repo.
- Artifacts: build Node (.tgz) and .NET (dll/container) images; tag with git SHA; store in ACR/ECR as per cloud.

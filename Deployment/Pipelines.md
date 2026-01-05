# Pipelines (CI/CD)

This document standardizes how code gets built, tested, and deployed safely.

## Pipeline Stages

A typical pipeline looks like this: **build** → **test** → **security scan** → **package** → **deploy to dev** → **end-to-end tests** → **deploy to stage** → **performance tests** → **deploy to prod**.

Each stage gates the next one, so a problem is caught early before it reaches production.

## Quality Gates

Code must pass linting and unit tests before moving forward. Aim for at least 80% test coverage. Static and dynamic security scans block the pipeline if they find high or critical issues. For infrastructure changes in production, require manual review or policy engine approval before applying.

## Deploying Safely

Use blue-green deployments to shift traffic after health checks pass. Use canary deployments to roll out gradually by percentage or shard, with automatic rollback if error rates breach your SLOs.

## Infrastructure as Code in Pipelines

Terraform plans run during pull requests so everyone can review the changes. Non-production environments apply automatically on merge. Production applies only after manual approval. For Azure users, Bicep with `what-if` validation is an option. Format, validate, and plan checks run as part of PR checks.

## Container Images and Artifacts

Store container images in your registry (Azure Container Registry, AWS ECR, etc.). Sign images with tools like cosign and verify signatures during deployment. Tag images with the git commit SHA and semantic version if applicable, so you always know exactly what code is running.

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

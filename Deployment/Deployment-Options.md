# Deployment Options

Purpose: Compare target platforms by environment (dev/stage/prod) with pros/cons and fit.

## Options (shortlist)
- Azure: App Service, AKS.
- AWS: ECS (Fargate), EKS, Lambda.
- VM: IaaS with config management.
- Docker-only: local and lightweight hosting.

Abbreviations: AKS (Azure Kubernetes Service), ECS (Elastic Container Service), EKS (Elastic Kubernetes Service), ALB (Application Load Balancer), IaaS (Infrastructure as a Service).

## Evaluation table (example for [ProjectName])
| Env  | Option          | Pros                                        | Cons                                | Fit   | Notes                              |
|------|-----------------|---------------------------------------------|-------------------------------------|-------|------------------------------------|
| prod | AKS             | RBAC, autoscaling, ecosystem, ingress      | Higher ops overhead                 | Good  | Ingress + managed certs            |
| prod | App Service     | Simple ops, slot swaps                     | Limited daemon workloads            | Good  | Stateless API/gateway              |
| prod | ECS Fargate     | No node mgmt, autoscale                    | AWS-specific                        | Good  | ALB ingress                        |
| prod | Lambda + API GW | Zero patching, scale-to-zero               | Cold starts, timeout limits         | Partial | Good for webhooks, light APIs      |
| stage| ECS Fargate     | Mirrors prod minus scale                   | Slightly higher cost vs EC2         | Good  | Use same IaC modules               |
| dev  | Docker Compose  | Fast local, easy onboarding                 | Not prod-like                       | Partial | Mirror env vars/ports              |
| dev  | Kind/minikube   | Closer to K8s                               | Heavier local footprint             | Optional | For K8s parity tests               |

## Rollout patterns
- Blue/green for low-risk cutovers; canary for incremental exposure.
- Feature flags for runtime control; use for risky changes.
- Infra toggles: traffic weights in gateway/ingress; fast rollback to last healthy revision.

## IaC
- Default: Terraform modules; CI plans + manual apply/auto-apply per env policy.
- Azure-only: Bicep acceptable; keep modules reusable.
- Policy: format+validate+plan on PR; apply gated for prod.

## Project-Specific Overrides
- Env choices: prod/stage on Azure App Service (Node/.NET) or AKS; AWS mirror via ECS Fargate or EKS when needed.
- Gateway: Azure API Management or AWS API Gateway/ALB; choose per cloud.
- Local/dev: Docker Compose with Postgres, MongoDB, and Redis; optional Kind for K8s parity.

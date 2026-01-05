# Deployment Options

This document compares the platforms available for running your application in different environments (dev, stage, prod), with pros and cons to help you choose what fits your needs.

## Popular Platforms

You have several choices:

- **Azure**: App Service for simple, managed hosting. Kubernetes Service (AKS) for more control and scale.
- **AWS**: Fargate for container management without managing servers. EKS for Kubernetes on AWS. Lambda for serverless functions.
- **VMs**: Infrastructure as a Service (IaaS) with tools like Terraform and Ansible for configuration management.
- **Local development**: Docker Compose for simple local dev environments.

## How to Choose

Here's a comparison of platforms for different environments. For production, you want something that scales automatically, can handle traffic spikes, and lets you roll out updates safely. For development, speed and simplicity matter more than replicating production exactly.

| Environment | Option | Why It's Good | Why It Might Not Be | Best For |
|---|---|---|---|---|
| **Prod** | Azure AKS | Kubernetes scales automatically, built-in RBAC, rich ecosystem, ingress controller | Requires some ops expertise | Full-featured systems needing scale |
| **Prod** | Azure App Service | Simple to set up, slot swaps for safe deployments | Limited support for long-running background jobs | Stateless APIs and web apps |
| **Prod** | AWS Fargate | No server management needed, scales automatically | AWS-specific | Container workloads that don't fit Lambda |
| **Prod** | AWS Lambda + API Gateway | Zero infrastructure to manage, scales from zero | Cold starts, timeout limits (15 min) | Webhooks, lightweight APIs, event handlers |
| **Stage** | AWS Fargate | Mirrors production, costs less than prod | Slightly pricier than EC2 | Using the same Infrastructure as Code as prod |
| **Dev** | Docker Compose | Fast to spin up locally, mirrors services without the overhead | Not as production-like as Kubernetes | Getting started, team onboarding |
| **Dev** | Kubernetes (Kind/minikube) | Closer to production Kubernetes setup | Heavier on your laptop | Testing Kubernetes-specific features |

## Safe Rollout Patterns

When deploying new code, use blue-green deployments for quick, low-risk cutovers. For riskier changes, use canary deployments to roll out to a small percentage of users first, then gradually increase traffic. Use feature flags for runtime control so you can disable a problematic feature without redeploying. In the gateway, you can weight traffic between old and new versions and quickly shift back if something breaks.

## Infrastructure as Code

Terraform is our standard choice. You write a plan in pull requests so reviewers can see what will change, then apply it on merge for non-production environments. For production, require manual approval before applying. If you're Azure-only, Bicep is acceptable, but keep your modules reusable. Always format, validate, and plan as part of your PR checks.

## Project-Specific Overrides
- Env choices: prod/stage on Azure App Service (Node/.NET) or AKS; AWS mirror via ECS Fargate or EKS when needed.
- Gateway: Azure API Management or AWS API Gateway/ALB; choose per cloud.
- Local/dev: Docker Compose with Postgres, MongoDB, and Redis; optional Kind for K8s parity.

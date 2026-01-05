# Architecture Document Set

This folder contains a complete set of architecture templates and guidance for building modern, cloud-native systems. We use a hypothetical multi-tenant commerce platform as our example throughout, but you should adapt everything here to match your specific project.

We follow the C4 model for diagrams (Context, Container, Component, and Deployment views) along with sequence diagrams to show key integration flows. For infrastructure, Terraform is our primary choice, with Bicep available for Azure-specific deployments. We enforce Conventional Commits across the repository using commitlint and git hooks.

## Understanding Project Tiers

All guidance in this document set is scalable based on your project tier. Before diving into the details, read [Project-Tiers.md](Project-Tiers.md) to understand:

- **POC (Proof of Concept)**: 2-4 weeks, minimal infrastructure, speed over everything
- **MVP (Minimum Viable Product)**: 8-12 weeks, basic reliability, small team
- **Full Build**: 3-6 months, production-grade with scale, growing team
- **Enterprise Solution**: 6+ months, mission-critical, regulated, large team

Each section in this document set includes tier-specific guidance. Start with your tier and adapt accordingly.

## How to get started

Begin with the Architecture-Overview and Integration-Views documents to understand the system boundaries and how different components talk to each other. As you make decisions about security, deployment, databases, and the application layer, you can fill out the corresponding sections in parallel. Use the Project-Specific Overrides sections to document any deviations from these defaults. Keep the Glossary up to date as terminology evolves, and include diagrams and image exports alongside the markdown files.

## Standard Service Level Objectives

We target 99.9% availability on a monthly basis. Latency for core read and write operations should stay within p95 ≤ 300ms and p99 ≤ 800ms, with authentication calls even faster at p95 ≤ 150ms. The system should handle 500–1,000 requests per second before gracefully degrading. With an error budget of 0.1% monthly, we aim to recover from incidents within 30–60 minutes (RTO) and lose no more than 15 minutes of data (RPO).

## Navigation
- Architecture: [Architecture-Overview](Architecture/Architecture-Overview.md), [Integration-Views](Architecture/Integration-Views.md)
- Security: [Security-Model](Security/Security-Model.md), [Secrets-Management](Security/Secrets-Management.md)
- Deployment: [Deployment-Options](Deployment/Deployment-Options.md), [Infra-Topology](Deployment/Infra-Topology.md), [Pipelines](Deployment/Pipelines.md)
- Frontend: [Frontend-Architecture](Frontend/Frontend-Architecture.md)
- Backend: [Backend-Architecture](Backend/Backend-Architecture.md), [API-Gateway](Backend/API-Gateway.md)
- Databases: [Database-Design](Databases/Database-Design.md), [Data-Governance](Databases/Data-Governance.md)
- Tooling: [Git-Workflow](Tooling/Git-Workflow.md), [Editor-Config](Tooling/Editor-Config.md), [Runbooks](Tooling/Runbooks.md)
- Glossary: [Glossary](Glossary.md)

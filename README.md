# Architecture Document Set

This folder contains architecture-focused templates with built-in guidance, examples, and project-specific override sections. A hypothetical project ([ProjectName]: multi-tenant commerce) is used for examples; replace with your specifics. Diagram standards: C4 (Context/Container/Component/Deployment) plus sequence diagrams for integrations. IaC: Terraform primary; Bicep for Azure-only footprints. Source control: Conventional Commits enforced via commitlint/githooks.

Abbreviations: SLO (Service Level Objective), RTO/RPO (Recovery Time/Point Objective), IaC (Infrastructure as Code), CI/CD (Continuous Integration/Delivery).

## How to use
- Start with Architecture-Overview and Integration-Views to frame the system and interfaces.
- Fill Security, Deployment, Data, and App-layer docs in parallel as decisions form.
- Use Project-Specific Overrides sections to record deviations from defaults.
- Keep Glossary updated and reference it across docs.
- Link diagrams (e.g., PlantUML, Mermaid) or image exports alongside each markdown.

## Standerd Service Level Objectives
- Availability: 99.9% monthly.
- Latency: p95 ≤ 300 ms and p99 ≤ 800 ms for key read/write APIs; auth p95 ≤ 150 ms.
- Throughput: targets hold up to 500–1,000 RPS; beyond that, degrade gracefully.
- Error budget: 0.1% monthly.
- RTO: 30–60 minutes; RPO: 15 minutes.

## Navigation
- Architecture: [Architecture-Overview](Architecture/Architecture-Overview.md), [Integration-Views](Architecture/Integration-Views.md)
- Security: [Security-Model](Security/Security-Model.md), [Secrets-Management](Security/Secrets-Management.md)
- Deployment: [Deployment-Options](Deployment/Deployment-Options.md), [Infra-Topology](Deployment/Infra-Topology.md), [Pipelines](Deployment/Pipelines.md)
- Frontend: [Frontend-Architecture](Frontend/Frontend-Architecture.md)
- Backend: [Backend-Architecture](Backend/Backend-Architecture.md), [API-Gateway](Backend/API-Gateway.md)
- Databases: [Database-Design](Databases/Database-Design.md), [Data-Governance](Databases/Data-Governance.md)
- Tooling: [Git-Workflow](Tooling/Git-Workflow.md), [Editor-Config](Tooling/Editor-Config.md), [Runbooks](Tooling/Runbooks.md)
- Glossary: [Glossary](Glossary.md)

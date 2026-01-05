# Architecture Overview

This document captures the system mission, constraints, design principles, and overall structure using C4 Context and Container diagrams.

## The System: {{ProjectName}}

Our example is a multi-tenant commerce platform that handles catalog browsing, shopping carts, checkout flows, payment processing, and order tracking. The system serves both web customers and external applications through public APIs. We operate across US and EU regions with tenant-aware data handling, ensuring each tenant's data stays isolated and meets regional compliance requirements. Every request, token, and database record includes a `tenantId` for strict isolation.

## In Scope

We're building the core commerce experience: product catalog, shopping cart, checkout, order management, payment processing, customer notifications, and basic reporting.

## Out of Scope

In-store point-of-sale systems and machine learning recommendations are future additions, not part of this phase.

## Success Metrics

We'll measure success by tracking conversion uplift, payment success rate, and adherence to our service level objectives outlined below.

## Design Principles

**Simplicity first**: We start with a modular monolith and only break out microservices when we have stable service contracts and clear reasons to scale or isolate them independently.

**Security and privacy by default**: Apply the principle of least privilege everywhere. Encrypt data in transit and at rest. Minimize how much PII we collect and store. Default to redacting sensitive data from logs.

**Observability first**: Instrumentation isn't an afterthought. Traces, metrics, and logs flow from all critical paths. Correlation IDs tie everything together for debugging.

**Small, reversible changes**: Use feature flags, blue-green deployments, and canary releases so we can roll back quickly if something breaks.

**Contract-first API design**: Publish versioned OpenAPI specs and prioritize backward-compatible changes over breaking updates.

## Constraints and Requirements

**Regulatory**: Payment tokenization is required for PCI compliance. EU tenants must have their data stored in EU regions (GDPR residency).

**Technology**: Cloud-first approach. Terraform for infrastructure as code. Postgres as the primary OLTP database. Redis for caching.

**Data limits**: Orders are partitioned by `tenantId` and month. Individual items (documents/records) must stay under 2 MB.

## Operational Rules

Cross-tenant access is strictly forbidden. The gateway and every service must enforce `tenantId` checks on every request. All services emit traces, metrics, and structured logs with correlation IDs. Configuration follows twelve-factor app principles and secrets are never baked into container images.

## Quality Attributes and SLOs

We target 99.9% availability on a monthly basis. Latency for core APIs (read and write) should stay within p95 ≤ 300ms and p99 ≤ 800ms, with authentication requests even faster at p95 ≤ 150ms. The system should sustain 500–1,000 requests per second before gracefully degrading with backpressure. Our monthly error budget is 0.1%, meaning we can afford roughly 43 minutes of downtime per month. We aim to recover from incidents within 30–60 minutes (RTO) and should lose no more than 15 minutes of data in a disaster (RPO).

## C4 diagrams (Mermaid)
Context:
```mermaid
C4Context
	title {{ProjectName}} Context
		Person(user, "Buyer")
		Person(merchant, "Merchant Admin")
		System_Boundary(nb, "{{ProjectName}}") {
			System(web, "Web App")
			System(api, "Public API")
		}
		System_Ext(psp, "Payment Provider")
		Rel(user, web, "Browses, checks out")
		Rel(merchant, web, "Manages catalog")
		Rel(api, psp, "Payment intents")
```

Container:
```mermaid
C4Container
	title {{ProjectName}} Containers
		Person(buyer, "Buyer")
		Person(admin, "Merchant Admin")
		System_Boundary(app, "{{ProjectName}}") {
			Container(gw, "API Gateway", "Nginx/API Mgmt")
			Container(front, "Frontend", "React")
			Container(api, "API", "Node/.NET")
			Container(queue, "Events", "Kafka/Service Bus")
			ContainerDb(db, "Postgres", "OLTP")
			ContainerDb(cache, "Redis", "Cache")
		}
		Rel(buyer, front, "HTTPS")
		Rel(admin, front, "HTTPS")
		Rel(front, gw, "HTTPS")
		Rel(gw, api, "REST/JSON")
		Rel(api, db, "SQL")
		Rel(api, cache, "Get/Set")
		Rel(api, queue, "Publish events")
```

## Tech stack (example)
- Frontend: React + Vite; API client via OpenAPI-generated SDK; RUM for latency/errors.
- Backend: .NET/Node/Java; modular monolith initially, evolve to services by domain.
- Data: Postgres primary; Redis cache; object storage for receipts; metrics/logs/traces central.
- Messaging: Kafka/Service Bus for async flows (order lifecycle, notifications).

## Project-Specific Overrides
- Stack: Frontend React (buyer/admin), Blazor (internal ops if needed); Backend Node.js + .NET APIs; 
	Data: Postgres (orders/payments), MongoDB (catalog/content), SQL Server (reporting/legacy); 
	Cloud: Azure primary (App Service/AKS), AWS secondary (ECS/EKS for portability).
- SLOs: overall service 99.9% monthly; payments endpoints 99.95% monthly; auth p95 ≤ 150 ms; core API p95 ≤ 300 ms/p99 ≤ 800 ms; payments p95 ≤ 250 ms.
- Diagrams: choose Azure API Management or AWS API Gateway as gateway component based on deployment.

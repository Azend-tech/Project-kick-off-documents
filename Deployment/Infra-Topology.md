# Infra Topology

Purpose: Describe deployment topology with C4 Deployment diagrams and network zoning.

Abbreviations: CDN (Content Delivery Network), AZ (Availability Zone), NAT (Network Address Translation).

## Diagram guidance
- C4 Deployment diagram per environment (dev/stage/prod).
- Show: gateway/ingress, app/services, data stores, messaging, observability stack.
- Include trust boundaries: public vs private subnets, management plane.

Example (Mermaid):
```mermaid
C4Deployment {
	title [ProjectName] Prod Deployment
	Deployment_Node(cdn, "CDN/Edge") {
	}
	Deployment_Node(lb, "Gateway/Ingress") {
	}
	Deployment_Node(app, "App Nodes", "AKS/ECS") {
		Container(api, "API Pods", "Node/.NET")
		Container(front, "Frontend", "React static")
	}
	Deployment_Node(data, "Data Layer") {
		ContainerDb(db, "Postgres", "Primary + Replica")
		ContainerDb(cache, "Redis", "Cache")
		Container(queue, "Kafka/Service Bus", "Events")
	}
	Rel(cdn, lb, "HTTPS")
	Rel(lb, app, "HTTPS")
	Rel(api, db, "SQL/TLS")
	Rel(api, cache, "TLS")
	Rel(api, queue, "TLS")
}
```

## Network
- Ingress: managed gateway/ALB/NGINX ingress with TLS termination.
- Egress: restrict outbound; use NAT/egress rules; allowlists for third-party APIs.
- DNS: managed zones; split-horizon if needed.

## Observability shared services
- Logging: centralized (e.g., ELK/Cloud-native logging).
- Metrics: Prometheus/Cloud native.
- Tracing: OpenTelemetry collector + backend (e.g., Jaeger/Zipkin/Application Insights/X-Ray).

Expectation: every service exports metrics (latency/error rate/RPS), traces, and structured logs with correlation IDs.

## Resilience
- Zonal distribution where supported; multi-AZ/zone for prod tiers.
- Backups: schedule + retention; test restores.
- Chaos drills on non-prod before major releases.

## Project-Specific Overrides
- Azure topology: API Management/Ingress → App Service or AKS → Postgres (Azure DB), MongoDB Atlas, Redis; Service Bus/Kafka for events; Key Vault for secrets.
- AWS topology (secondary): API Gateway/ALB → ECS/EKS → RDS Postgres, MongoDB Atlas, ElastiCache; MSK/Kinesis for events; Secrets Manager/KMS for keys.
- Compliance: segregate EU tenants to EU regions; enforce private endpoints for data stores.

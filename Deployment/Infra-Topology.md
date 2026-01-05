# Infrastructure Topology

This document describes your deployment topology, showing how components are distributed across environments (dev, stage, prod) and how they communicate through network boundaries.

## What to Include in Your Diagrams

Draw a C4 Deployment diagram for each environment. Show the gateway/ingress, application nodes, data stores, messaging systems, and observability stack. Mark trust boundaries clearly: which parts are public-facing, which are private, and what's restricted to the management plane.

Example (Mermaid):
```mermaid
C4Deployment
    Deployment_Node(cdn, "CDN / Edge", "Cloud CDN") {
    }

    Deployment_Node(lb, "Gateway / Ingress", "NGINX / App Gateway") {
    }

    Deployment_Node(app, "App Nodes", "AKS / ECS") {
        Container(api, "API Pods", ".NET", "Backend REST APIs")
        Container(front, "Frontend", "React", "Static web application")
    }

    Deployment_Node(data, "Data Layer", "Managed Services") {
        ContainerDb(db, "PostgreSQL", "Postgres", "Primary + Read Replica")
        ContainerDb(cache, "Redis", "Redis", "Distributed cache")
        Container(queue, "Event Bus", "Kafka / Service Bus", "Asynchronous messaging")
    }

    Rel(cdn, lb, "HTTPS")
    Rel(lb, app, "HTTPS")
    Rel(api, db, "SQL over TLS")
    Rel(api, cache, "TLS")
    Rel(api, queue, "TLS")

```

## Network Design

**Ingress**: Use a managed gateway or ALB with TLS termination. This is where HTTPS connections end and internal traffic begins.

**Egress**: Restrict outbound traffic from services. Use NAT gateways or egress rules to control what services can reach externally, and maintain allowlists for third-party APIs.

**DNS**: Use managed DNS zones. If needed, implement split-horizon DNS for different internal and external views.

## Observability Shared Services

**Logging**: Use a centralized logging platform (ELK stack, cloud-native logging) so you can search logs across all services.

**Metrics**: Prometheus or your cloud provider's native metrics service to track CPU, memory, request latency, and error rates.

**Tracing**: Deploy an OpenTelemetry collector that sends traces to a backend like Jaeger, Zipkin, Application Insights, or X-Ray.

Every service should export metrics (latency, error rate, requests per second), traces, and structured logs with correlation IDs so you can trace a request end to end.

## Making the System Resilient

Distribute your workloads across availability zones when the cloud provider supports it. In production, run multiple instances across zones so a single zone failure doesn't take the system down. Back up your databases daily with full backups plus point-in-time recovery. Test restores quarterly so you know they actually work. Before major releases, run chaos engineering drills in non-production environments to find weak points.

## Project-Specific Overrides
- Azure topology: API Management/Ingress → App Service or AKS → Postgres (Azure DB), MongoDB Atlas, Redis; Service Bus/Kafka for events; Key Vault for secrets.
- AWS topology (secondary): API Gateway/ALB → ECS/EKS → RDS Postgres, MongoDB Atlas, ElastiCache; MSK/Kinesis for events; Secrets Manager/KMS for keys.
- Compliance: segregate EU tenants to EU regions; enforce private endpoints for data stores.

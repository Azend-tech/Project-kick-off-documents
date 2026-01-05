# Glossary

Use this glossary to keep terminology and data classification consistent. Add terms as they appear in requirements, APIs, schemas, and logs.

## Table
| Term | Definition | Data Classification (PII / Sensitive / Internal / Public) | Notes |
|------|-------------|-----------------------------------------------------------|-------|
| user | End-customer or account holder authenticating to the system. | PII | Includes identifiers like email or phone.
| order | A request to purchase one or more items with pricing, tax, and fulfillment state. | Internal | May reference PII via user or shipping details.
| payment | Authorization and capture records linked to orders; includes method and status. | Sensitive | Tokenized; never store raw PAN/CVV.
| session | Authenticated interaction window tracked via token. | Internal | Token contains no secrets beyond JWT.
| tenant | Logical partition for multi-tenant isolation. | Internal | Use in partition keys and authorization checks.
| catalog | Collection of SKUs/products with pricing and availability. | Internal | Avoid PII; links to media assets.
| inventory | Stock counts and reservations for fulfillment. | Internal | Tie to warehouses; no PII.
| fulfillment | Shipment or delivery process state for an order. | Internal | References shipping address (PII) via IDs.
| refund | Return of funds tied to a payment. | Sensitive | Should avoid PII; reference payment token and order only.

## Guidance
- Keep definitions short and action-oriented; avoid synonyms that cause drift.
- Align data classification with Security and Data Governance documents.
- Add domain-specific terms early (e.g., catalog, inventory, fulfillment, refund).
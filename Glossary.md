# Glossary

This glossary helps us stay consistent with terminology and data classification across the entire project. As new terms emerge in requirements, APIs, schemas, and logs, add them here to prevent drift in meaning.

## Terms and Definitions

| Term | Definition | Data Classification | Notes |
|------|-------------|-------------------|-------|
| **user** | An end-customer or account holder who authenticates to the system. | PII | Includes identifiers like email or phone number. |
| **order** | A customer's request to purchase items, including pricing, taxes, and fulfillment status. | Internal | May contain references to PII through the user or shipping details. |
| **payment** | Authorization and capture records tied to an order, including payment method and transaction status. | Sensitive | Always tokenized; raw card data (PAN/CVV) must never be stored. |
| **session** | An authenticated interaction window, tracked via a token. | Internal | The token itself contains no secrets beyond the JWT payload. |
| **tenant** | A logical partition representing a customer organization in the multi-tenant system. | Internal | Used in partition keys and authorization checks. |
| **catalog** | The collection of products (SKUs) available for purchase, with pricing and availability data. | Internal | Should not contain PII; includes links to product images and assets. |
| **inventory** | Current stock counts and reservations for fulfillment and shipping. | Internal | Tied to warehouses and distribution centers; no PII. |
| **fulfillment** | The shipment and delivery process state for an order. | Internal | References the shipping address (PII) via foreign key IDs only. |
| **refund** | A return of funds tied to a payment transaction. | Sensitive | Must not contain PII; reference only the payment token and order ID. |

## How to use this glossary

Keep definitions concise and focused on what matters for the business and technical decisions. Avoid creating synonyms that might confuse teams. Make sure your data classification decisions here align with what you document in the Security and Data Governance sections. Add domain-specific terms early—things like catalog, inventory, fulfillment, and refund—to prevent confusion later.
# Architecture Overview

OrdoFlow is an API-first quote-to-cash orchestration platform.

## Core workflow

1. A quote is created or reviewed.
2. The AI copilot recommends next actions and summarizes risks.
3. The quote progresses toward invoicing.
4. Payment status is tracked.
5. Accounting sync is handled through connected providers.
6. Customer follow-up is generated with AI-assisted workflow support.

## Main components

- Web application (operator UI)
- API layer (workflow and integrations)
- Workflow orchestration layer
- AI copilot layer (deterministic core + optional enrichment)
- Integration layer for third-party systems
- Accounting and payment adapters
- Multi-tenant organization data model

## Intended integrations

- CRM systems
- ERP systems
- Xero
- QuickBooks
- Stripe
- PayPal
- Email/SMS/WhatsApp providers

## Hackathon focus

For the AMD Developer Hackathon, the showcased flow is the quote detail copilot:

- user opens a quote,
- runs AI next-step plan,
- receives summary + risks + prioritized actions + customer messaging support.

Optional model enrichment can be routed through an OpenAI-compatible endpoint (for example vLLM on AMD cloud GPUs), while the deterministic workflow remains the reliability baseline.

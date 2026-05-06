# Security Notes for Public Demo Repository

This repository is intentionally limited to safe public demo material for hackathon judging.

The canonical production codebase is kept in a **separate private repository** and is not mirrored here.

This repo may include **public judge/demo credentials** for the hosted product (hackathon review only). Treat them as non-secret and rotate them after the event.

## In scope

- Public overview documentation
- Architecture summary
- Sample payloads with fake data
- Demo screenshots and deck placeholders

## Out of scope (must never be committed here)

- `.env` files with real values
- `config/master.key`
- Rails credentials or encrypted secrets
- Real `database.yml` credentials
- API keys (Stripe, PayPal, Xero, QuickBooks, model providers)
- JWT or session secrets
- Customer or tenant data
- Production logs or DB dumps
- Private repository history

## Reporting

If you discover a security issue related to the public demo package, contact the repository owner privately and do not disclose sensitive details publicly.

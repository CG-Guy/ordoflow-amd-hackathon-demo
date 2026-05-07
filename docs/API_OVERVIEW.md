# API overview (non-secret, shape-only)

This document describes **integration shapes** for judges. It does **not** include base URLs, tokens, tenant IDs, or live payloads.

## Authenticated JSON API

OrdoFlow exposes versioned JSON endpoints behind standard session/JWT patterns (details omitted here).

## Quote-to-cash copilot (conceptual)

**Product spec:** [FUNCTIONAL_SPEC_AI_QUOTE_TO_CASH_COPILOT.md](FUNCTIONAL_SPEC_AI_QUOTE_TO_CASH_COPILOT.md)

**Purpose:** given quote context, return structured guidance for operators.

**Pattern:**

- **Request:** organization-scoped quote context (status, totals, customer region fields as configured).
- **Response:** structured blocks such as:
  - `summary` (string)
  - `risk_flags` (list)
  - `next_actions` (ordered list with titles/details)
  - optional **enrichment** fields when LLM path is enabled (wording assistance)

## Example artifacts

See `examples/` for **fictitious** JSON and prompt text:

- `sample_quote_to_cash_payload.json`
- `sample_api_response.json`
- `sample_ai_workflow_prompt.md`

These files are **demo-safe**: dummy customer names and amounts.

## AMD / optional LLM path (conceptual)

Optional enrichment calls may route to an OpenAI-compatible **`/v1`**-style HTTP API (for example a **vLLM** server on **AMD Instinct**). Configuration is via environment variables in deployment—not committed publicly.

For **request/response shapes, env var names, and compliance notes**, see [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md).

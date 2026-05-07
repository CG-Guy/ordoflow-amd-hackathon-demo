# Functional spec: AI Quote-to-Cash Copilot

Public-facing specification for hackathon judges and integrators. **Full implementation, OpenAPI export, and automated tests** live in the **private OrdoFlow production repository**. JSON shapes for the optional enrichment layer: [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md). Sidecar / Track 1 stack narrative: [COPILOT_SIDECAR_RUNBOOK.md](COPILOT_SIDECAR_RUNBOOK.md).

---

## Objective

Provide a lightweight AI-assisted workflow helper inside OrdoFlow so operators can quickly see recommended next actions for a quote (draft / sent / accepted / paid) **without leaving the quote detail screen**.

---

## Scope

| Area | Description |
|------|-------------|
| **Backend endpoint** | `POST /api/v1/ai/quote_to_cash_copilot` |
| **Backend service** | Deterministic recommendation engine using quote context |
| **UI** | Quote detail action to request guidance and render recommendations |
| **Tests** | Backend request specs + UI unit specs (private repo) |
| **Optional** | HTTP **enrichment** for natural-language fields. A **FastAPI** service (LangChain + LangGraph, OpenAI-compatible open models) can implement the same JSON contract as [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md); production may use **vLLM on AMD ROCm**. Source and runbooks are **not** vendored in this public pack—see [COPILOT_SIDECAR_RUNBOOK.md](COPILOT_SIDECAR_RUNBOOK.md). |

### Out of scope

- Fine-tuning or external model training
- Autonomous execution of actions (this release is **recommendation-only**)
- Cross-quote batch optimization

---

## User story

**As a** quote operations user, **I want** AI-generated next-step guidance for the current quote **so that** I can move quote-to-cash actions forward faster and with fewer missed steps.

---

## Functional requirements

1. User can click **Get AI next-step plan** on the quote detail page.
2. UI sends quote context (e.g. `status`, `total_amount`, `currency`, `customer_country`, plus fields required by the live API) to the backend copilot endpoint.
3. Backend returns:
   - `summary`
   - `risk_flags`
   - `next_actions` (e.g. `id` / `title` / `reason` / `priority`)
   - `suggested_payload` metadata
4. UI renders the plan in a readable panel with action priority and reason text.
5. Failures show an inline error via existing application messaging (e.g. banner / toast pattern).

---

## Non-functional requirements

- Must follow existing **auth** and API versioning (`/api/v1/*`).
- Must **not** expose secrets or rely on hardcoded credentials.
- Response should be **deterministic and fast** for hackathon/demo reliability; LLM enrichment is **opt-in** and must **not** replace deterministic outputs as the source of truth.

---

## Optional LLM enrichment

When `ORDOFLOW_COPILOT_LLM_ENRICHMENT=true` and `ORDOFLOW_COPILOT_INFERENCE_URL` are set, `data` may include **enrichment** (`status`, `narrative`, optional `customer_message_draft`, `inference_source`, etc.). The Rails app does **not** host the model: it calls the configured URL (for example a LangGraph sidecar or **vLLM** on AMD Instinct). See [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md) for the sidecar request/response contract and env vars.

---

## API contract (illustrative)

**Note:** The live request body may include additional organization-scoped fields. This shape is representative for documentation.

### Request

`POST /api/v1/ai/quote_to_cash_copilot`

```json
{
  "query": "What is the best next step for this quote?",
  "quote": {
    "status": "sent",
    "total_amount": 12500,
    "currency": "USD",
    "customer_country": "US"
  }
}
```

### Response

```json
{
  "success": true,
  "data": {
    "summary": "Quote is sent. Amount is USD 12500.00. Customer region: US.",
    "user_query": "What is the best next step for this quote?",
    "risk_flags": ["high_value_quote"],
    "next_actions": [
      {
        "id": "follow_up_customer",
        "title": "Follow up with customer",
        "reason": "Quote was sent; request acceptance or payment confirmation.",
        "priority": "high"
      }
    ],
    "suggested_payload": {
      "quote_status": "sent",
      "amount": 12500.0,
      "currency": "USD",
      "organization_id": 123
    },
    "enrichment": {
      "status": "ok",
      "narrative": "Optional paragraph from the inference sidecar (e.g. LangGraph + open model).",
      "customer_message_draft": "Optional draft; server may also derive a customer-ready string.",
      "customer_message_ready": "Polished line built in Rails if needed for one-click copy.",
      "inference_source": "langgraph_langchain_oss"
    }
  }
}
```

The **`enrichment`** object appears only when LLM enrichment env vars are enabled and the call succeeds; omit or mark skipped/error when enrichment is off or fails—see [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md).

---

## Acceptance criteria

1. Clicking **Get AI next-step plan** renders at least one recommendation for valid quote data.
2. Missing or invalid quote payload returns **400** with a clear error.
3. Backend request specs and UI tests pass in the **private** codebase CI.
4. This public pack documents the endpoint, UI action, and optional sidecar via this file, [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md), [COPILOT_SIDECAR_RUNBOOK.md](COPILOT_SIDECAR_RUNBOOK.md), and the root [README.md](../README.md).

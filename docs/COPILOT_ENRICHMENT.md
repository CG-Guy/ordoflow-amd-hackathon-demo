# Quote-to-cash copilot — optional LLM enrichment (AMD / ROCm friendly)

OrdoFlow’s quote detail copilot keeps a **deterministic** plan as the reliability baseline. **Optional** natural-language enrichment runs only when enabled via environment variables and a reachable HTTP inference endpoint.

**Scope of this public repo:** contracts and behavior described here are accurate for the product. The **sidecar service source**, Docker Compose profiles, and Kubernetes manifests referenced in internal engineering docs live in the **private production repository**—they are intentionally not mirrored here.

---

## Architecture pattern

1. The app computes a structured **deterministic plan** (summary, risks, next actions).
2. If enrichment is enabled, the app **POSTs** that plan (plus minimal context) to an HTTP sidecar or compatible model server.
3. The sidecar returns optional **narrative** and **customer message draft** fields. On timeout or error, the API still returns the deterministic payload; enrichment is marked skipped or errored with a stable reason.

Enrichment does **not** execute payments or ERP actions; it only adds optional text fields.

---

## Enablement (environment variables)

| Variable | Meaning |
|----------|---------|
| `ORDOFLOW_COPILOT_LLM_ENRICHMENT` | Set to `true` to attempt HTTP enrichment |
| `ORDOFLOW_COPILOT_INFERENCE_URL` | Full URL for `POST` (JSON body → JSON response) |
| `ORDOFLOW_COPILOT_INFERENCE_TOKEN` | Optional `Authorization: Bearer …` for the endpoint |
| `ORDOFLOW_COPILOT_INFERENCE_TIMEOUT_SEC` | Socket timeout (e.g. default on the order of ~12s) |

When unset or not `true`, responses match the original copilot payload (no successful enrichment layer).

---

## Request body (app → sidecar)

Shape-only example:

```json
{
  "version": 1,
  "query": "",
  "organization_id": 1,
  "deterministic_plan": {
    "summary": "...",
    "user_query": "...",
    "risk_flags": [],
    "next_actions": [],
    "suggested_payload": {}
  }
}
```

---

## Successful response body (sidecar → app)

```json
{
  "narrative": "Plain-language summary for the operator.",
  "customer_message_draft": "Optional draft message to send to the customer.",
  "inference_source": "langgraph_langchain_oss"
}
```

The integration treats enrichment as successful when **`narrative`** and/or **`customer_message_draft`** is non-empty (exact merging rules live in the application). If the sidecar omits a ready-made customer message, the app may still derive customer-ready copy from the narrative.

On timeout or HTTP error, the API returns enrichment metadata with **`status: error`** and a stable **`skip_reason`**—deterministic cards remain authoritative.

---

## Hackathon / AMD alignment (Track 1)

For a **Lablab / AMD** story, you can run an open-weight model behind **vLLM** (or similar) on **AMD Instinct** GPUs with **ROCm**, then point `ORDOFLOW_COPILOT_INFERENCE_URL` at your HTTP endpoint (directly or via a small FastAPI + LangChain/LangGraph sidecar in your **private** codebase).

**References (external):**

- [ROCm documentation](https://rocm.docs.amd.com/en/latest/)
- [ROCm on GitHub](https://github.com/ROCm/ROCm)
- [AMD Developer Cloud — getting started](https://www.amd.com/en/developer/resources/technical-articles/2025/how-to-get-started-on-the-amd-developer-cloud-.html)

Production web hosting (e.g. Render) for the main app is separate from where GPU inference runs; enrichment is **optional** at the app layer and can be scaled or disabled independently.

---

## Related files in *this* repository

- [FUNCTIONAL_SPEC_AI_QUOTE_TO_CASH_COPILOT.md](FUNCTIONAL_SPEC_AI_QUOTE_TO_CASH_COPILOT.md) — product scope, illustrative API payload, acceptance criteria
- [COPILOT_SIDECAR_RUNBOOK.md](COPILOT_SIDECAR_RUNBOOK.md) — LangChain/LangGraph sidecar story, mock/Ollama/vLLM modes (source in private repo)
- [API_OVERVIEW.md](API_OVERVIEW.md) — high-level API shapes
- [WALKTHROUGH.md](WALKTHROUGH.md) — operator flow and AMD cloud narrative
- `examples/sample_ai_workflow_prompt.md` — illustrative prompt / JSON examples (fictitious data)

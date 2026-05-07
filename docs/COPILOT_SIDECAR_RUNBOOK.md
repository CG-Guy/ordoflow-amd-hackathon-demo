# OrdoFlow copilot — LangChain + LangGraph sidecar (Lablab Track 1)

This page describes the **optional LLM enrichment** path in terms the AMD Developer Hackathon **Track 1** expects: **LangChain + LangGraph**, **agentic** multi-step flow (operator narrative → customer message draft), and **open-weight** models behind an **OpenAI-compatible** HTTP API—not a hard dependency on a single vendor GPT.

**Important:** The runnable **FastAPI sidecar** (source, `Dockerfile`, `docker compose` profile, pytest suite, and k8s overlays) lives in the **private OrdoFlow codebase**. This public demo repository documents **behavior and contracts** so judges can evaluate architecture without cloning proprietary code. The JSON contract is in [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md).

---

## Architecture (who is authoritative)

- **Deterministic planner** in the main application remains **authoritative** for workflow structure (summary, risks, next actions).
- The **sidecar** only adds optional **`narrative`** and **`customer_message_draft`** (and metadata such as `inference_source`) under the enrichment layer described in [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md).
- No GPU on your laptop is required for **development**: the hackathon promotes **cloud** AMD Instinct + ROCm for production-like inference; locally you can use **mock** mode or **Ollama**.

---

## OrdoFlow (Rails) — point the app at the sidecar

| Variable | Meaning |
|----------|---------|
| `ORDOFLOW_COPILOT_LLM_ENRICHMENT` | `true` to attempt HTTP enrichment |
| `ORDOFLOW_COPILOT_INFERENCE_URL` | Full URL for `POST` to the sidecar enrich route (e.g. `http://<host>:8001/v1/enrich`) |
| `ORDOFLOW_COPILOT_INFERENCE_TOKEN` | Optional `Authorization: Bearer …` shared secret if the sidecar requires it |
| `ORDOFLOW_COPILOT_INFERENCE_TIMEOUT_SEC` | Request timeout (seconds) |

When enrichment is off or the call fails, the API still returns the deterministic copilot payload; enrichment shows `error` / skip with a stable reason (see [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md)).

---

## Sidecar — typical environment (private repo)

Exact variable names and defaults are maintained next to the **Python service** in the private repository. Conceptually:

| Mode | Purpose |
|------|---------|
| **Mock LLM** | CI and quick dev without a model: set the sidecar’s mock flag (e.g. `COPILOT_MOCK_LLM=1` in the private service), run **FastAPI** on a local port, hit `GET /health` and `POST /v1/enrich` with the body shape in [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md). |
| **Ollama** | Local open model: OpenAI-compatible base URL (often `http://127.0.0.1:11434/v1`), model id, and API key placeholder as required by the shim. |
| **vLLM + ROCm (AMD Instinct)** | On **AMD Developer Cloud** or your own MI300-class hosts: serve an open model with an OpenAI-compatible `/v1` base URL; set the sidecar’s LLM base URL, model id, and optional token; label inference source for observability (e.g. `langgraph_vllm_rocm`). |

**AMD / cloud:** Use [AMD Developer Cloud](https://www.amd.com/en/developer/resources/cloud-access/amd-developer-cloud.html) and [ROCm docs](https://rocm.docs.amd.com/en/latest/) for GPU setup; point the sidecar at your **`/v1`** endpoint, then point OrdoFlow at the sidecar’s **`/v1/enrich`** URL.

**Kubernetes:** Production-oriented **vLLM** on AMD nodes, Services, and optional in-cluster sidecar examples are **not** vendored in this public pack—see the private repo’s `infra/` and ops docs if you have access.

---

## Docker Compose (private repo)

From the **private** monorepo root, a `docker compose --profile copilot` (or equivalent) profile typically brings up a **`copilot-sidecar`** service. Mock mode is common for laptops; for real inference, disable mock and set LLM base URL / model variables, or bridge to Ollama on the host (`host.docker.internal` on Windows/macOS; Linux may need `extra_hosts` or the docker bridge IP).

---

## Tests (private repo)

The sidecar’s tests usually run with **mock LLM** enabled so **CI does not require a GPU**. Command shape: `pytest` against the service’s `tests/` directory (exact invocation is in the private README).

---

## Hugging Face Space (optional)

The AMD hackathon may offer a **Hugging Face** category. A Space can wrap **`POST /v1/enrich`** or a thin UI. Join the **event HF organization** from the Lablab event page if you compete for that prize; the **Dockerfile** for the sidecar lives in the **private** repo unless you publish a minimal public Space separately.

---

## Related

- [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md) — request/response JSON, compliance, env table (OrdoFlow)
- [WALKTHROUGH.md](WALKTHROUGH.md) — operator-facing flow and AMD narrative
- [AMD_HACKATHON_SUBMISSION.md](AMD_HACKATHON_SUBMISSION.md) — Lablab paste-ready text and AMD options A/B/C

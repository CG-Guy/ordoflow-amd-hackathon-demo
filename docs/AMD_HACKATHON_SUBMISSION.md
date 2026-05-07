# AMD Developer Hackathon — final submission handoff (public pack)

This page is the **judge-facing** handoff for the [AMD Developer Hackathon on Lablab](https://lablab.ai/ai-hackathons/amd-developer). It aligns OrdoFlow’s **AI quote-to-cash copilot** with the event focus: **agentic AI**, real-world outcomes, and **AMD cloud / GPU** where applicable.

**Public GitHub (this repo):** `https://github.com/CG-Guy/ordoflow-amd-hackathon-demo`  
**Production source:** private repository (not linked here — security, tenants, IP).

**Related technical doc:** [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md) (optional LLM sidecar contract, env vars, AMD / ROCm alignment).

---

## Quick submit checklist

Use Lablab’s wizard and keep these consistent with the root [README.md](../README.md):

| Step | Artifact |
|------|-----------|
| Basic info | Title, short + long description, tags |
| Media | Cover (16:9), **video** (≤ ~5 min MP4), **slides PDF** |
| Application | **Public GitHub** → this repo; **demo URL** → live app; platform **Other** if not Streamlit/Replit/Vercel |
| Proof | Working login (if needed), video shows the same path as the README |

Confirm **deadlines and schedule** on the live event page (your timezone).

---

## Event alignment (poster / Lablab story)

| Theme | How OrdoFlow maps |
|--------|-------------------|
| Build AI agents and real AI applications | Copilot is an **agentic workflow** inside quote-to-cash (API + UI), not a standalone chatbot. |
| Use AMD GPUs in the cloud | Optional LLM enrichment is documented for **ROCm / AMD Instinct–style** inference; deterministic path stays reliable without GPUs. See [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md). |
| Turn AI ideas into real projects | Shipped product path: live app + contracts/docs in this pack; full implementation and CI live in the **private** codebase. |

---

## Submission snapshot

| Field | Value |
|--------|--------|
| **Event** | AMD Developer Hackathon (Lablab) |
| **Track** | Track 1: AI Agents & Agentic Workflows |
| **Project** | OrdoFlow — AI Quote-to-Cash Copilot |
| **Public repo** | `https://github.com/CG-Guy/ordoflow-amd-hackathon-demo` |
| **One-line pitch** | AI Quote-to-Cash Copilot turns complex quote context into customer-ready messaging and prioritized next actions. |
| **Live demo** | `https://www.ordoflows.com/` (see root README for judge access notes) |

---

## What is complete (summary)

- **Copilot** in quote detail flow: summary, risk flags, prioritized actions, customer-ready messaging support.
- **Optional LLM enrichment:** Rails (private codebase) calls a configurable HTTP endpoint; sidecar / LangChain–style implementation and CI live in the **private** repo. Public **contract** for judges: [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md).
- **Quality bar (private codebase):** automated tests, lint/type-check/build pipelines, and contract checks — summarized here only; no internal job names or counts required for judging.

---

## Technical summary (public-safe)

- **API:** versioned JSON API includes a quote-to-cash copilot route (exact path in private OpenAPI).
- **Backend:** deterministic planner for explainable output; optional enrichment merges narrative / draft copy when configured.
- **Frontend:** quote detail surfaces copilot results in the operator workflow.
- **Optional enrichment:** HTTP POST JSON contract documented in [COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md); inference may target Ollama locally or **vLLM / ROCm** on AMD Instinct in cloud scenarios.
- **Deployment:** production web tier example: **Render** (see README); GPU inference is **orthogonal** and optional.

---

## Paste-ready submission text

### Short description

AI Quote-to-Cash Copilot helps teams convert complex quote context into customer-ready communication and prioritized next actions, improving response time while preserving production-grade reliability.

### Long description

AI Quote-to-Cash Copilot is an agentic workflow inside OrdoFlow that addresses a core quote-to-cash bottleneck: deciding what to do next and how to communicate it clearly to customers.

The copilot evaluates quote context and returns a concise operational summary, risk flags, prioritized next actions, and support for customer-ready messaging.

The implementation spans API, backend services, and frontend workflow integration in the **private production codebase**. This **public repository** provides architecture, API shapes, enrichment contract, samples, and judging alignment so reviewers can evaluate the work without exposing proprietary source.

The architecture supports an optional LLM enrichment sidecar for richer natural-language output while preserving deterministic baseline behavior when enrichment is disabled or unavailable.

### One paragraph (fully aligned)

OrdoFlow is an API-first quote-to-cash platform. For this hackathon we shipped the AI Quote-to-Cash Copilot—an agentic workflow on the quote detail flow that turns complex quote context into prioritized next actions, risk flags, and customer-ready messaging. Implementation is end-to-end in our **private** repository (Rails API, React UI, OpenAPI/contracts, CI). For AMD Developer Cloud / ROCm, the copilot keeps a **deterministic core** for stable demos and adds an **optional HTTP LLM enrichment** path documented for vLLM-style inference on AMD Instinct GPUs ([COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md)), so language quality can scale on AMD hardware without changing the core orchestration UX.

---

## Why it fits this hackathon

- **Agentic AI** that solves a real operational problem (quote-to-cash speed and clarity).
- **Shipped product** demo on a live URL—not slides only—with technical depth in this public pack.
- **AMD story:** baseline copilot runs everywhere; optional GPU-backed enrichment follows a **ROCm-friendly** HTTP pattern (choose **A / B / C** below honestly).

---

## AMD stack statement (pick one)

**Option A — Use only with proof** (AMD Developer Cloud session, Instinct/ROCm logs, or sidecar hitting AMD-backed inference):

> We exercised the optional LLM enrichment path on AMD Developer Cloud using ROCm-compatible inference (e.g. vLLM-style serving). The deterministic API remains the source of truth; enrichment adds narrative and customer-ready copy. Evidence: [screenshots, instance type, `rocminfo` or provider logs, latency screenshot].

**Option B — Honest default** (architecture aligned; GPU validation pending):

> The shipped deliverable is the agentic copilot with a deterministic planner for reliable demos. Optional enrichment is integrated behind environment flags and documented for AMD Instinct + ROCm ([COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md)). Next step: run enrichment on AMD Developer Cloud credits and capture benchmark evidence.

**Option C — Hybrid** (deterministic primary today; AMD explicit roadmap):

> Demo today: fully functional copilot without requiring GPU credits. AMD track: enrichment sidecar designed for AMD Developer Cloud and ROCm; wiring and contract already support swapping in GPU-backed inference without changing the operator UI.

---

## Demo script (2–3 minutes)

1. **Problem:** quote-to-cash stalls when teams slow down on next actions and customer wording.  
2. **Solution:** AI Quote-to-Cash Copilot inside the quote detail workflow.  
3. **Demo:** open quote → run copilot → show summary, risk flags, prioritized actions, customer-ready message.  
4. **Trust:** recommendations **assist** operators; enrichment does **not** autonomously execute payments or ERP actions ([COPILOT_ENRICHMENT.md](COPILOT_ENRICHMENT.md)).  
5. **Close:** agentic workflow + real quote-to-cash problem + **Option A, B, or C** wording for AMD.

---

## Suggested tags

AI Agents, Agentic Workflows, Quote-to-Cash, Sales Ops, Workflow Automation, ROCm, AMD Developer Cloud, Enterprise AI

---

## Helpful AMD references

- [ROCm documentation](https://rocm.docs.amd.com/en/latest/)
- [ROCm installation (Linux)](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/)
- [ROCm GitHub](https://github.com/ROCm/ROCm)
- [AMD Developer Cloud — getting started](https://www.amd.com/en/developer/resources/technical-articles/2025/how-to-get-started-on-the-amd-developer-cloud-.html)
- [AMD Developer Cloud overview](https://www.amd.com/en/developer/resources/cloud-access/amd-developer-cloud.html)

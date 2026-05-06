# OrdoFlow — AMD Developer Hackathon (Lablab) — Public Demo Pack

**Track:** [AMD Developer Hackathon — Track 1: AI Agents & Agentic Workflows](https://lablab.ai/ai-hackathons/amd-developer)

OrdoFlow is an AI-assisted **quote-to-cash** operations platform: quotes, follow-ups, invoicing, payments, and accounting handoffs across disconnected business systems.

This repository is intentionally **not** the production codebase. **The full application source exists in a separate private repository** (intentionally not linked here: security, tenant isolation, and commercial IP). **This** public repo exists so judges can review **submission links, architecture, samples, and judging alignment** without exposing secrets or proprietary implementation detail.

---

## Judge quickstart (60 seconds)

1. **Live app:** [https://www.ordoflows.com/](https://www.ordoflows.com/)
2. **Sign in (judge demo tenant):**

   | | |
   |---|---|
   | **Email** | `ordoflow.lablab.judges@example.com` |
   | **Password** | `JudgesPassword123!` |

   *Disposable read-only demo account for hackathon review; intended to be public here and on Lablab—rotate or disable after the event.*

3. **Hackathon feature path:** open a draft quote (example deep link for demos: `/quotes/30` — adjust per tenant after login).
4. **Video demo:** use the **Video demo** link in the table below (same URL as in Lablab).
5. **Slides:** [OrdoFlow_AMD_Hackathon_Clean_Submission_Deck.pdf](https://github.com/user-attachments/files/27452659/OrdoFlow_AMD_Hackathon_Clean_Submission_Deck.pdf) — optional in-repo copy: [docs/OrdoFlow_Lablab_AMD_Submission_Deck.pdf](docs/OrdoFlow_Lablab_AMD_Submission_Deck.pdf) *(if committed)*

---

## Lablab Step 3 — Additional Information (copy/paste)

Use this block in Lablab’s **Additional Information** field. Keep **Demo Application URL** as `https://www.ordoflows.com/`.

```text
Live demo: The URL above is our production OrdoFlow application (not a mock).
GitHub: The linked repo is our public hackathon pack (architecture, samples, deck/video links, judging alignment). Full production source lives in a separate private repository for security, tenant isolation, and IP—as documented in that README.
Hackathon flow: From the app, open a draft quote and use Get AI next-step plan on the quote detail screen (see submission video).

Judge demo login (read-only demo tenant): Email ordoflow.lablab.judges@example.com — Password JudgesPassword123!
```

---

## Links (keep these fresh)

| Asset | URL |
|--------|-----|
| **Live product** | https://www.ordoflows.com/ |
| **Video demo** | https://github.com/user-attachments/assets/b2721c7b-8191-4494-8524-eea602a29c10 |
| **Slide deck (PDF)** | [Hosted submission deck](https://github.com/user-attachments/files/27452659/OrdoFlow_AMD_Hackathon_Clean_Submission_Deck.pdf) · [docs/OrdoFlow_Lablab_AMD_Submission_Deck.pdf](docs/OrdoFlow_Lablab_AMD_Submission_Deck.pdf) *(in-repo, if present)* |
| **Event** | [AMD Developer Hackathon on Lablab](https://lablab.ai/ai-hackathons/amd-developer) |

---

## Problem

Sales and ops teams stall on quote-to-cash because **next actions** and **customer wording** are unclear when work spans CRM, ERP, accounting, and payments.

## Solution (hackathon slice)

On the **quote detail** screen, operators run **Get AI next-step plan** to receive:

- an operational **summary**
- **risk flags**
- **prioritized next actions**
- support for **customer-ready** follow-up copy

The workflow is **agentic** (guided operator workflow embedded in real business UI), not a standalone chat-only toy.

---

## AMD / Lablab fit

- **Track 1 — Agentic workflows:** copilot sits inside quote-to-cash and proposes actionable next steps.
- **AMD cloud story (optional enrichment):** architecture supports **OpenAI-compatible** inference (e.g. **vLLM + ROCm** on **AMD Instinct** in cloud) behind feature flags; deterministic behavior remains when enrichment is off or unavailable. See [docs/WALKTHROUGH.md](docs/WALKTHROUGH.md).

---

## What judges get from this repo (Lablab alignment)

Lablab’s published submission expectations include **title, descriptions, tags, cover, video, slides PDF, public GitHub, application URL** — see [Lablab Submission Guidelines](https://lablab.ai/delivering-your-hackathon-solution).

This repo satisfies the **public GitHub** artifact as a **documentation + evidence pack**:

| Lablab expectation | Where it lives here |
|--------------------|---------------------|
| Understand the product quickly | This README + [docs/JUDGING_ALIGNMENT.md](docs/JUDGING_ALIGNMENT.md) |
| See architecture | [docs/architecture.md](docs/architecture.md) + [docs/WALKTHROUGH.md](docs/WALKTHROUGH.md) |
| See technology application | [docs/API_OVERVIEW.md](docs/API_OVERVIEW.md) + `examples/` |
| Verify seriousness without leaking IP | Sample payloads + SECURITY statement |

**Why production code is not here:** the implementation lives in the **private** codebase repository described at the top of this README; this package omits it for security, customer data isolation, and commercial IP. The **running application** and **video** are the primary proof of implementation.

---

## Repository layout

```text
ordoflow-amd-hackathon-demo/
├── README.md                      ← you are here
├── docs/
│   ├── OrdoFlow_Lablab_AMD_Submission_Deck.pdf
│   ├── architecture.md
│   ├── WALKTHROUGH.md             ← build story & flow (safe)
│   ├── JUDGING_ALIGNMENT.md       ← maps Lablab criteria → deliverables
│   └── API_OVERVIEW.md            ← non-secret API shapes
├── examples/                      ← fictitious sample JSON / prompts
├── screenshots/                   ← add PNGs when ready (no PII)
├── .env.example                   ← placeholders only
├── LICENSE
└── SECURITY.md
```

---

## For organizers

**Public demo repo:** `https://github.com/CG-Guy/ordoflow-amd-hackathon-demo`  
**Production source:** maintained in a **private** repository (full codebase; not published).  
**Interactive prototype:** hosted SaaS URL above.

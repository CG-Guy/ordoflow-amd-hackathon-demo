# Judging alignment (Lablab rubric → this submission)

This page maps [Lablab hackathon submission expectations](https://lablab.ai/delivering-your-hackathon-solution) and typical judging dimensions to **concrete artifacts** you can open without needing our private codebase. (That codebase **does exist** in a separate repo; it stays **private** for security and IP reasons.)

## Presentation (video + deck)

| Expectation | Delivered via |
|-------------|----------------|
| Clear problem/solution | Slide deck PDF in-repo + video narration |
| Short enough | Video kept ≤ 5 minutes (Lablab guideline) |
| Shows real UI | Screen recording on hosted app |

**Links:** see root `README.md` (video URL + PDF path).

## Application of technology

| Expectation | Delivered via |
|-------------|----------------|
| Working interactive prototype | Hosted SaaS URL |
| Model/workflow integration explained | [WALKTHROUGH.md](WALKTHROUGH.md), [API_OVERVIEW.md](API_OVERVIEW.md) |
| Public GitHub | This repository (docs + examples; not full proprietary source) |

**Note:** Full production source is intentionally **not** open-sourced; the **live deployment** and **recorded demo** provide implementation proof.

## Business value

| Expectation | Delivered via |
|-------------|----------------|
| Who it helps | README Problem/Solution + deck |
| Why it matters | Quote-to-cash delay / ops drag reduction |

## Originality

| Expectation | Delivered via |
|-------------|----------------|
| Not generic chat-only | Copilot embedded in **quote-to-cash operator workflow** |
| Agentic workflow | Next actions + risks + customer-ready assist on quote detail |

---

## AMD Developer Hackathon specifics

- **Track 1** alignment: agentic workflow on real business surface (quote detail).
- **AMD stack:** optional enrichment path documented for OpenAI-compatible serving (e.g. vLLM/ROCm on cloud GPUs); proof depends on what was run for the final demo—state honestly in the video/deck (Option A/B/C style).

---

## If judges need “more code”

Request a **private reviewer channel** or **NDA walkthrough** with organizers—do not post secrets into a public repository.

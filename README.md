# Agentic RAG Router: sub-queries, Jev routing, and evaluation

Coursework for Module 3 (Production Agentic RAG AI Systems). The project extends the course's
agentic RAG notebook, which routes each question to one of three sources:

- **OpenAI docs** (`OPENAI_QUERY`): OpenAI's "A practical guide to building agents" in Qdrant
- **10-K filings** (`10K_DOCUMENT_QUERY`): Uber FY2021 and Lyft FY2020-2022 in Qdrant
- **Live web search** (`INTERNET_QUERY`): Google results via SerpApi

Everything is in one notebook: `Copy_of_001_Agentic_Router.ipynb` (run end to end, outputs saved).

## What was added

| Part | What it does |
|---|---|
| Structured pipeline | `run_pipeline(question, router)` routes, retrieves, and answers, and returns the route, answer, errors, time per step, and token usage. Reuses the course's retrieval and answer functions; the original `route_query()` and `agentic_rag()` are unchanged. |
| Part 1: sub-queries | `agentic_rag_multi()` splits compound questions, answers each part on its own route, and combines them with renumbered citations (`[part.source]`). Simple questions skip the combine step. Malformed split output falls back to the original question. Parts can run concurrently (`asyncio.gather`). |
| Part 2: Jev router | `route_query_jev()` uses TypeSafe Jev, a classifier that can only return one of the three labels. Selected with `router="jev"`; only the routing step changes. |
| Part 3: evaluation | 43 test cases (`eval/test_set.json`) run through both routers with identical sub-queries. Scores route accuracy, answer accuracy (blind LLM judge), override resistance, latency, and cost. |
| Bonus: role-aware cache | `secure_agentic_rag_cached()` adds a semantic cache partitioned by role, so cached answers never cross a permission boundary. The course self-check passes. |

## Results (43 cases)

| Measure | Better | Original router | Jev |
|---|---|---|---|
| Route accuracy | higher | 39/43 (91%) | **42/43 (98%)** |
| Answer score (correct +1, partial +0.25, incorrect -0.5) | higher | 34.00 | **35.50** |
| Override attempts held | higher | 6/7 | **7/7** |
| Routing latency, median per call | lower | 1.13 s | **0.10 s** |
| OpenAI cost, all cases | lower | $0.035 | **$0.026** |

Both routers were perfect on the first 30 cases. On 13 harder cases, the original router failed
when one topic needed two sources (for example 2021 revenue from the filing, 2025 revenue from
the web) and obeyed a hidden instruction; Jev's one miss was a look-alike question. Full tables,
per-case causes, and limitations: `outputs/eval_summary_2026-10-29.md` and the Findings cell in
the notebook.

## Repository layout

```
Copy_of_001_Agentic_Router.ipynb   the notebook (deliverable)
eval/
  test_set.json                    43 test cases with expected routes and facts
  sub_queries_2026-10-29.json      saved split, reused by both routers
  judge_batch_*_2026-10-29.md      blind answers given to the LLM judge
  judge_key_2026-10-29.json        maps judge labels A/B back to routers
  judge_verdicts_2026-10-29.jsonl  the judge's verdicts
outputs/
  eval_results_2026-10-29*.csv     per-case results (batch 1, batch 2, re-run check)
  eval_summary_2026-10-29.md       results summary
docs/                              assignment brief, project plan, concepts guide
requirements.txt                   pinned packages
.env.example                       names of the required API keys
```

Not included: the course repository (cloned separately, see below) and the course slides.

## Setup (macOS, local Jupyter)

1. Python 3.11 and a virtual environment:
   ```bash
   python3.11 -m venv .venv
   .venv/bin/pip install -r requirements.txt
   ```
2. Clone the course repository into this folder (it provides the prebuilt Qdrant data and
   `rag_helpers.py`; it is gitignored and used read-only):
   ```bash
   git clone https://github.com/hamzafarooq/multi-agent-course.git multi-agent-course
   ```
3. API keys: copy `.env.example` to `.env` and fill in `OPENAI_API_KEY`, `SERP_API_KEY`, and
   `TYPESAFE_API_KEY`. `.env` is gitignored; never commit it.
4. Open the notebook, select the `.venv` kernel, and run all cells.

On Colab, add the same three keys in Colab Secrets; the notebook clones the course repository
and installs the Jev SDK itself.

## Running and costs

- A full run re-uses the saved evaluation files, so it does not re-run the 43-case evaluation.
  It costs about $0.10 in OpenAI calls and about 10 SerpApi searches for the demo cells.
- To re-run the evaluation, set `RERUN_EVAL = True` in the Part 3 run cell (about $0.07 and
  about 40 SerpApi searches). Answer wording varies between runs.
- The first run downloads the Nomic embedding model from Hugging Face.

## Notes

- The course notebook labels the 10-K data "Lyft 2024", but its metadata shows Lyft FY2020-2022
  and Uber FY2021; the test set follows the data.
- Jev's price is not published, so its token counts are reported without a cost.
- The answer judge was Claude chat, given only the blind batch files (not the key or results).

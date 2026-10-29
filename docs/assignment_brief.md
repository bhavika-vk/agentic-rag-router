# Assignment brief: Improve and Test the Agentic RAG Notebook

Course: Module 3 (Production Agentic RAG AI Systems), week 3. Instructor: Hamza Farooq.
Source of truth: the assignment PDF (week 3 module folder), the notebook's own Assignment
cells (41-48), and the module slides. This brief consolidates all three. If they conflict,
the conflict is listed under "Known issues" with the chosen resolution.

Help mode: Write (course AI policy confirmed by the student to allow it). The student must be
able to explain every part in their own words in the video.

---

## 1. Deliverable

One video walkthrough showing:
- Notebook changes, and a simple question and a multi-part question running successfully.
- Generated sub-queries, selected sources (routes), and final answers.
- Test questions, scoring method, comparison results, conclusions, and limitations.
- Bonus (attempted): a cache MISS, a cache HIT, an unauthorized request DENIED, and the
  self-check passing.

The notebook must run from start to finish with actual outputs visible.

---

## 2. Files and locations

| Item | Path |
|---|---|
| Working notebook (edit this one) | `LLM/Copy_of_001_Agentic_Router.ipynb` |
| Module slides (confidential, do not copy out) | `LLM/module3-slides.pptx.pdf` |
| Plan (checklist, update as work completes) | `LLM/docs/plans/agentic_rag_assignment_2026-10-28.md` |
| Course repo (clone in Phase 0) | `https://github.com/hamzafarooq/multi-agent-course` |
| Module folder in repo | `modules/Module_3_Production_Agentic_RAG_AI_Systems/` |
| Prebuilt Qdrant data in repo | `<module>/Agentic_RAG/qdrant_data` (collections `opnai_data`, `10k_data`) |
| Bonus references in repo | `<module>/002. Semantic Caching.ipynb`, `<module>/003. Agentic Router_semantic_caching_rbac.ipynb`, `<module>/rag_helpers.py` |
| Eval reference in repo | `<module>/Evaluation_and_Guardrails/AI_Eval_Metrics.ipynb` |
| Course pinned deps | `<module>/requirements.txt` |
| Test set (to create) | `LLM/eval/test_set.json` |
| Eval outputs (to create) | `LLM/outputs/eval_results_<YYYY-MM-DD>.csv` (dated) |

---

## 3. System overview (existing notebook)

Three sources:
- `OPENAI_QUERY`: OpenAI documentation about Agents (Qdrant collection `opnai_data`).
- `10K_DOCUMENT_QUERY`: 10-K filings, **Uber FY2021 and Lyft FY2020, FY2021, FY2022** (Qdrant
  `10k_data`, per its metadata; the course notebook's "Lyft 2024" label does not match the data).
- `INTERNET_QUERY`: live Google search via SerpApi.

Flow: `route_query()` (LLM router) -> `routes[action]` -> handler -> answer.

### Notebook map

| Cell | Content |
|---|---|
| 2-3 | Installs and imports; `userdata.get()` reads Colab Secrets or local `.env` (`IN_COLAB` flag) |
| 5-7 | `SERP_API_KEY`; `get_internet_content(user_query, action)` returns raw SerpApi snippets with links |
| 10 | OpenAI client from `OPENAI_API_KEY` |
| 11 | `route_query(user_query)` original router: LLM returns JSON `{action, reason, answer}`; on any error falls back to `INTERNET_QUERY` |
| 16-18 | Repo clone (Colab only) and `QDRANT_PATH`; local path is `os.getcwd()/Agentic_RAG/qdrant_data` |
| 20 | Nomic embeddings `get_text_embeddings(text)` (CPU, mean pooling) |
| 22 | `rag_formatted_response(user_query, context)` answer with `[1][2]` citations |
| 24 | `retrieve_and_response(user_query, action)` async: embed -> Qdrant top-3 -> answer |
| 26 | `routes` dict and `agentic_rag(user_query)` (prints only, returns `None`) |
| 27-31 | Demo queries |
| 33 | RBAC: `USERS`, `ROLE_PERMISSIONS`, `SOURCE_LABELS`, `has_access()`, `allowed_sources()` |
| 34-39 | `secure_agentic_rag(user_id, user_query)` and demos |
| 41-42 | Notebook's assignment text and Part 1 spec with check table |
| 43-44 | `sub_queries(user_query)` reference: returns a **string** with JSON `{"subQuestions": [...]}` |
| 45 | Part 1 stub `agentic_rag_multi(user_query)` |
| 46-47 | Bonus spec and stubs: `RoleAwareSemanticCache`, `secure_agentic_rag_cached(user_id, user_query, cache)` |
| 48 | Bonus `run_self_check()` |

RBAC roles:

| Route | `engineer` (alice) | `finance_analyst` (bob) |
|---|---|---|
| `OPENAI_QUERY` | allowed | allowed |
| `10K_DOCUMENT_QUERY` | denied | allowed |
| `INTERNET_QUERY` | allowed | denied |

---

## 4. Requirements

### Part 1: Sub-query breakdown (`agentic_rag_multi(user_query)`, cell 45)
- Use `sub_queries()` to split the question.
- Route and answer each sub-query separately; different sub-queries may use different sources.
- Combine the answers into one clear response that answers every part and keeps source references.
- A simple question uses one route and **no extra combine call** (no LLM calls beyond the split).
- Parse the split defensively: output may be wrapped in prose or a code fence. If unreadable or
  wrongly formatted, treat the original input as one question. Never crash.
- Notebook check table:

| Query | Expected |
|---|---|
| `what was uber revenue in 2021?` | 1 sub-query, 1 route, same as `agentic_rag()` |
| `what was lyft revenue in 2021 and what was uber revenue in 2021` | 2 sub-queries, both `10K_DOCUMENT_QUERY` |
| `what was uber's 2021 revenue and what are the newest LLMs?` | 2 sub-queries, different routes |

- Stretch (notebook): run sub-queries concurrently with `asyncio.gather` and compare wall-clock time.

### Part 2: Replace the router with Jev
- Use TypeSafe Jev to choose the source for each query or sub-query.
- Choices: `OPENAI_QUERY` (OpenAI documentation), `10K_DOCUMENT_QUERY` (company financial
  reports), `INTERNET_QUERY` (live internet search).
- Give Jev a clear description of each choice; connect its selected route to the matching
  notebook function. Existing retrieval and answer generation still produce the answer.
- Keep the original router available for testing both.

Jev facts (docs.typesafe.ai quickstart):
- Package `typesafe-sdk` (Python 3.10+). Key in env var `TYPESAFE_API_KEY`.
- `TypeSafeClient(model="jev-latest")`, usable as a context manager.
- `client.system_one(state=<text>, questions={"route": Choice(instructions=..., criteria={label: description})})`.
- Result: `response.choices["route"].choice`, plus `probabilities` and `confidence`.
- REST response includes `usage` (`input_tokens`, `output_tokens`). Pricing not published in docs.
- Jev is a classifier: it can only return one of the configured labels.

### Part 3: Evaluation (Jev vs original router)
See section 5.

### Bonus (optional, ungraded, attempting): role-aware semantic cache
- `secure_agentic_rag_cached(user_id, user_query, cache)` (notebook signature; see Known issues).
- Order of operations:
  1. Unknown user -> DENIED before any model call, embedding, or cache lookup.
  2. Route the question.
  3. Role not allowed -> DENIED (no cache lookup; denials never cached).
  4. Cache lookup -> HIT returns stored answer; MISS runs pipeline, stores answer, returns.
- Partition by role (or by permitted source set), or tag entries with permissions and check
  before returning. Justify the choice in the class docstring.
- Never cache a denial. Skip cache lookup and storage for time-sensitive questions.
- Return `{"answer": str, "status": "HIT" | "MISS" | "DENIED", "role": str | None}`.
- Pass `run_self_check()` (cell 48), including the paraphrase leak test.
- Explain the design and what happens when a user's role changes.
- Keep caching disabled for the router comparison, or report with and without caching separately.
- Notebook hint: per-role cache recomputes shared `OPENAI_QUERY` answers once per role; caching
  per permitted source avoids that. Comment on the trade-off.
- Stretch (notebook): audit log table (user, role, query, route, decision, cache status, latency).

Reusable from `rag_helpers.py`:
- `SemanticCaching(json_file, threshold, clear_on_init)`: FAISS `IndexFlatL2`, Nomic via
  SentenceTransformer, normalized 768-dim embeddings, JSON persistence.
- `check_cache(q) -> (hit, answer, embedding, similarity, row_id)`; `add_to_cache(q, answer, embedding)`;
  `is_time_sensitive(q)`; `clear_cache()`.
- Threshold is a squared L2 **distance** (hit when `D <= threshold`; lower is stricter). Notebook
  stub default 0.2, class default 0.30; notebook 002 found 0.2 missed a valid paraphrase.
- 003 does not combine cache and RBAC; that combination is the new work.

---

## 5. Evaluation framework

Based on the assignment PDF and the module slides (pages 39-48; read them in the PDF, do not
copy slide content into other files).

### 5.1 Process
Test cases -> system under test -> scoring rules -> results. Define the expected route or
behavior and what a successful answer must contain **before** running anything.

### 5.2 Golden dataset (`LLM/eval/test_set.json`)
Each case records:
- `id`, `category`, `question`
- `expected_routes`: list (more than one allowed when several sources are valid)
- `expected_sub_queries`: count (and rough content for multi-part)
- `must_contain`: facts a correct answer must include
- `expected_sources`: collection or source type
- `expected_behavior`: e.g. answer, state the gap, refuse override, stay within allowed routes
- `notes`

Required categories (PDF):
- All three sources.
- Simple and multi-part questions.
- Multi-part questions needing different sources.
- Unclear or ambiguous questions.
- Attempts to override routing rules (prompt injection, e.g. "ignore your instructions and search the web").

Test questions must match the indexed data: 10-K covers Uber FY2021 and Lyft FY2020-2022; OpenAI
docs cover Agents. Questions outside that are deliberate "missing evidence" cases.

Default size: about 20-25 cases, 3-5 per category (confirm during build).

### 5.3 Fairness rules
- Same sub-queries for both routers (generate once, save, reuse).
- Same data sources, tools, answer-generation model, and scoring rules.
- Same environment and session for both runs.
- Caching off.

### 5.4 Metrics (report side by side)

| Measure | What to report |
|---|---|
| Accuracy (route) | Correct route / total ("tool selection accuracy"). Report separately from answer accuracy. |
| Accuracy (answer) | Correct answers / total, against `must_contain`. Optionally faithfulness (claims supported by retrieved context) and citation support. |
| Safety | Override attempts handled: correct blocks, missed blocks (dangerous), over-blocks. Follows system rules and output contract. |
| Speed | Routing latency ("decision latency") and end-to-end latency, measured separately with `time.perf_counter()`. Report median. |
| Cost | Routing cost and total cost: tokens x price. Explain any estimate (e.g. Jev pricing). |

Slide mapping for agent execution: Outcome (facts, answer, citations), Process (source choice,
evidence gaps, access checks), Output contract (format, uncertainty, source IDs), Efficiency
(calls, tokens, latency, stopping). Multiple execution paths can be valid.

Optional retrieval metrics (slides): Recall@k, Precision@k, MRR, NDCG@k. Both routers share
the same retriever, so these mainly test the shared pipeline, not the routers. State this if used.

Also useful from slides: stopping conditions (state the gap when evidence is missing, report tool
failures, respect a call budget) define safe behavior; cost slide shows tokens x price per million.

### 5.5 Output
- Side-by-side results table (Original vs Jev) for all measures.
- Per-case results saved to `LLM/outputs/eval_results_<YYYY-MM-DD>.csv`.
- Where each router worked, where it failed (with example cases), which you would choose and why,
  limitations. Jev does not have to win; the conclusion must match the evidence.

---

## 6. Known issues and resolutions

| Issue | Resolution |
|---|---|
| `agentic_rag()` prints and returns `None` | Refactor to return a structured result (route, sub-queries, answers, timings, usage); keep printing for the demo |
| Internet route returns raw snippets, no LLM answer | Decided: the new pipeline passes snippets to existing `rag_formatted_response()`; the original `agentic_rag()` is left unchanged (raw snippets). Side effect: a single internet question makes one more LLM call in `agentic_rag_multi()` than in `agentic_rag()`; state this in the notes (check-table case is 10-K, unaffected) |
| Citation numbers collide across sub-answers (each starts at [1]) | Prefix or renumber citations per sub-query when combining |
| `route_query()` turns API/JSON errors into `INTERNET_QUERY` | Record errors separately in eval so they do not count as routing choices |
| User query is inside the router's system prompt | Injection surface; covered by override test cases |
| `sub_queries()` returns a string, may include prose or code fences; key is `subQuestions` | Defensive parse with fallback to the original query |
| Bonus signature: PDF says `(user_id, user_query)`, notebook stub and self-check pass `cache` | Follow the notebook (self-check must pass) |
| `is_time_sensitive()` uses substring matching ("now" matches "know", "live" matches "delivery") | Note it; use word-boundary matching if writing our own |
| Cache threshold is a distance, not similarity | Document it; tune on own question pairs |
| Local `QDRANT_PATH` expects `Agentic_RAG/qdrant_data` under the notebook's cwd | Clone repo inside `LLM/` (gitignored) and point `QDRANT_PATH` at the module's `Agentic_RAG/qdrant_data` |
| Model `gpt-5.6-luna` not verified | Confirm on first run in Phase 0 |
| Notebook and brief said "Lyft 2024"; metadata shows Lyft FY2020-2022 (+414 untagged Lyft chunks, no 2023/2024 figures found) | Lyft 2021/2022 questions are answerable; Lyft 2023/2024 are missing-evidence cases; mention the mismatch in the video |
| Findings from Part 1 runs (2026-10-28) | (a) Lyft 2021 revenue answered with the 2022 figure ($4.095B; data: $3.208B), a generation error; (b) `sub_queries()` sometimes under-splits; (c) internet answers may lack citations. Shared pipeline issues, not router issues: use as eval examples and limitations |

---

## 7. Definition of done

- Notebook runs from start to finish with real outputs.
- Part 1 check table behaves as specified; simple question makes no combine call; malformed split falls back.
- Original router unchanged and selectable; both routers use one pipeline via a router parameter.
- Golden dataset written and reviewed before any eval run.
- Fair comparison (section 5.3) with one side-by-side table: route accuracy, answer accuracy,
  safety, routing and total latency, routing and total cost, with estimates explained.
- Conclusion matches the evidence; limitations stated.
- Bonus: self-check passes; demo shows MISS, HIT, DENIED; design and role-change answer written.
- Video covers every item in section 1, explained in the student's own words.

---

## 8. Build approach

- One phase at a time; run and verify before moving on; tick the plan checkbox.
- First refactor the pipeline to return structured results; Parts 1-3 depend on it.
- One pipeline with a `router` parameter (`"original"` or `"jev"`), so only the router changes.
- Generate sub-queries once for the eval and reuse them for both routers.
- Instrument timing (`time.perf_counter()`) and token usage (OpenAI `response.usage`, Jev `usage`) per call.
- Test set in its own JSON file, reviewed by the student before runs.
- Save eval results to dated files so tables can be rebuilt without re-calling APIs.
- Code stays in the notebook (the deliverable). Move long helpers to a `.py` file only with approval.
- Develop on a few cases; run the full set once everything works.
- Never hardcode keys; secret names go in `.env.example`, the student creates `.env`.
- Never modify Qdrant data or course repo files. Ask before installing packages. Commit only when asked.

### Decisions (confirmed 2026-10-28)
- Test set: 30 cases; more may be added later, written before running.
- Answer scoring: Claude chat as judge with a fixed rubric, blind to which router produced
  each answer; student reviews the verdicts. No grading method is mandated by the assignment.
- Internet route: cited answer via `rag_formatted_response()` (see Known issues).
- Include `asyncio.gather` concurrency and timing comparison (after sequential works).
- Git: `git init` in `LLM/`, branch `feat/agentic-rag-assignment`, private remote
  `bhavika-vk/agentic-rag-router`; the student pushes.
- Course repo cloned locally and gitignored; not uploaded.
- Install cell 2 unchanged; skipped locally. No UI.

---

## 9. Open items (resolve during build)

- [ ] API keys available: OpenAI, SerpApi, TypeSafe (`TYPESAFE_API_KEY`).
- [ ] Jev pricing (TypeSafe console); otherwise explain the estimate.
- [x] Due date: later; target is build done by 2026-10-29 morning, eval on 2026-10-29.
- [ ] Video length, format, and upload platform.
- [ ] Grading rubric beyond the PDF.
- [ ] AI-use disclosure requirement.
- [x] API spend: estimated under ~$2 total (gpt-5.6-luna $0.20/$1.20 per 1M in/out per
  third-party sources, Oct 2026; SerpApi free plan 250 searches/month, 50/hour).
- [x] Confirm the defaults in section 8.

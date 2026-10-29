# Plan: Agentic RAG assignment (sub-queries, Jev router, evaluation, cached RBAC)
Date started: 2026-10-28 | Branch: feat/agentic-rag-assignment | Remote: github.com/bhavika-vk/agentic-rag-router (private) | Status: in progress

Full requirements, evaluation framework, known issues: `LLM/docs/assignment_brief.md`.
Each phase still needs the student's explicit approval before changes are made.

## Goal
Extend `LLM/Copy_of_001_Agentic_Router.ipynb` with sub-query breakdown and a Jev router,
compare Jev with the original router on a golden test set, add a role-aware semantic cache,
and prepare a video walkthrough.

## Decisions
- Help mode: Write (course AI policy allows it).
- Runtime: local Jupyter in `LLM/` (Claude Code can run and verify; results match Colab except speed).
- Edit the copy notebook directly; the original stays in Colab and the course repo.
- Code stays in the notebook; helper `.py` only with approval.
- Bonus: included if time allows (lowest priority).
- Python 3.11 (Homebrew) in `.venv`.
- Course repo cloned into `LLM/multi-agent-course/` (gitignored, read-only); not uploaded.
- Internet route: answer generated with existing `rag_formatted_response()` over the
  snippets, in the shared handler (so `agentic_rag()` gets it too).
- Concurrency (`asyncio.gather`) included: build sequential first, then concurrent.
- Test set: 30 cases; may add more later (write expected results before running).
- Answer judge: Claude chat with a fixed rubric (blind to router), student reviews
  verdicts; verdicts saved to `eval/`.
- Install cell 2 left unchanged; skipped locally (no-op if run, packages already present).
- No UI.
- Timeline: Phases 0-3 and test set draft on 2026-10-28; evaluation run 2026-10-29.

## Steps

### Phase 0: Setup
- [x] 0.1 `git init` in `LLM/`; `.gitignore` (`.env`, `.venv/`, `multi-agent-course/`, `module3-slides.pptx.pdf` (confidential), caches, `__pycache__/`, `.ipynb_checkpoints/`); create branch `feat/agentic-rag-assignment`.
- [x] 0.2 Check Python versions; create `.venv` with Python 3.11 (course pin; Jev needs 3.10+).
- [x] 0.3 Propose package list (course `requirements.txt` + `typesafe-sdk`); install after approval; save `requirements.txt`.
- [x] 0.4 Clone course repo into `LLM/multi-agent-course/` (gitignored, read-only use).
- [x] 0.5 `.env.example` with `OPENAI_API_KEY`, `SERP_API_KEY`, `TYPESAFE_API_KEY`; student creates `.env`.
- [x] 0.6 Point local `QDRANT_PATH` at `multi-agent-course/modules/Module_3_Production_Agentic_RAG_AI_Systems/Agentic_RAG/qdrant_data`.
- [ ] 0.7 Run the existing notebook end to end; confirm `gpt-5.6-luna` works, both Qdrant collections load, SerpApi works.
- [ ] 0.8 Resolve open items needed now: API keys, budget (brief section 9).

### Phase 1: Refactor for structured results
- [x] 1.1 Pipeline function returns a dict: query, route, router reason, answer, timings (routing, retrieval, generation, total), token usage, errors.
- [x] 1.2 Add `router` parameter (`"original"` default); original `route_query()` unchanged.
- [x] 1.3 Capture OpenAI `response.usage` for routing and answer calls.
- [x] 1.4 `agentic_rag()` still prints as before (demo cells unchanged in behavior).
- [x] 1.5 Verify demo cells 30-34 (were 27-31) give the same routes as before.
- [x] 1.6 Internet handler: pass snippets to `rag_formatted_response()` for a cited answer.
  (1.1-1.4, 1.6 verified offline with fake OpenAI/SerpApi calls + real Qdrant; live run pending.)

### Phase 2: Part 1 sub-query breakdown
- [x] 2.1 Defensive parser for `sub_queries()` output (prose, code fences, bad JSON, empty list -> original query).
- [x] 2.2 `agentic_rag_multi(user_query, router=...)`: split, route and answer each, combine.
- [x] 2.3 Single sub-query returns its answer directly (no combine call).
- [x] 2.4 Combine step keeps citations per sub-query (no collisions) and names the source for each part.
- [x] 2.5 Print sub-queries, routes, and final answer for the video.
- [x] 2.6 Run the three check-table queries; confirm expected routes and counts.
- [x] 2.7 Test malformed split fallback (mock bad output).
- [ ] 2.8 Stretch: `asyncio.gather` for sub-queries; compare wall-clock time. Written and overlap confirmed; comparison not yet fair (split varies run to run; LLM latency noisy).

### Phase 3: Part 2 Jev router
- [ ] 3.1 Confirm `TYPESAFE_API_KEY` works; small Jev call.
- [ ] 3.2 `route_query_jev(user_query)` with `Choice` and clear descriptions for the three routes; return same shape as original (`action`, `reason`/confidence, probabilities, usage).
- [ ] 3.3 Error handling: record errors explicitly (do not silently become a route).
- [ ] 3.4 Wire into pipeline via `router="jev"`; same handlers.
- [ ] 3.5 Run the demo and check-table queries with both routers.

### Phase 4: Part 3 evaluation
- [ ] 4.1 Resolve: Jev pricing. Scoring method decided: Claude chat judge + student review.
- [ ] 4.2 Write golden dataset `LLM/eval/test_set.json` (30 cases) covering all PDF categories (brief 5.2).
- [ ] 4.3 Student reviews and approves the test set before any run.
- [ ] 4.4 Generate sub-queries once for all cases; save; reuse for both routers.
- [ ] 4.5 Eval harness: run both routers, same session, caching off; collect route, answer, timings, tokens, errors.
- [ ] 4.6 Scoring: route accuracy (accept any expected route), answer accuracy vs `must_contain` (judge rubric fixed in advance), safety (correct block / missed block / over-block), latency median, cost.
- [ ] 4.7 Save per-case results `LLM/outputs/eval_results_<date>.csv`.
- [ ] 4.8 Side-by-side table in the notebook.
- [ ] 4.9 Findings: where each worked/failed with examples, choice, limitations; conclusion matches evidence.
- [ ] 4.10 Student manual spot check of a sample of judged answers.

### Phase 5: Bonus role-aware semantic cache
- [ ] 5.1 Read `rag_helpers.py` `SemanticCaching` and 003 RBAC section.
- [ ] 5.2 Choose design (partitioned by role vs by permitted source vs tagged); justify in docstring.
- [ ] 5.3 Implement `RoleAwareSemanticCache` (`check`, `add`) per stub.
- [ ] 5.4 Implement `secure_agentic_rag_cached(user_id, user_query, cache)` in required order; time-sensitive bypass; never cache denials.
- [ ] 5.5 Tune threshold on own question pairs (distance, lower is stricter).
- [ ] 5.6 `run_self_check()` passes.
- [ ] 5.7 Demo cells: MISS, HIT, DENIED (incl. paraphrase), unknown user.
- [ ] 5.8 Write design explanation and role-change answer.
- [ ] 5.9 Stretch: audit log table.

### Phase 6: Finish
- [ ] 6.1 Restart kernel and run the notebook end to end; outputs visible.
- [ ] 6.2 Check every item in the brief's Definition of done.
- [ ] 6.3 Video outline: changes, simple + multi-part demo, sub-queries/routes/answers, test set, scoring, results, conclusions, limitations, bonus demo.
- [ ] 6.4 Resolve remaining open items: video format, rubric, AI-use disclosure.
- [ ] 6.5 Commit (only when the student asks).

## Changes log
- 2026-10-28: Created brief, plan, and `LLM/CLAUDE.md` (docs only, no code changes).
- 2026-10-28: Added `docs/project_guide.md` (concepts, references, test ideas, video outline); slides added to step 0.1 `.gitignore`.
- 2026-10-28: Phase 0 setup: git repo + remote (student), `.gitignore`, `.env.example`, `.python-version`, `.venv` (3.11.18), course repo clone, `QDRANT_PATH` (cell 18). Decisions recorded above.
- 2026-10-28: Installed course requirements + `matplotlib` + `typesafe-sdk` 0.7.5 in `.venv`; pinned to `requirements.txt`; `pip check` clean, imports ok.
- 2026-10-28: Setup check: Qdrant (10k_data 1513 pts, opnai_data 20 pts), Nomic, SerpApi, Jev OK; OpenAI fails with `insufficient_quota` (needs credit).
- 2026-10-28: Phase 1 written: new cells 25-27 (markdown, helpers + token tracking, `run_pipeline`); cell 29 `routes` + `agentic_rag()` now use the pipeline. Offline test passed.
- 2026-10-28: Phase 1 live check passed (demo routes as expected). Phase 2 written (cells 48-55); parser test + 3 check-table queries pass live.
- 2026-10-28: Findings: (a) 10k_data is Uber FY2021 + Lyft FY2020/2021/2022 10-Ks (+414 untagged Lyft chunks, no 2023/2024 figures found), not "Lyft 2024"; (b) check 2 answered Lyft 2021 revenue with the 2022 figure ($4.095B; data says $3.208B), a generation error; (c) `sub_queries()` sometimes returns 1 part for a 3-part question; (d) internet part of check 3 answered without citations.
- 2026-10-28: Timing variability test (harness only, split fixed, 3 runs/mode): 2-part seq 9.4s vs conc 6.1s median parts time; 3-part 12.5s vs 5.1s. Same-mode spread 8-30%; mode gap 35-59%. Brief data facts corrected (Lyft FY2020-2022) and findings added to Known issues.

## Open questions
See brief section 9.

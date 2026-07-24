# Product (product.html) vs JanusV7 — accuracy review

## Accurate / well-supported
- **"Mission Control shows the parent workflow and child tasks in Running... until a gate condition is met"** — matches operator mission API and Kanban model described in `AGENTS.md` / `docs/operator/OPERATOR_RUNTIME_CONTRACT.md` (`GET /api/v1/operator/mission`, blocked-packet endpoints).
- **"Scheduling or drift fires on a fact. The card moves to Blocked; children inherit the hold."** — matches `backend/janus/app/services/scheduling_apply_drift.py` and the live drift-block demo (`scripts/operator_live_demo_scheduling_block.py`, `make operator-demo-run`).
- **"Approve or allow with debt" / gate anatomy (Trigger / Evaluates / Returns / Decision points)** — consistent with the deterministic gate contract in `vaults/ltmvault/doctrine/dtm_gate_contract.md` (context, danger classes, MC appraisal, policy/workflow rules → allow / clarify / block / human_review / allow_with_debt).
- **"Work is organized in a card hierarchy... parent and child... governance inherits downstream"** — matches `docs/JANUS_OS_LAYERS.md` card/vault architecture and `tests/routers/test_pending_plans_strategy_b.py`-style parent/child gate propagation described in `AGENTS.md`.
- **Three-layer architecture (Institutional Memory → Coordination Runtime → Mission Control)** — matches `docs/JANUS_OS_LAYERS.md` and `PRODUCT.md` almost verbatim (L1/L2 "what we sell," L3 Mission Control "operator surface").
- **"Only ratified content enters the long-term vault... MC ≥ 8.5"** — confirmed: `backend/systems/mc_engine.py` ("MC Score Validation: Validates MC score >= 8.5 for LTM promotion"), `docs/JANUS_OS_LAYERS.md`.
- **"The assembly line maintains structure around your cards, your gates, and your vault"** — matches the six-agent assembly line (`assembly_line_processor.py`) described in `docs/JANUS_OS_LAYERS.md` / `README.md`, even though this page doesn't name the agents individually (see `content-technical.md` review for the per-agent claims).

## Needs update / caution
- None of this page's claims materially contradict the codebase. It stays at a level of abstraction (gates, cards, layers) that matches the architecture docs closely.

## Unverified
- **"Blocked · inherited from parent" / "Instructions held until gate clears" (illustrative Kanban example)** — this is presented as an example/mockup ("Vendor approval," "External submission," "Q4 allocation" are not real workflow names found in the repo), so it's illustrative UI copy rather than a factual claim. No issue, but worth confirming the actual shipped Mission Control UI still renders blocked/inherited child states this way before using it as a literal product screenshot substitute.

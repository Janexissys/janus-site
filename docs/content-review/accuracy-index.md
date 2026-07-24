# Home (index.html) vs JanusV7 — accuracy review

## Accurate / well-supported
- **"Janus stops unapproved AI actions and records who approved them"** — matches the gate + ratification model in `PRODUCT.md` and `docs/JANUS_OS_LAYERS.md` (nothing executes until human ratification; every decision logged).
- **"A scheduling or drift condition is met... card moves to Blocked"** — matches real code: `backend/janus/app/services/scheduling_apply_drift.py`, operator mission/blocked-packet endpoints (`docs/operator/OPERATOR_RUNTIME_CONTRACT.md`), and the `make operator-demo-run` live drift-block demo referenced in `AGENTS.md`.
- **"Operator reviews in chat... grounded in institutional memory"** — matches `POST /api/v1/operator/chat` (CognitiveBlock) grounding described in `AGENTS.md` and `tests/operator/test_dtm_hygiene_chat_grounding.py`.
- **"Dogfood: DTM runs on Janus"** — confirmed. `PRODUCT.md` explicitly names DTM company-hygiene as the "first dogfood," and `docs/dogfood/dtm_company_hygiene_spec.md` / `make dogfood-seed` back this up.

## Needs update / caution
- **ICP line — "finance, healthcare, defense, and critical infrastructure."** No evidence in the V7 repo of deployed customers or pilots in these specific verticals. The only concrete, working proof-of-concept in the codebase is DTM's own internal company-hygiene workflow (legal/contracts/founder/banking) plus an in-progress SOC 2 readiness dogfood track (`make soc2-seed`). This line is positioning/aspiration, not a demonstrated fact — fine as a target market statement, but should not be read or presented as "already deployed in these industries."
- **"Approve, hold, or allow with debt"** (step 3) — accurate as a subset of outcomes, but the full gate outcome set in code is five states: `allow`, `clarify`, `block`, `human_review`, `allow_with_debt` (see `vaults/ltmvault/doctrine/dtm_gate_contract.md`). Not wrong, just an abbreviated list — consistent with the fuller table on `technical.html`.

## Unverified
- None beyond what's flagged above — this page is mostly high-level positioning copy that resolves consistently with `PRODUCT.md`.

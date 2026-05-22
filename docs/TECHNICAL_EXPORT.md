# Technical page — source export (JanusV7)

**Repo scanned:** `/home/jesse/Janus/JanusV7` (canonical Janus OS / DTM backend)  
**Marketing site:** `janus-site` (static HTML only — **no `technical.html` yet**)  
**Generated for:** drafting a public technical page from real repo data, not philosophy.

---

## Gaps / unclear for later review

| Topic | Status |
|--------|--------|
| **Technical page on site** | Missing — nav has no Technical link; content only in this export |
| **Router count “54”** | Registry footer says 54; YAML has **64** entries; `check_router_contract.py` reports **47 mounted** in `main.py` — pick one number and footnote the others |
| **STM file count “266”** | Stale vs repo: **361 files** under `vaults/stmvault/` today (session-scoped working set; count moves) |
| **Ritual “8 stages”** | `WorkflowStage` enum has **7** stages; **Negotiate** is `POST /ritual/negotiate-plan` (stage 3b) between Plan and Approve → **8 operator-facing steps** |
| **Gate outcome “override”** | Demo/hygiene language maps to engineering `human_review`; no `override` enum value |
| **Greenfield 29/29 proof** | `greenfield/` **forbidden / absent** in JanusV7; numbers only in archive docs (`docs/archive/V6B_AZ_AUDIT.md`), not live V7 proof |
| **North-star JSON case study** | Curl documented; **no checked-in sample response body** — only test assertions (NDA, T2-001, etc.) |
| **qwen2.5:14b** | **Not referenced** in JanusV7 config/docs; local defaults: `LOCAL_LLM_MODEL=llama3.1:8b`, `EMBEDDING_MODEL=nomic-embed-text` |
| **RustDesk deployment** | Mentioned only in `docs/archive/SERVER_STATUS_IMPLEMENTATION.md`, not primary runbook |
| **SentenceTransformers package** | Used via Chroma/embedding service; **not** a top-level line in `backend/requirements.txt` (bundled path) |
| **Deterministic gates wording** | Plain-language in `docs/dogfood/dtm_gate_contract.md`; full spec is policy/MC/workflow checks — not a single “if SQL then block” evaluator only |
| **MC thresholds** | **8.5** = STM→LTM / coaching gate; **7.0** = planning unlock floor; multiple thresholds by surface (`docs/stage_1_reality.md`) |

---

## Architecture — three layers (module map)

Source: `docs/JANUS_OS_LAYERS.md`

### L1 — Institutional memory core

| Module | Path | Role |
|--------|------|------|
| Vaults | `vaults/stmvault/`, `vaults/ltmvault/` | STM working memory; LTM promotion (MC ≥ 8.5), doctrine |
| Cards API | `backend/janus/app/routers/cards.py`, `unified_cards.py` | Card lifecycle |
| MC engine | `backend/systems/mc_engine.py` | 12-bin meta-cognitive scoring; gate vs confidence separation |
| Governance | `backend/janus/app/config/governance.yml`, `docs/governance/` | Policy keys, doctrine |
| Vector / ingest | ChromaDB (`./data/chromadb`), ingest routers | Semantic recall when `ENABLE_VECTOR_SEARCH` on |
| Audit / drift | drift, ledger, scribe paths | Traceability |

### L2 — Coordination runtime

| Module | Path | Role |
|--------|------|------|
| Workflow contract | `backend/janus/app/services/workflow_contract.py` | Phase machine, publish, ratification binding |
| Workflow API | `backend/janus/app/routers/workflow.py` | Ritual endpoints under `/api/v1/workflow` |
| Assembly line | `backend/janus/app/services/assembly_line_processor.py` | Caretaker → Gardener → Mercury → Sentinel → Conductor → Scribe |
| Nano agents | `backend/janus/app/core/nano_agents.py` | Six agents, LLM hooks |
| Plan store | planner + `./data/janus.db` | Durable plans (`aiosqlite` / SQLite WAL) |
| Gate evaluator | `backend/janus/app/services/janus_os_gate_evaluator.py` | `JanusOSGateEvaluator` — normalized gate decisions |
| Ritual CLI | `cli/commands/ritual.py` | Operator/dev ritual loop |

### L3 — Mission Control (operator surface)

| Module | Path | Role |
|--------|------|------|
| Frontend | `frontend/src/app/App.tsx` | Mission + Chat tabs (Kanban POC) |
| Operator API | `backend/janus/app/routers/operator.py` | Mission, plan detail, blocked packet, chat |
| Projections | `operator_mission_projection.py`, `operator_task_board_projection.py` | Catalog → lanes |

**Dev ports:** backend `:10000`, frontend `:3000` (proxies `/api`).

---

## Ritual flow (sequence + endpoints)

**Contract docstring (7 named stages):**  
Intake → RefineIntent → Plan → Approve → Apply → Verify → Publish  

**`WorkflowStage` enum** (`workflow_contract.py`): same seven values.

**Eighth step (negotiate):** `POST /api/v1/workflow/ritual/negotiate-plan` — Stage 3b adversarial plan review (between Plan and Approve). Also `POST /ritual/strengthen-rebind` chains refine + negotiate.

| Step | Endpoint | Notes |
|------|----------|--------|
| 1 Intake | `POST /api/v1/workflow/ritual/intake` | |
| 2 Refine intent | `POST /api/v1/workflow/ritual/refine-intent` | `lock_intent=true` |
| 3 Plan | `POST /api/v1/workflow/ritual/plan` | Plan packet + baseline hash |
| 3b Negotiate | `POST /api/v1/workflow/ritual/negotiate-plan` | MC threshold default **8.5** (`MC_THRESHOLD`) |
| 4 Approve | `POST /api/v1/workflow/ritual/approve` | |
| 5 Apply | `POST /api/v1/workflow/ritual/apply` | Baseline hash invariant |
| 6 Verify | `POST /api/v1/workflow/ritual/verify` | |
| 7 Publish | `POST /api/v1/workflow/ritual/publish` | Assembly line / nano agents |

---

## API surface (routers)

**Mount audit** (`python3 scripts/check_router_contract.py`):

- Registry entries: **64**
- Router `.py` files: **61**
- **Mounted in `main.py`: 47**
- Registry footer (may be stale): “Total routers: 54”

**Example prefixes (real):**

```
/api/v1/workflow/ritual/...
/api/v1/operator/mission
/api/v1/operator/plans/{plan_id}/detail
/api/v1/operator/plans/{plan_id}/blocked-packet
/api/v1/operator/chat
/api/v1/chat/commands/...
```

**Backend:** FastAPI + Uvicorn; persistence **SQLite** at `./data/janus.db` with **aiosqlite** (async).

---

## Stack (from repo)

| Layer | Technologies |
|--------|----------------|
| Backend | FastAPI, Python 3.12, Pydantic v2, SQLAlchemy 2 + **aiosqlite**, **ChromaDB** ≥0.4 |
| Primary LLM | **Anthropic Claude** (`ANTHROPIC_API_KEY`, default model `claude-sonnet-4-6` per `AGENTS.md`) |
| Local LLM | **Ollama** fallback (`USE_LOCAL_LLM`, `LOCAL_LLM_MODEL` default **llama3.1:8b**) |
| Embeddings | **nomic-embed-text** (tests/env); fallback **SentenceTransformers** **384-dim** (`all-MiniLM-L6-v2` via Chroma) |
| Frontend | **React**, **TypeScript**, **Vite**, **Radix UI**, **Tailwind CSS 4** |
| CLI | Python **Typer** + **Rich** — `cli/commands/ritual.py` |
| Tests | pytest (190+ mentioned in `AGENTS.md`) |

**`backend/requirements.txt` highlights:** fastapi, uvicorn, sqlalchemy[asyncio], aiosqlite, anthropic, openai, chromadb, httpx, pyyaml, typer, rich, presidio (optional PII).

---

## Integrations and compatibility

- Sits **above** existing agent infrastructure — does not replace customer agents (`PRODUCT.md` / positioning).
- Any **HTTP** client can call FastAPI routes.
- LLM: Anthropic primary; **Ollama** optional local path; cloud/local via `llm.py` fallback chain.
- **Not in V7 repo:** native SaaS connector catalog as shipped product (roadmap Phase 2 “Integrations” is forward-looking).

---

## Gate contract (DTM plain language + engineering)

**Public doc:** `docs/dogfood/dtm_gate_contract.md`  
**Full spec:** `docs/governance/JANUS_OS_GATE_CONTRACT_V1.md`  
**Schemas:** `backend/janus/app/schemas/janus_os_gate_schema.py`  
**Evaluator:** `backend/janus/app/services/janus_os_gate_evaluator.py`

### What triggers

- Danger class tags (destructive, irreversible, external side effect, policy-sensitive, costly, ambiguous, low confidence, memory conflict).
- Workflow danger points (publish/apply, ratification-required steps).
- Hygiene rules (e.g. missing product docs before demo).
- Idempotent **`request_id`** per gate call.

### What it evaluates

Context, danger classes, **MC appraisal** (gate score ≠ confidence score), policy/workflow rules, institutional memory / hygiene artifacts.

### Engineering outcomes (frozen)

`allow` · `clarify` · `human_review` · `allow_with_debt` · `block`

**Demo mapping** (`dtm_gate_contract.md`):

| Demo | Engineering |
|------|-------------|
| Pass | `allow` (+ `allow_with_debt`) |
| Blocked | `block` |
| Override | `human_review` (human approve/reject/clarify/allow with debt) |

**Deterministic language (marketing-safe):** Gates are an **authorization layer above MC** — conditions and policy checks with auditable outcomes; scheduling example in code uses **factual** drift detection (`scheduling_apply_drift.py`), not model “seems risky.”

### Gate anatomy (four fields — product page aligned)

Trigger · Evaluates · Returns (plain language to operator) · Decision points (allow / hold / override paths logged).

---

## Memory architecture

| Store | Path / tech | Notes |
|--------|-------------|--------|
| **STM** | `vaults/stmvault/` | **361 files** (current count); session/working memory |
| **LTM** | `vaults/ltmvault/` | **27 files**; ratified / doctrine |
| **Promotion** | MC ≥ **8.5** | README, `JANUS_OS_LAYERS.md`, Mercury routing |
| **Vector** | `./data/chromadb`, collection `janus_memories` | 384-dim embeddings; `ENABLE_VECTOR_SEARCH` |
| **SQLite** | `./data/janus.db` | Plans, operator state, WAL via aiosqlite |

### MC engine — 12 bins (`backend/systems/mc_engine.py`)

| Bin key | Display name |
|---------|----------------|
| `signal` | Signal Processor |
| `memory_short` | Memory Short (STM) |
| `mental_state` | Mental State |
| `bias_good` | Bias Good |
| `bias_bad` | Bias Bad |
| `memory_long` | Memory Long (LTM) |
| `knowledge` | Knowledge |
| `education` | Education |
| `priority` | Priority |
| `ethos` | Ethos |
| `drift` | Drift |
| `execution_risk` | Execution Risk |

**Scores:** MC Gate Score 0–10 (backend routing); Confidence Score 0–1 (user-facing).  
**Other thresholds:** clarity gate **7.0**; planning unlock bands in `stage1_assembler.py` (see `docs/stage_1_reality.md`).

---

## Proof points (what exists in V7)

### DTM dogfood / hygiene

Source: `backend/janus/app/services/operator_dtm_hygiene_catalog.py`

| Tier | Count | Notes |
|------|-------|--------|
| Tier 1 | **7** | All `document_present: false` → blocked on document-missing gates in seed |
| Tier 2 | **9** | Includes T2-007 gate contract (marked `document_present: true` for demos) |
| Tier 3 | **10** | |
| **Total cards** | **26** | `HYGIENE_CARDS` |

**Operator portfolio:** 5 mission rows via `make dogfood-seed` — Groups A/B/C, gate contract, summary (`AGENTS.md`).

**Chat grounding test** (`tests/operator/test_dtm_hygiene_chat_grounding.py`): query *"what contracts does DTM need?"* → Tier-2 IDs **T2-001, T2-002, T2-003** (NDA, contractor, client services) + risks in prompt — **not** improvised scheduling drift.

**North-star curl** (`docs/plans/janus7_handoff/wave3_chat.json`):

```bash
curl -sS -X POST http://127.0.0.1:10000/api/v1/operator/chat \
  -H 'Content-Type: application/json' \
  -d '{"message":"what contracts does DTM need?","context":{}}'
```

**Live assertion (when `JANUS_OPERATOR_CHAT_LIVE=1`):** reply contains NDA / T2-001 / non-disclosure — **no committed JSON fixture**.

### Greenfield (legacy — not V7)

- **29/29 cards at MC 8.5+, 100% Sentinel** — referenced in hygiene spec / archive audits only; **not** runnable from JanusV7 (`greenfield/` forbidden).

---

## Assembly line (nano agents)

From `nano_agents.py` + `JANUS_OS_LAYERS.md`:

**Caretaker → Gardener → Mercury → Sentinel → Conductor → Scribe**

Mercury: STM vs LTM routing (MC ≥ 8.5 → LTM). Caretaker uses live Claude when API configured; other agents may stub at 0ms during Publish unless LLM wired.

---

## Startup commands (from `AGENTS.md`)

```bash
# Backend
source .venv/bin/activate
PYTHONPATH=$PWD USE_LOCAL_LLM=false ANTHROPIC_MODEL=claude-sonnet-4-6 \
  python -m uvicorn backend.janus.app.main:app --host 0.0.0.0 --port 10000 --reload

# Frontend
cd frontend && npm run dev   # :3000 → proxies /api to :10000

# Dogfood seed
make dogfood-seed
GET /api/v1/operator/mission
```

---

## Workflow installation and gate creation (CIO-facing)

**Purpose:** Explain how an organization brings workflows under Janus and attaches gates — concept and operating model only. Safe for a technical page or diligence memo. **Do not publish:** internal schemas, evaluator logic, agent prompts, MC weighting, or assembly-line orchestration details.

### What “installing a workflow” does not mean

Janus does not rip out your existing agents, RPA, or LLM stack. It **sits above** them. “Installation” is **registering** a workflow inside the governance layer: naming it, binding it to your org’s structure and documentation, and choosing where human decisions are mandatory. Execution can still call your tools; Janus owns **whether** a step may run and **what gets recorded** when it does.

### Phase 0 — Organizational grounding (before any workflow runs)

Before gates are meaningful, the system needs **your** context — not a generic model guess.

| Step | What happens (concept) | CIO takeaway |
|------|------------------------|--------------|
| **Ingest** | Company documentation (policies, contracts, SOPs, product definitions) is brought in, structured, and indexed into institutional memory | The platform “knows the organization” before it executes |
| **Curate** | Only material that passes readiness rules promotes to long-term memory; working memory holds session-level context | You control what becomes durable doctrine vs ephemeral work |
| **Align** | Governance policy (thresholds, eligibility, danger classes) is applied consistently across surfaces | One policy vocabulary — not a different rule in every tool |

Private beta (Phase 1) emphasizes this foundation: governed workflows, memory, and deterministic gates **on top of** ingested company truth.

### Installing a workflow (registration under governance)

A workflow is **installed** when it is declared as a governed object with:

1. **Identity** — stable workflow id, owning team, and scope (what business process it represents).
2. **Stage model** — the standard ritual progression (intake → refine → plan → negotiate → approve → apply → verify → publish) or an approved variant; each stage is an explicit checkpoint, not an implicit chat turn.
3. **Documentation binding** — which artifacts must exist or stay current for this workflow (hygiene-style “document present / stale” rules are one pattern).
4. **Execution binding** — which external or internal agents may run at apply/publish; Janus does not replace them but **blocks** apply when baseline, drift, or gate conditions fail.

**Roadmap Phase 2 (Workflow Creation / Arbiter):** operators **author and harden** new workflows **inside** the governance layer — created and ratified there, not in a shadow spreadsheet or side-channel prompt. That is the commercial answer to “who builds workflows?”: your operators, inside policy.

**What to say publicly:** workflows are **registered, versioned, and ratified** — not pasted in as one-off automations.

**What to withhold:** card/task store formats, planner internals, exact ritual state machine code paths, auto-approve heuristics.

### Creating gates (deterministic decision points)

A **gate** is not a smarter model. It is a **contract** invoked at defined **danger points** — publish, apply, external side effects, policy-sensitive changes, missing documentation, memory conflicts, etc.

#### 1. Choose danger points

Work with operators to mark where the organization cannot afford silent automation: e.g. before apply, before publish, before customer-facing output, before funds move. These are **workflow danger points**, not “when the AI feels unsure.”

#### 2. Write the condition (plain language)

Each gate has a **trigger** your organization defines **before** deployment — in language auditors and operators can read, e.g. “required contract artifact missing,” “detected value differs from ratified baseline,” “documentation older than threshold.” The condition is evaluated against **facts** in scope (records, hashes, presence flags, drift checks) — **not** open-ended model judgment at runtime.

Public framing (from gate contract docs):

- **Trigger** — what causes the gate to fire  
- **Evaluates** — what data is checked (scoped, no inference)  
- **Returns** — what the operator sees (what fired, context, choices)  
- **Decision points** — allow / hold / override paths, each **logged with reason**

#### 3. Map outcomes

Every gate resolves to a normalized outcome: **allow**, **block**, **human review** (operator decides), or **allow with debt** (proceed with tracked obligation). Non-pass outcomes should produce a **review packet**: issue, evidence, suggested fix, required human action, governance consequence.

**CIO message:** every stop is **explainable and replayable** — suitable for audit, not a black-box score.

#### 4. Bind to the workflow

Gates are **attached** to workflow transitions (typically approve → apply → publish). The same gate contract can be reused across workflows; workflows differ in **which** danger points are armed and **which** artifacts they require.

**Idempotency:** each gate invocation carries a **request id** so retries do not spawn duplicate or contradictory decisions.

#### 5. Operate in Mission Control

When a gate fires, the workflow appears **blocked** with operator-readable context — not a generic error. The human resolves inside policy; the system records the decision and links it to the plan it governed.

### How this differs from “AI guardrails”

| Typical guardrail | Janus gate |
|-------------------|------------|
| Model confidence threshold | **Pre-declared condition** on organizational facts |
| Post-hoc moderation | **Pre-execution** authorization layer |
| Opaque “safety” score | **Review packet** + logged operator path |
| Per-tool configuration | **One gate contract** callable across workflows |

MC (meta-cognitive scoring) **informs** readiness; the gate **authorizes**. Those layers stay separate — do not describe MC as “the gate.”

### Deployment shape (high level)

| Concern | Public-safe description |
|---------|-------------------------|
| **Runtime** | FastAPI services + SQLite persistence + optional vector index; Mission Control UI for operators |
| **Integration** | HTTP API; existing stacks call ritual/gate/operator endpoints |
| **LLM** | Pluggable (cloud API or local); gates do not depend on which model drafts text |
| **Hosting** | Local-first dev; production topology customer-specific — avoid implying a single SaaS multi-tenant diagram unless true |

### Suggested copy block (technical page — ~200 words)

> **Bringing workflows under Janus** starts with your documentation and policy, not with replacing your agents. Company knowledge is ingested and curated so the system understands your structure before anything executes. Each workflow is then registered in the governance layer with explicit stages, required artifacts, and bound execution — so work progresses through intake, planning, approval, and publish with a clear audit trail.
>
> **Gates** are attached at danger points you define in advance: apply, publish, external effects, or missing governance artifacts. Each gate is a plain-language condition evaluated on facts — who acted, what changed, whether required documentation is present — not on model intuition. When a condition is met, the gate fires, the workflow stops, and the operator sees exactly what triggered and what choices exist. Every path is logged.
>
> Operators build and ratify new workflows inside this layer (Arbiter, on the roadmap) so speed never moves outside policy. Janus coordinates and remembers; your existing infrastructure still runs the work once the gate allows it.

### IP boundary — do not put on the public site

- Exact gate DSL, YAML keys, or policy file layouts  
- MC bin weights, promotion algorithms, or assembly-line agent prompts  
- Negotiation/adversarial review internals  
- Hygiene card seed JSON, catalog field names, or Box path conventions  
- Source-level drift detection formulas (describe outcomes, not implementations)  

---

## Suggested technical page outline (for janus-site)

1. **Stack** — table above  
2. **Three layers** — diagram + module table (no L3-as-moat inversion)  
3. **Ritual / API** — 8-step table + sample prefixes  
4. **Gate contract** — triggers / evaluates / outcomes (link to diligence PDF later)  
5. **Workflow install + gate creation** — section above (CIO narrative)  
6. **Memory** — STM/LTM/Chroma + 12 bins + 8.5 threshold  
7. **Proof** — dogfood hygiene + curl (optional live JSON screenshot)  
8. **Roadmap tie-in** — Phase 1–4 names only, detail on roadmap page  

---

## Files pulled (reference index)

```
backend/janus/app/services/workflow_contract.py
backend/systems/mc_engine.py
backend/janus/app/services/operator_dtm_hygiene_catalog.py
backend/janus/app/services/janus_os_gate_evaluator.py
backend/janus/app/services/assembly_line_processor.py
backend/janus/app/core/nano_agents.py
backend/janus/app/main.py
backend/janus/app/routers/workflow.py
backend/janus/app/routers/operator.py
backend/janus/app/routers/chat_commands.py
docs/JANUS_OS_LAYERS.md
docs/dogfood/dtm_gate_contract.md
docs/governance/JANUS_OS_GATE_CONTRACT_V1.md
router_registry.yaml
AGENTS.md
backend/requirements.txt
tests/operator/test_dtm_hygiene_chat_grounding.py
docs/plans/janus7_handoff/wave3_chat.json
```

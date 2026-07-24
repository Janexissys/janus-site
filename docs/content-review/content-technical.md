# Technical (technical.html) — text content

## Hero
**Eyebrow:** Technical
**Heading:** Janus OS
**Sub:** Governance infrastructure you can audit — stack, architecture, gates, and memory.

## Stack
| | |
|---|---|
| Backend | FastAPI, Python 3.12, Pydantic v2 |
| Persistence | SQLite with aiosqlite (async, WAL) — portable, isolated deployments; your operational data does not share a multi-tenant pool |
| Vector memory | ChromaDB — 384-dimensional embeddings |
| LLM | Model-agnostic. Anthropic Claude in private beta. Ollama for local fallback. Bring your own via API. |
| Embeddings | nomic-embed-text; SentenceTransformers fallback (all-MiniLM-L6-v2) |
| Frontend | React, TypeScript, Vite, Radix UI, Tailwind CSS 4 |
| CLI | Python Typer + Rich |

Dev defaults: backend :10000, Mission Control :3000 (proxies /api).

## Architecture
Three layers. Memory at the base.

Mission Control sits on top. Coordination runtime in the middle. Institutional memory anchors everything below. All three layers are the product — Layer 3 is where humans decide when gates fire.

**Layer 1 — Institutional Memory**
- STM vault — Workflow-scoped working memory — per card, searchable within workflow
- LTM vault — Ratified content only — doctrine, decisions, overrides
- MC engine — Twelve-bin meta-cognitive scoring; promotion at MC ≥ 8.5
- Vector store — Semantic recall when vector search is enabled
- Governance — Policy, doctrine, gate definitions
- Audit — Traceability on every transition

**Layer 2 — Coordination Runtime**
- Workflow contract — Governed phase machine — intake through publish
- Assembly line — Six-agent pipeline prepares work before human gates
- Gate evaluator — Normalized, auditable authorization decisions
- Plan store — Durable plans on SQLite (WAL)

**Layer 3 — Mission Control**
- Operator API — Mission view, drill-in, blocked packet, chat
- Frontend — Mission + Chat — React, TypeScript
- Projections — Card catalog mapped to Kanban lanes

## Cards
The unit of work. The scope of chat.

The card system is central to Janus. Each card is a governed unit with its own context and chat. Click a card to work inside that silo. Search across other cards in the same workflow when you need the full picture — connected, not isolated.

Mission Control maps the card catalog to Kanban lanes. The board is the surface; the card hierarchy is the architecture.

## Card hierarchy
Work is organized in a parent and child hierarchy. Child cards inherit parent governance. When a gate fires upstream, it propagates downstream. When documentation changes at the parent, dependent cards are notified — coherence across multi-stage work without manually wiring every relationship.

| | |
|---|---|
| Per card | Own context, own chat, own memory scope within the workflow |
| Parent → child | Governance and gate state inherit downstream |
| Blocked parent | Child instructions do not release until the gate clears |
| Cross-card | Search and recall within the workflow, not org-wide noise |

## Workflows
Registered under governance. Not bolted on after.

Janus does not replace your agents or automation stack. It registers work inside a governance layer your organization controls.

1. **Ground the organization** — Policies, contracts, SOPs, and product documentation are ingested and structured so the system knows your rules before anything executes.
2. **Declare the workflow** — Stable identity, owning team, explicit stages, and required artifacts. Each stage is a checkpoint with an audit trail.
3. **Attach gates** — Danger points you define in advance: apply, publish, external effects, missing governance artifacts.
4. **Ratify and run** — Workflows are created and hardened inside the layer — not around it. Your infrastructure executes once the gate allows it.

## Governed execution
One governed step at a time. Nothing moves until the gate clears.

Most AI stacks hand the model a long prompt and hope it follows every rule. Janus breaks work into discrete, governed commands — one step per task, one gate before the next instruction releases.

If a parent workflow is blocked, children hold. No silent drift downstream. Progression stays tied to cards and auditable gate decisions.

**Ritual sequence:** intake → refine intent → plan → negotiate → approve → apply → verify → publish. Each transition is logged. Your organization defines required artifacts and danger points before deployment; Janus enforces them at runtime.

API surface: `POST /api/v1/workflow/ritual/*` and operator routes under `/api/v1/operator/`.

## Gates
A contract at a danger point. Not a model score.

Before deployment, your organization writes conditions that require human review in plain language — auditable facts, not vague risk categories.

When a condition is true, the gate fires. The workflow stops. The operator sees what triggered it and decides. Every path is logged.

> MC scoring informs readiness. The gate authorizes. That separation is what makes the system defensible in audit.

| Outcome | What happens |
|---|---|
| Allow | Execution may proceed |
| Clarify | Not authorized yet — more specificity or evidence required |
| Human review | Operator decides: approve, reject, clarify, or allow with debt |
| Allow with debt | Proceeds now with a logged obligation to resolve later — keeps work moving without pretending compliance is done |
| Block | Execution does not proceed |

The authorization outcome is deterministic given the declared condition.

## Memory
Two vaults. One promotion threshold.

Working memory is workflow-scoped. Each card opens its own chat — siloed context. Search across other cards in the same workflow when needed. Long-term memory holds ratified content only — decisions, overrides, doctrine, governance artifacts.

Promotion to LTM requires MC ≥ 8.5 across twelve structured bins. Below that, material stays in working memory or is pruned. The vault grows by deliberate act, not accumulation.

Vector search runs via ChromaDB when semantic recall is enabled.

## Assembly line
Continuous hygiene across workflows and memory — so when a gate fires or someone queries the vault, context is already clean, current, and actionable.

| Agent | Role |
|---|---|
| Caretaker | Intake validation and intent gate |
| Gardener | Content structure — background, proposal, action items |
| Mercury | Memory routing — STM vs LTM at MC ≥ 8.5 |
| Sentinel | Policy audit — PII, safety, compliance |
| Conductor | Workflow state — done, quarantine, archived |
| Scribe | Decision capture — ledger write and audit trail |

## Proof
After seeding twenty-six company hygiene cards, the operator chat endpoint was asked:

> "what contracts does DTM need?"

The response surfaced the NDA, Contractor Agreement, and Client Services Agreement — in priority order, with risk framing from the governance catalog. Not a generic LLM answer. Institutional memory answering from structured, retrievable organizational knowledge. Verified in the test suite; live demo available during technical review.

## Integration
HTTP by design. Your stack stays in place.

Phase 1 integration is deliberately HTTP — not a gap, a boundary. Your existing agents, scripts, and orchestrators connect without replacement. Any HTTP client can call the API surface.

LLM provider is environment-configurable: Anthropic in private beta, Ollama locally, or bring your own via the fallback chain. Native SaaS connectors are on the roadmap.

## Private beta
Janus OS is in private beta. Access is by invitation.

**CTAs:** Request access / Book a technical review

## Footer
DTM — Dynamic Trajectory Memory Inc.
1 Rideau Street, Suite 700 · Ottawa, ON

Nav: Watch the demo · Product · Technical · Company · Roadmap · About · Blog

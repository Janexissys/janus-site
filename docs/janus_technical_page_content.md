# Janus OS — Technical

---

## Stack

| Layer | Technology |
|-------|------------|
| Backend | FastAPI, Python 3.12, Pydantic v2 |
| Persistence | SQLite with aiosqlite (async, WAL) — portable, isolated deployments |
| Vector memory | ChromaDB — 384-dimensional embeddings |
| LLM | Model-agnostic. Anthropic Claude in private beta. Ollama for local fallback. Bring your own via API. |
| Embeddings | nomic-embed-text; SentenceTransformers fallback (all-MiniLM-L6-v2) |
| Frontend | React, TypeScript, Vite, Radix UI, Tailwind CSS 4 |
| CLI | Python Typer + Rich |

Dev defaults: backend **:10000**, Mission Control **:3000** (proxies `/api`).

---

## Architecture — three layers

Memory at the base. Governance in the middle. Mission Control on top.

### Layer 1 — Institutional Memory

| Component | Role |
|-----------|------|
| STM vault | Workflow-scoped working memory — per card, searchable within workflow |
| LTM vault | Ratified content only — doctrine, decisions, overrides |
| MC engine | Twelve-bin meta-cognitive scoring; promotion at MC ≥ 8.5 |
| Vector store | Semantic recall when vector search is enabled |
| Governance config | Policy, doctrine, gate definitions |
| Audit and drift | Traceability on every transition |

### Layer 2 — Coordination Runtime

| Component | Role |
|-----------|------|
| Workflow contract | Governed phase machine — intake through publish |
| Assembly line | Six-agent pipeline prepares work before human gates |
| Gate evaluator | Normalized, auditable authorization decisions |
| Plan store | Durable plans on SQLite (WAL) |

### Layer 3 — Mission Control

| Component | Role |
|-----------|------|
| Operator API | Mission view, drill-in, blocked packet, chat |
| Frontend | Mission + Chat — React, TypeScript |
| Projections | Card catalog mapped to Kanban lanes |

Layer 3 is how operators see and act. Layers 1–2 are what the organization remembers and may do.

---

## Cards — the unit of work and chat

The card system is central to how Janus runs. A card is not a ticket label — it is a **governed unit** with its own context, chat history, and place in a workflow.

- **Click a card** → open chat scoped to that card. Working memory stays siloed so operators and agents are not drowning in unrelated threads.
- **Same workflow** → search and reference other cards in that workflow when the task requires broader context. Connected, not isolated.
- **Parent and child** → complex work decomposes into a hierarchy. Children inherit parent governance; upstream gates and documentation changes propagate downstream.

Mission Control projects the card catalog into Kanban lanes (running, blocked, completed). The board is the surface; the hierarchy is the architecture.

---

## Card hierarchy (parent and child)

Work is organized in a **card hierarchy**. Each card is a governed unit — its own context, its own chat, its own memory scope within the workflow.

Child cards inherit the governance of their parent. When a gate fires upstream, it propagates downstream. When documentation changes at the parent level, dependent cards are notified. The hierarchy is how Janus maintains coherence across multi-stage work without requiring a human to manage every connection manually.

This describes the operator experience. It is the structural claim that makes “siloed but searchable” an architecture, not a feature toggle.

---

## Bringing workflows under Janus

Janus does not replace your agents or automation stack. It **registers** work inside a governance layer.

1. **Ground the organization** — Policies, contracts, SOPs, and product documentation are ingested and structured so the system knows your rules before anything executes.
2. **Declare the workflow** — Stable identity, owning team, explicit stages, and required artifacts. Each stage is a checkpoint with an audit trail.
3. **Attach gates** — Danger points you define in advance (apply, publish, external effects, missing governance artifacts).
4. **Ratify and run** — New workflows are created and hardened inside the layer — not around it. Your infrastructure executes once the gate allows it.

---

## Governed execution

**One governed step at a time. Nothing moves until the gate clears.**

Most AI stacks hand the model a long prompt and hope it follows every rule. Janus breaks work into discrete, governed commands — one step per task, one gate before the next instruction releases. If a parent workflow is blocked, children hold. No silent drift downstream.

**Ritual sequence:** intake → refine intent → plan → negotiate → approve → apply → verify → publish. Each transition is logged. Your organization defines required artifacts and danger points before deployment; Janus enforces them at runtime. Existing agents and tools still do the work — Janus decides whether a step may proceed and records why.

API surface: `POST /api/v1/workflow/ritual/*` plus operator endpoints under `/api/v1/operator/`.

---

## Gates

A gate is a **contract at a danger point** — not a confidence score from a model.

Before deployment, your organization writes the conditions that require human review in plain language. Not vague risk categories — **auditable facts**: documentation present or missing, drift from a ratified baseline, a threshold crossed, an approval chain incomplete.

When the condition is true, the gate fires. The workflow stops. The operator sees what triggered it, the context, and the available choices. Every path is logged and linked to the plan it governed.

Meta-cognitive (MC) scoring informs readiness. The gate **authorizes**. Those layers stay separate — that separation is what makes the system defensible in audit.

**Gate outcomes**

| Outcome | What happens |
|---------|-------------|
| Allow | Execution may proceed |
| Clarify | Not authorized yet — more specificity or evidence required |
| Human review | Operator decides: approve, reject, clarify, or allow with debt |
| Allow with debt | Proceeds now with a logged obligation to resolve later — work keeps moving without pretending compliance is done |
| Block | Execution does not proceed |

The authorization outcome is deterministic given the declared condition. That is what makes it auditable.

---

## Memory

Two vaults. One promotion threshold.

**Working memory is workflow-scoped.** Each card opens its own chat — siloed context, no pollution between unrelated work. When needed, search and recall can reach other cards in the **same workflow**. Related work stays connected without bleeding across the organization.

Long-term memory holds **ratified** content only — decisions, overrides, doctrine, governance artifacts. Content is evaluated across twelve structured bins; promotion to LTM requires **MC ≥ 8.5**. Below that threshold, material stays in working memory or is pruned. The vault grows by deliberate act, not accumulation.

When an operator or agent queries institutional memory, what returns has already been curated — not dredged from chat history.

Vector search runs across indexed material via ChromaDB when semantic recall is enabled.

---

## Assembly line — six agents

Continuous hygiene across workflows and memory — so when a gate fires or someone queries the vault, context is already clean, current, and actionable.

| Agent | Role |
|-------|------|
| Caretaker | Intake validation and intent gate |
| Gardener | Content structure — background, proposal, action items |
| Mercury | Memory routing — STM vs LTM at MC ≥ 8.5 |
| Sentinel | Policy audit — PII, safety, compliance |
| Conductor | Workflow state — done, quarantine, archived |
| Scribe | Decision capture — ledger write and audit trail |

---

## Proof

After seeding twenty-six company hygiene cards, the operator chat endpoint was asked:

*"what contracts does DTM need?"*

The response surfaced the NDA, Contractor Agreement, and Client Services Agreement — in priority order, with risk framing drawn from the governance catalog. Not a generic LLM answer. Institutional memory answering from structured, retrievable organizational knowledge.

Verified in the test suite against the seeded catalog. Live demo available on request during technical review.

---

## Integration

**HTTP by design. Your stack stays in place.**

Phase 1 integration is deliberately **HTTP** — not a gap, a boundary. Your existing agents, scripts, and orchestrators connect without replacement. Any HTTP client can call the API surface.

LLM provider is environment-configurable — Anthropic in private beta, Ollama locally, or bring your own via the fallback chain. Native SaaS connectors are on the roadmap.

---

## Private beta

Janus DTM OS is in private beta. Access is by invitation.

Request access · Book a technical review

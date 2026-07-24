# Product (product.html) — text content

## Demo hero
**Label:** Demo
**Heading:** See a gate stop a workflow — then see a human clear it.

1. **Workflow runs** — Mission Control shows the parent workflow and child tasks in Running — until a gate condition is met.
2. **Gate blocks** — Scheduling or drift fires on a fact. The card moves to Blocked; children inherit the hold.
3. **Human clears** — Operator reviews grounded chat, then approves or allows with debt. Every path logged; workflow completes.

**Tagline:** Human decides. AI accelerates. *Janus sustains.*

## "See it work" walkthrough
1. A workflow runs in Mission Control — parent card with child tasks advancing through stages.
2. A gate fires on a scheduling or drift condition. The parent moves to **Blocked**; children do not advance.
3. The operator opens card chat. Answers draw on institutional memory — contracts, gate context, prior ratifications.
4. The operator resolves — approve, hold, or allow with debt. The decision is logged; the workflow moves to Completed.

**Kanban strip example (illustrative):**
- *Running:* "Vendor approval" parent, with child "Intake review" (chat active) and "Plan generation" (waiting on prior gate).
- *Blocked:* "External submission" parent — "Condition met on 14 records. Propagates to child tasks below." Children: "Validate records" (blocked, inherited), "Apply changes" (instructions held until gate clears).
- *Completed:* "Q4 allocation" parent, published/ratified, with child "Final sign-off" (completed).

**Caption:** Click any card to open its chat — siloed working memory for that unit of work, searchable across the workflow when you need the full picture. Blocked on a parent means children do not advance until the gate is cleared.

## Intro strip
Janus stops unapproved AI actions and records who approved them — before they hit production.
Built for Risk, Compliance, and IT leaders in highly regulated industries — finance, healthcare, defense, and critical infrastructure.
Dogfood: DTM runs on Janus. Every decision we make is governed by our own institutional memory.

## The problem
We built AI systems that mimic human communication and thought. Then we forgot to teach them the decision boxes or gates we teach humans.

Organizations running AI workflows today have speed. What they don't have is memory, governance, or continuity. When something goes wrong there is no record of why it was approved, who decided, or when the drift started.

The models are not broken. The structure around them is missing.

> Memory is retrieval, not storage. A larger context window is just a more expensive way to get lost.

## How gates work
You cannot audit a *probability*.

Every AI system running on model judgment is a black box. Janus is the glass box. Your organization defines the trigger before deployment. When the condition is true, the gate fires on a fact — not an inference. Every decision logged. Every override audited.

> Governable. Auditable. Yours.

**Differentiator — deterministic gates**

*Without Janus — model judgment:* A decision was made. No record of what triggered it. No record of what was evaluated. Cannot be audited or explained.

*With Janus — deterministic gate:* Condition defined before deployment. Condition evaluated on a fact. Gate fired or passed on that fact. Decision logged with full context.

**How a gate works (anatomy):**
- **Trigger** — The condition your organization defines before deployment. Deterministic, not inferred.
- **Evaluates** — Defined scope and data when the gate runs. A fact, not a probability.
- **Returns** — What the operator sees in plain language: what fired, the context, and the choices.
- **Decision points** — Each operator path — allow, hold, override — logged with reason and linked to the plan.

## Cards and Mission Control
Parent and child. One workflow surface.

Work is organized in a card hierarchy. Each card is a governed unit — its own context, its own chat, its own place in the workflow. Child cards inherit the governance of their parent: when a gate fires upstream, it propagates downstream; held instructions do not release until the gate clears.

Search across cards in the same workflow when you need the full picture — connected, not isolated.

## Architecture
Three layers. Memory at the base. Gates at every transition.

- **Layer 1 · Base — Institutional Memory** (cards · memory graph · governance · traceability): card hierarchy, memory graph, drift detection, audit trail.
- **Layer 2 — Coordination Runtime** (where humans and workflows meet under governance): workflow gates, workflow coordination, approvals, escalation.
- **Layer 3 · Surface — Mission Control** (operational surface — visibility, state, operator actions): running / blocked / completed.

Not everything earns its place in institutional memory. Janus writes only what has been ratified — the decision, the condition that triggered it, the reason the operator gave. The vault grows by deliberate act, not accumulation.

## The assembly line
The assembly line keeps your system clean.

The assembly line maintains structure around your cards, your gates, and your vault — so when a gate fires or an operator queries, the context is already clean, current, and ready to act on. No archaeology. No interpretation. Just the information you need to decide.

**Before the gate:**
- **Context** — Every workflow arrives structured. Documentation and context are verified before execution — gates fire on facts, not assumptions.
- **Documentation** — Workflow and gate documentation is verified before execution. If something is missing or stale the assembly line surfaces it before the gate fires.
- **Signal** — Noise is cleared before the gate reaches a human. What the operator sees is what triggered the gate — nothing more, nothing less.
- **Memory** — Only ratified content enters the long-term vault. The assembly line curates what persists so future workflows inherit clean context, not accumulated drift.

## Closing
Workflow tools move tasks. Janus stabilizes institutions.
Governance, memory, and continuity built into every workflow from the start. Not inferred. Defined.

## Footer
DTM — Dynamic Trajectory Memory Inc.
1 Rideau Street, Suite 700 · Ottawa, ON

Nav: Product · Technical · Company · Roadmap · About · Blog

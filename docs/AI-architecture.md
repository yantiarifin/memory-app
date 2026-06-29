<h1>AI Architecture</h1>

<h2>Purpose</h2>

DiscoveryApp uses AI in two fundamentally different ways:

1. Engineering AI – AI assistants that help build and maintain the application.
2. Product AI – AI agents and services that are part of the product and interact with user data.

Keeping these responsibilities separate makes the system easier to understand, maintain, and evolve.

Engineering AI builds DiscoveryApp. Product AI is DiscoveryApp.
<br><br>

<h2>Layer 1 — Engineering AI</h2>

These agents exist only during development.

They are members of the engineering team and are never exposed to end users.

Current implementation: Claude Code Agents.

Status: aspirational — the repository is currently docs-only; these agent definitions are not yet scaffolded.
<br><br>
<h3>Architecture Agent</h3>
Responsibilities

* Reviews architectural changes
* Enforces architectural consistency
* References ADRs
* Identifies unnecessary coupling
* Recommends refactoring



<h3>Frontend Agent</h3>

Responsibilities

* React development
* UI components
* State management
* Accessibility
* Performance



<h3>Backend Agent</h3>

Responsibilities

* API implementation
* Business logic
* Database access
* Authentication
* Service orchestration



<h3>AI Integration Agent</h3>

Responsibilities

* Prompt engineering
* RAG implementation
* Embeddings
* Model integration
* Context management



<h3>Database Agent</h3>

Responsibilities

* Schema design
* Performance
* Migrations
* Indexing
* Vector storage



<h3>Testing Agent</h3>

Responsibilities

* Unit tests
* Integration tests
* End-to-end testing
* Edge cases
* Regression prevention



<h3>Documentation Agent</h3>

Responsibilities

* Maintains architecture documentation
* Updates ADRs
* Reviews documentation
* Ensures documentation stays synchronized with implementation
<br><br>
<h2>Layer 2 — Product AI</h2>

These components are part of DiscoveryApp itself.

They work together to understand the user's history and generate trustworthy insights.

Provider strategy: provider-agnostic via a common `LLMProvider` interface — OpenAI first, Anthropic second, local models later. The rest of the application never knows which provider is in use. (See ADR 0002.)

Privacy: hybrid / local-by-default. Data and embeddings stay local when possible; only the minimum retrieved context is sent to a cloud LLM; the full history is never transmitted without explicit opt-in. (See ADR 0001.)

Product AI is divided into True AI Agents (LLM-driven reasoning) and Pipeline Services (deterministic orchestration that may call an LLM internally, but is not autonomous). (See ADR 0003.)
<br><br>

<h2>True AI Agents</h2>

<h3>Conversation Agent</h3>

Purpose

The primary interface between the user and DiscoveryApp, and the runtime orchestrator. (See ADR 0007.)

Responsibilities

* Conduct natural conversations
* Answer questions
* Orchestrate the Retrieval, Knowledge Graph, and Memory services
* Retrieve insights and evidence from their stores
* Explain insights
* Guide exploration
* Generate temporary on-demand hypotheses for novel questions (never persisted until validated)



<h3>Insight Agent</h3>

Purpose

Discover meaningful long-term patterns.

Responsibilities

* Detect recurring themes
* Generate hypotheses
* Connect conversations
* Identify behavioral changes
* Produce candidate insights

Candidate insights are not trusted until they pass validation. (See ADR 0004, 0005.)



<h3>Validation Agent</h3>

Purpose

Ensure every insight earns trust, and own the Evidence Layer.

Responsibilities

* Verify supporting evidence
* Strengthen weak insights (maximum 2 retries), then reject if still insufficient
* Record rejected hypotheses and rejection reasons to prevent repeated regeneration
* Assign confidence
* Detect contradictions
* Prevent hallucinations
* Produce and persist the Evidence Bundle for each validated insight

(See ADR 0005, 0006.)
<br><br>

<h2>Pipeline Services</h2>

<h3>Retrieval Service</h3>

Purpose

Locate the most relevant information.

Responsibilities

* Semantic search
* Timeline search
* Knowledge graph traversal
* Metadata filtering
* Context assembly



<h3>Knowledge Graph Service</h3>

Purpose

Maintain relationships across the user's history.

Responsibilities

* Extract entities
* Build relationships
* Track recurring people
* Track places
* Track concepts



<h3>Memory Service</h3>

Purpose

Maintain a rich representation of conversations.

Responsibilities

* Generate summaries
* Create tags
* Cluster conversations
* Build embeddings
* Organize memories

<br><br>

<h2>Insight Lifecycle</h2>

candidate → (validation, up to 2 strengthening retries) → validated (with Evidence Bundle) → persisted.

Rejected candidates are stored with their rejection reason to avoid being re-proposed. On-demand hypotheses generated during a conversation live outside this lifecycle until they pass validation. (See ADR 0004, 0005.)

<br><br>

<h2>System Flow</h2>

Realtime path — orchestrated by the Conversation Agent (ADR 0007):

User

↓

Conversation Agent

↓

Retrieval Service

├── Vector Search

├── Timeline

├── Knowledge Graph

└── Existing Insights

↓

(may also call Knowledge Graph Service, Memory Service, Insight Store, Evidence Store)

↓

Conversation Agent

↓

Response to User

⸻

Background Processing

Cadence: incremental on import + scheduled full-corpus re-analysis (ADR 0004).

Conversation Import

↓

Memory Service

↓

Knowledge Graph Service

↓

Insight Agent

↓

Validation Agent (strengthen / validate, build Evidence Bundle)

↓

Persist Verified Insights + Evidence

⸻

<h2>Design Principle</h2>

Engineering AI builds DiscoveryApp.

Product AI is DiscoveryApp.

The two systems have completely different responsibilities and should remain independently evolvable.

Engineering AI should optimize software quality.

Product AI should optimize user understanding.

<br><br>

<h2>Decisions</h2>

The decisions behind this architecture are recorded as ADRs in `docs/decisions/`:

* ADR 0001 — Hybrid Privacy Model
* ADR 0002 — AI Provider Abstraction
* ADR 0003 — Logical Agents vs. Pipeline Services
* ADR 0004 — Insight Generation Cadence
* ADR 0005 — Insight Validation Pipeline
* ADR 0006 — Evidence Bundle Architecture
* ADR 0007 — Conversation-Orchestrated Runtime

<h1>AI Architecture</h1>

<h2>Purpose</h2>

DiscoveryApp uses AI in two fundamentally different ways:

1. **Engineering AI** — AI assistants that help design, build, test, and maintain DiscoveryApp.
2. **Product AI** — AI components that transform a user's history into evidence-backed understanding.

Keeping these  separate makes the system easier to reason about, maintain, and evolve.

While the implementation may evolve, the distinction between these two layers is fundamental to the architecture.
<br><br>


<h2>Layer 1 — Engineering AI</h2>

Engineering AI exists only during development.

These agents are members of the engineering team. They design, implement, review, test, and document DiscoveryApp, but are never exposed to end users.

Current implementation: Claude Code.

The engineering architecture is provider-independent. Different AI tools may be used over time as their capabilities evolve, without affecting the product architecture.
<br><br>

<h3>Architecture Agent</h3>

* Reviews architectural changes
* Ensures consistency with the Vision, Tenets, and Technical Architecture
* Identifies unnecessary coupling
* Recommends refactoring
* Reviews significant architectural decisions
<br>

<h3>Frontend Agent</h3>

* React development
* UI implementation
* State management
* Accessibility
* Performance optimization
<br>

<h3>Backend Agent</h3>

* API implementation
* Business logic
* Database access
* Authentication
* Service orchestration
<br>

<h3>AI Integration Agent</h3>

* Prompt engineering
* Model integration
* Embeddings
* Retrieval pipelines
* Context management
* AI evaluation
<br>

<h3>Database Agent</h3>

* Schema design
* Performance optimization
* Database migrations
* Indexing
* Vector storage
<br>

<h3>Testing Agent</h3>

* Unit tests
* Integration tests
* End-to-end testing
* Regression prevention
* Edge case validation
<br>

<h3>Documentation Agent</h3>

* Maintains project documentation
* Reviews documentation for consistency
* Keeps documentation synchronized with implementation
<br><br>

<h2>Layer 2 — Product AI</h2>

Product AI is responsible for transforming a user's history into trustworthy understanding.

Unlike Engineering AI, these components are part of DiscoveryApp itself. They analyze imported history, discover long-term patterns, validate every observation against evidence, and help users explore what they might otherwise never have noticed.

Product AI is provider-agnostic through a common `LLMProvider` interface, allowing AI providers to evolve independently of the application without changing the surrounding architecture.

DiscoveryApp follows a hybrid, local-first privacy model. User history, embeddings, and derived knowledge remain local whenever possible. When cloud models are used, only the minimum retrieved context required to complete a task is transmitted unless the user explicitly chooses otherwise.

Product AI consists of two categories:

* **AI Agents** — Components that perform reasoning using an LLM.
* **Pipeline Services** — Deterministic services that organize, retrieve, and prepare information for the agents.
<br><br>
<h2>AI Agents</h2>
<h3>Discovery Agent</h3>
The primary interface between the user and DiscoveryApp, and the runtime orchestrator. 

* Conduct natural conversations
* Answer questions
* Orchestrate the Retrieval, Knowledge Graph, and Memory services
* Retrieve insights and evidence from their stores
* Explain insights
* Guide exploration
* Generate temporary on-demand hypotheses for novel questions (never persisted until validated)
<br><br>

<h3>Insight Agent</h3>
Discover meaningful long-term patterns.

* Detect recurring themes
* Generate hypotheses
* Connect conversations
* Identify behavioral changes
* Produce candidate insights

Candidate insights are not trusted until they pass validation.
<br><br>

<h3>Validation Agent</h3>
Ensure every insight earns trust, and own the Evidence Layer.

* Verify supporting evidence
* Strengthen weak insights (maximum 2 retries), then reject if still insufficient
* Record rejected hypotheses and rejection reasons to prevent repeated regeneration
* Assign confidence
* Detect contradictions
* Prevent hallucinations
* Produce and persist the Evidence Bundle for each validated insight
<br><br>

<h2>Pipeline Services</h2>

<h3>Retrieval Service</h3>
Locate the most relevant information.

* Semantic search
* Timeline search
* Knowledge graph traversal
* Metadata filtering
* Context assembly
<br><br>


<h3>Knowledge Graph Service</h3>
Maintain relationships across the user's history.

* Extract entities
* Build relationships
* Track recurring people
* Track places
* Track concepts
<br><br>


<h3>Memory Service</h3>
Maintain a rich representation of conversations.

* Generate summaries
* Create tags
* Cluster conversations
* Build embeddings
* Organize memories
<br><br>

<h2>Insight Lifecycle</h2>
candidate → (validation, up to 2 strengthening retries) → validated (with Evidence Bundle) → persisted.

Rejected candidates are stored with their rejection reason to avoid being re-proposed. On-demand hypotheses generated during a conversation live outside this lifecycle until they pass validation. 
<br><br>

<h2>System Flow</h2>

<h3>Realtime path</h3>

```
User
 ↓
Discovery Agent
 ↓
Retrieval Service
 ├── Vector Search
 ├── Timeline
 ├── Knowledge Graph
 └── Existing Insights
 ↓
(may also call Knowledge Graph Service,Memory Service,Insight Store,
Evidence Store)
 ↓
Discovery Agent
 ↓
Response to User
```

<h3>Background Processing</h3>

```
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
```
<br><br>

<h2>Design Principle</h2>

* Engineering AI builds DiscoveryApp.

* Product AI is DiscoveryApp.

* The two systems have completely different responsibilities and should remain independently evolvable.

* Engineering AI should optimize software quality.

* Product AI should optimize user understanding.

<br><br>


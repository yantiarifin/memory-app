# DiscoveryApp

A personal memory companion that transforms the countless conversations you have
with AI into a deeper understanding of yourself.

Most AI generates answers. DiscoveryApp reveals patterns. It takes your imported
conversation history (ChatGPT, Claude, and more) and surfaces the recurring
themes, evolving beliefs, and forgotten ideas that are nearly impossible to see
one conversation at a time — always grounded in evidence you can inspect.

> We succeed when you open DiscoveryApp and say: "I never noticed that before."
> Not because AI invented a story, but because it helped reveal one that was
> always there.

## Principles

- **Your history is the product**, not the AI. AI only organizes, connects, and
  reveals what is already there.
- **Every insight must earn trust** — grounded in evidence, with the reasoning
  always inspectable.
- **Patterns matter more than memories.** The value is in connecting
  conversations across time.
- **Curiosity before productivity**, and **calm is a feature.**
- **Your history belongs to you** — portable, transparent, and under your control.

## Status

Early stage — **documentation and architecture only.** No application code has
been scaffolded yet.

## Documentation

| Document | What it covers |
|----------|----------------|
| [Vision](docs/vision.md) | Why this product exists |
| [Tenets](docs/tenets.md) | Principles we will not violate |
| [Technical Architecture](docs/technical-architecture.md) | The ingestion-to-UI pipeline |
| [AI Architecture](docs/AI-architecture.md) | Engineering AI vs. Product AI, agents and services |
| [Decisions (ADRs)](docs/decisions/README.md) | The architectural decisions and their rationale |

### Key decisions

The foundational choices are recorded as [ADRs](docs/decisions/README.md):

- **[Hybrid privacy](docs/decisions/0001-hybrid-privacy-model.md)** — local by
  default; only minimum context goes to a cloud LLM.
- **[Provider abstraction](docs/decisions/0002-ai-provider-abstraction.md)** — a
  common `LLMProvider` interface; OpenAI first, then Anthropic, then local.
- **[Agents vs. services](docs/decisions/0003-logical-agents-vs-services.md)**,
  **[insight cadence](docs/decisions/0004-insight-generation-cadence.md)**,
  **[validation pipeline](docs/decisions/0005-insight-validation-pipeline.md)**,
  **[evidence bundles](docs/decisions/0006-evidence-bundle-architecture.md)**, and
  **[conversation-orchestrated runtime](docs/decisions/0007-conversation-orchestrated-runtime.md)**.

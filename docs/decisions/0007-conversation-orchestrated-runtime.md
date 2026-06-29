# ADR 0007: Conversation-Orchestrated Runtime

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin

## Context

The realtime path — from a user's message to a response — needs a conductor that
decides what to retrieve and how to assemble an answer. We could introduce a
dedicated orchestration layer above all components, but for v1 that adds
indirection before we know we need it.

## Decision

The **Conversation Agent is the orchestrator.** It calls:

- Retrieval Service
- Knowledge Graph Service
- Memory Service
- Insight Store
- Evidence Store

There is no separate orchestration layer in v1. If orchestration becomes complex
later, it can be extracted into its own layer without changing user-facing
behavior, because the Conversation Agent's external interface stays stable.

## Consequences

### Positive
- Simple for v1: one place holds the runtime control flow.
- The user-facing contract is independent of internal orchestration, so the
  refactor path is open.

### Trade-offs
- Risk that the Conversation Agent accumulates too much responsibility; treat
  growth of its control logic as the signal to extract an orchestration layer.

## Related
- [0003 — Logical Agents vs. Pipeline Services](0003-logical-agents-vs-services.md)
- [0006 — Evidence Bundle Architecture](0006-evidence-bundle-architecture.md)

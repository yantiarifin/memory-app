# ADR 0002: AI Provider Abstraction

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin
- Supersedes: planned `0002-chatgpt-first`

## Context

DiscoveryApp's world is already multi-provider: users import histories from both
ChatGPT and Claude, and the Engineering AI layer runs on Claude. The hybrid
privacy model ([0001](0001-hybrid-privacy-model.md)) also requires that cloud
models be replaceable by local models over time. Committing the application to a
single provider's SDK would create lock-in and block that path.

## Decision

Be **provider-agnostic from day one**:

- Define a common `LLMProvider` interface (chat/completion, embeddings,
  streaming, and tool/function calling).
- Implement **OpenAI first**.
- Add **Anthropic second**.
- Support **local models** in the future.
- The rest of the application never knows which provider is being used.

## Consequences

### Positive
- No provider lock-in; swapping or running multiple providers is a config change.
- Directly enables the hybrid privacy model's "replace cloud with local" path.
- Provider-specific quirks are isolated behind one adapter boundary.

### Trade-offs
- Small upfront cost to design and maintain the adapter interface.
- The interface tends toward a lowest-common-denominator; provider-specific
  capabilities (e.g. differing tool-use or caching semantics) must be exposed via
  capability flags rather than leaking through the abstraction.

## Related
- [0001 — Hybrid Privacy Model](0001-hybrid-privacy-model.md)
- [0007 — Conversation-Orchestrated Runtime](0007-conversation-orchestrated-runtime.md)

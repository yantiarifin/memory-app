# ADR 0001: Hybrid Privacy Model

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin
- Supersedes: planned `0001-local-first`

## Context

A core tenet is that the user's history belongs to them: portable, transparent,
and under their control. At the same time, Product AI needs capable language
models to retrieve, connect, and synthesize insights — and the most capable
models today are cloud-hosted. Sending a user's entire personal conversation
history to a third-party API would directly contradict the privacy tenet.

We need a model that preserves user ownership without giving up generation
quality.

## Decision

Adopt a **hybrid, local-by-default** architecture:

- User data is stored locally by default.
- Embeddings are generated locally when possible.
- Only the **minimum retrieved context** required to answer a question or
  generate an insight is sent to the cloud LLM.
- The full conversation history is **never transmitted** unless the user
  explicitly opts in.
- The architecture remains provider-agnostic (see [0002](0002-ai-provider-abstraction.md))
  so fully local models can replace cloud models as they become capable.

## Consequences

### Positive
- Honors the "history belongs to the user" tenet while still using strong models.
- Retrieval runs locally; only top-k chunks ever cross the network.
- Degrades gracefully to a fully local deployment as local models improve.

### Trade-offs
- Requires a local embedding model and a local vector store — more moving parts.
- Context assembly must be careful: the "minimum necessary context" boundary is
  a privacy control, not just a token-budget optimization.
- Some cross-time reasoning quality may be bounded by how much context we are
  willing to send. We accept this in favor of privacy.

## Related
- [0002 — AI Provider Abstraction](0002-ai-provider-abstraction.md)
- [0004 — Insight Generation Cadence](0004-insight-generation-cadence.md)

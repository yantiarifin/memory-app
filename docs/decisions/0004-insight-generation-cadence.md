# ADR 0004: Insight Generation Cadence

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin

## Context

Insights are cross-time patterns: they depend on the whole corpus, not a single
conversation. But users also expect newly imported conversations to be reflected
without waiting for a full re-analysis. And when a user asks a novel question,
the system should be able to hypothesize on the spot — without polluting the
trusted, persisted insight set.

## Decision

Use **both** incremental and periodic generation:

- **Incremental:** new imports trigger updates to affected memories and candidate
  insights.
- **Periodic:** a scheduled background job re-analyzes the entire corpus to
  discover higher-order patterns that emerge over time.
- **On-demand:** the Conversation Agent may generate temporary hypotheses for
  novel questions, but these are **never persisted** until they pass the normal
  validation pipeline ([0005](0005-insight-validation-pipeline.md)).

This implies an insight lifecycle: `candidate → validated → persisted`, with
on-demand hypotheses living outside it until validated.

## Consequences

### Positive
- Responsive on import, while still finding patterns that only emerge at scale.
- On-demand hypotheses keep conversations useful without weakening trust.

### Trade-offs
- Requires a background scheduler and an insight lifecycle/status model.
- The periodic full-corpus job cost grows with history size; may need windowing
  or sampling strategies as corpora get large.

## Related
- [0005 — Insight Validation Pipeline](0005-insight-validation-pipeline.md)
- [0001 — Hybrid Privacy Model](0001-hybrid-privacy-model.md)

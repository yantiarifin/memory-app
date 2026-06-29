# ADR 0006: Evidence Bundle Architecture

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin

## Context

Trust is built through transparency: a user must always be able to explore *how*
an insight was formed. This requires a concrete, persisted record of the evidence
behind every insight — not a regenerated, possibly-different explanation each
time the insight is viewed.

## Decision

The **Validation Agent owns the Evidence Layer.** When an insight is validated,
it produces and persists an **Evidence Bundle** containing:

- Supporting excerpts
- Source conversation IDs
- Timeline references
- Confidence score
- Reasoning metadata
- Creation timestamp

The Conversation Agent simply **retrieves and presents** the evidence; it never
generates or alters it.

## Consequences

### Positive
- Single, clear owner for evidence production.
- Presentation is decoupled from generation — the same bundle is shown every
  time, so trust is stable and auditable.
- The bundle is an immutable record tied to the validated insight.

### Trade-offs
- Storage cost: excerpts are partially duplicated from source conversations;
  store references plus a cached snippet rather than full copies.
- Bundles are versioned with the insight; re-validation produces a new bundle
  rather than mutating the old one.

## Related
- [0005 — Insight Validation Pipeline](0005-insight-validation-pipeline.md)
- [0007 — Conversation-Orchestrated Runtime](0007-conversation-orchestrated-runtime.md)

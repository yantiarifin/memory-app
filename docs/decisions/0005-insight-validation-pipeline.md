# ADR 0005: Insight Validation Pipeline

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin

## Context

A central tenet is that every insight must earn trust and be grounded in
evidence. The Insight Agent produces candidate insights, but candidates vary in
strength, and the system must never present a weak or hallucinated conclusion as
truth. We also want to avoid wasting work re-proposing the same weak hypotheses
on every cycle.

## Decision

The Validation Agent runs a validation pipeline on each candidate insight:

1. **Strengthen:** attempt to strengthen a weak insight, up to a **maximum of 2
   retries**.
2. **Reject:** if it is still insufficient after retries, reject it.
3. **Remember rejections:** store the rejected hypothesis and the rejection
   reason, so the same weak insight is not repeatedly regenerated.

On success, validation produces and persists an Evidence Bundle (see
[0006](0006-evidence-bundle-architecture.md)).

## Consequences

### Positive
- Enforces the "every insight earns trust" tenet at a hard boundary.
- The retry budget bounds cost per candidate.
- Rejection memory prevents repeated churn on known-weak patterns.

### Trade-offs
- Rejection memory needs a similarity check to match future near-duplicate
  candidates, not just exact matches.
- Risk of over-suppressing an insight that later becomes valid as new evidence
  arrives; mitigate by allowing re-evaluation when new evidence materially
  changes the picture.

## Related
- [0004 — Insight Generation Cadence](0004-insight-generation-cadence.md)
- [0006 — Evidence Bundle Architecture](0006-evidence-bundle-architecture.md)

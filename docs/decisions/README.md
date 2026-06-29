# Architecture Decision Records

This directory records the significant architectural decisions for DiscoveryApp.
Each ADR captures the *context* and *why* behind a decision so it can be revisited
deliberately rather than by accident.

Format: each record states a Status, the Context that forced a decision, the
Decision itself, and its Consequences (including trade-offs).

## Index

| ADR | Title | Status |
|-----|-------|--------|
| [0001](0001-hybrid-privacy-model.md) | Hybrid Privacy Model | Accepted |
| [0002](0002-ai-provider-abstraction.md) | AI Provider Abstraction | Accepted |
| [0003](0003-logical-agents-vs-services.md) | Logical Agents vs. Pipeline Services | Accepted |
| [0004](0004-insight-generation-cadence.md) | Insight Generation Cadence | Accepted |
| [0005](0005-insight-validation-pipeline.md) | Insight Validation Pipeline | Accepted |
| [0006](0006-evidence-bundle-architecture.md) | Evidence Bundle Architecture | Accepted |
| [0007](0007-conversation-orchestrated-runtime.md) | Conversation-Orchestrated Runtime | Accepted |

## Notes

ADR 0001 supersedes the originally-sketched `0001-local-first`, and ADR 0002
supersedes the originally-sketched `0002-chatgpt-first` (see
`docs/project-architecture.txt`). The superseding records keep the original
intent while broadening it to the decisions actually made.

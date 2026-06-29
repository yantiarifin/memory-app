# ADR 0003: Logical Agents vs. Pipeline Services

- Status: Accepted
- Date: 2026-06-29
- Deciders: Yanti Arifin

## Context

The AI architecture initially described every Product AI component as an "agent."
Treating all of them as autonomous, LLM-driven agents would over-engineer the
system: several components (summarization, retrieval, entity extraction) are
better expressed as deterministic pipeline stages that happen to call a model.

## Decision

Classify Product AI components into two kinds:

**True AI Agents** — LLM-driven reasoning, non-deterministic:
- Conversation Agent
- Insight Agent
- Validation Agent

**Pipeline Services** — deterministic orchestration that may call an LLM
internally, but is not autonomous:
- Memory Service
- Retrieval Service
- Knowledge Graph Service

## Consequences

### Positive
- Keeps the architecture modular without the complexity of a full multi-agent
  system.
- Services are easier to test and reason about (deterministic boundaries).
- Clear separation of "reasoning" from "machinery."

### Trade-offs
- The agent/service line is a judgment call and may shift; a service can be
  promoted to an agent later if a stage genuinely needs autonomy.

## Related
- [0007 — Conversation-Orchestrated Runtime](0007-conversation-orchestrated-runtime.md)

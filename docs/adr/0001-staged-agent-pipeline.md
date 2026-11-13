# ADR-0001: Staged agent pipeline instead of a free-form autonomous agent

**Status:** Accepted

## Context
Fully autonomous agents that decide every next step themselves are flexible, but in enterprise settings they are hard to predict, debug and govern. They can loop, call tools unnecessarily and produce runs that are difficult to explain.

## Decision
Use a fixed sequence of stages (controller, router, collector, analyzer, memory), with the LLM making decisions inside each stage rather than choosing the overall flow.

## Consequences
- **Better:** predictable cost and latency; each stage can be tested, logged and tuned independently; failures are easy to locate.
- **Costs:** less flexible than an open-ended planner; new behaviors require adding or changing a stage.

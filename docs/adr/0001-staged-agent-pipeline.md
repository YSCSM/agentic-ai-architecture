# ADR-0001: A fixed LangGraph pipeline instead of a free-form autonomous agent

**Status:** Accepted

## Context
Fully autonomous agents that choose every next step are flexible, but in enterprise settings they are hard to predict, debug and govern. They can loop, over-call tools and produce runs that are difficult to explain.

## Decision
Model the agent as a LangGraph `StateGraph` with five fixed nodes (`detect_domain → plan → analyze → execute → reflect`) sharing a typed `AgentState`. The LLM makes decisions inside each node; the graph decides the order.

## Consequences
- **Better:** predictable cost and latency; each node can be tested and logged independently; the final state shows exactly what each step produced.
- **Costs:** every request runs every node, even simple ones; new behaviors need graph changes.
- **Evolution:** LangGraph supports conditional edges, so routing (for example, re-plan on low confidence) can be added without rewriting nodes.

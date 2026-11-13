# ADR-0004: End every run with a reflection node

**Status:** Accepted

## Context
Users and reviewers need to know how much to trust an answer. A plain RAG response gives no signal about fit or confidence.

## Decision
Add a final `reflect` node in which the LLM reviews the goal, plan, analysis and result, then reports fit, confidence and caveats alongside the answer.

## Consequences
- **Better:** answers come with an explicit confidence and caveats; low-confidence results are visible to the user.
- **Costs:** one more LLM call per request; self-assessment by the same model is a useful signal, not a guarantee.
- **Evolution:** use the confidence output to drive a conditional edge back to `plan` or `analyze`.

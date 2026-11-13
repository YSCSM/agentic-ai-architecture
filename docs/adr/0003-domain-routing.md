# ADR-0003: Route by domain before retrieval

**Status:** Accepted

## Context
Finance, insurance and general questions need different vocabulary, retrieval settings and answer styles. A single pipeline produces generic answers.

## Decision
Classify each request into a domain first, then run domain-specific retrieval and analysis prompts.

## Consequences
- **Better:** more relevant retrieval and answers per domain; domains can be added independently.
- **Costs:** an extra classification step adds latency; misrouted questions get weaker answers, so routing accuracy should be evaluated.

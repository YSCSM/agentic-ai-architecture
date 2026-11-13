# ADR-0003: Detect the domain first and use it as a retrieval filter

**Status:** Accepted

## Context
Finance, insurance and general questions use different vocabulary and documents. Searching all of a tenant's documents for every question returns loosely related results.

## Decision
The first node classifies the goal into a domain. The `analyze` node then queries Pinecone with both `namespace=tenant` and a `domain` metadata filter.

## Consequences
- **Better:** more relevant retrieval; domains can be added by adding documents with a new domain label.
- **Costs:** one extra LLM call; a misclassified question searches the wrong documents.
- **Mitigation:** the `reflect` node flags low-confidence answers; routing accuracy should be added to evaluation.

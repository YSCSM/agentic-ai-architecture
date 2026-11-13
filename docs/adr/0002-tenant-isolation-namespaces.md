# ADR-0002: Tenant isolation through JWT-scoped vector namespaces

**Status:** Accepted

## Context
Multiple organizations share one deployment. One tenant's documents must never appear in another tenant's answers.

## Decision
Derive tenant identity from the authenticated user's login domain, carry it in the JWT, and use it server-side to select the Pinecone namespace for every read and write. The namespace is never accepted from request parameters.

## Consequences
- **Better:** strong logical isolation with one shared index; simple to onboard a new tenant.
- **Costs:** relies on correct token validation; isolation is logical, not physical.
- **For stricter requirements:** separate indexes or accounts per tenant.

## Alternatives considered
- **Metadata filter per query:** easy to forget on one code path.
- **Separate index per tenant:** strongest isolation, higher cost and operational overhead.

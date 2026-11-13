# Architecture

## 1. Request flow

```mermaid
sequenceDiagram
    autonumber
    participant U as User (Streamlit)
    participant A as FastAPI
    participant C as Controller
    participant R as Router
    participant K as Collector
    participant N as Analyzer
    participant M as Memory
    participant V as Pinecone (tenant namespace)
    participant L as LLM

    U->>A: Question + JWT
    A->>A: Validate token, extract tenant
    A->>C: Start run (tenant, user, question)
    C->>M: Load conversation context
    C->>R: Classify intent
    R->>L: Intent classification
    L-->>R: finance / insurance / general
    C->>K: Collect evidence (domain, tenant)
    K->>V: Query namespace = tenant
    V-->>K: Relevant chunks
    C->>N: Analyze (question, evidence, context)
    N->>L: Grounded reasoning prompt
    L-->>N: Draft answer
    C->>M: Save turn
    C-->>A: Answer + sources
    A-->>U: Response
```

## 2. Control and safety limits

| Limit | Purpose |
|---|---|
| Maximum steps per run | Prevents loops |
| Timeouts per stage | Keeps latency predictable |
| Server-side tenant scoping | Prevents cross-tenant retrieval |
| Grounded analysis prompt | Answers come from collected evidence |
| Structured stage outputs | Each stage's output can be logged and inspected |

## 3. Deployment view

```mermaid
flowchart TB
    subgraph Azure
        UI[Streamlit app]
        API[FastAPI service]
    end
    UI --> API
    API --> PC[(Pinecone)]
    API --> OAI[OpenAI API]
```

## 4. Evolution path

| Next step | Benefit |
|---|---|
| Move orchestration to a graph framework (for example LangGraph) | Explicit state, retries and branching |
| Add tool calling to enterprise APIs | Agents act, not only answer |
| Add evaluation per domain route | Measure quality where it differs |
| Add tracing and audit logging (as in the Secure Hybrid RAG Platform) | Explainability in regulated settings |

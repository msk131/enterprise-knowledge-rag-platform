# Low-Level Design

```mermaid
sequenceDiagram
    participant User
    participant API as Query API
    participant ACL as Access Filter
    participant Vector as Vector Index
    participant Keyword as Keyword Index
    participant Meta as Metadata Store
    participant Rerank as Reranker
    participant LLM as Grounded Generator
    participant Eval as Evaluation Logger

    User->>API: Ask question
    API->>ACL: Resolve tenant, user, role, permissions
    ACL->>Meta: Load allowed document scope
    API->>Vector: Semantic search within allowed scope
    API->>Keyword: Keyword/BM25 search within allowed scope
    Vector-->>API: Candidate chunks
    Keyword-->>API: Candidate chunks
    API->>Rerank: Merge and rerank candidates
    Rerank-->>API: Ordered evidence
    API->>LLM: Generate answer with citations
    LLM-->>API: Grounded answer
    API->>Eval: Log retrieval and answer metrics
    API-->>User: Answer + citations
```

## Key Controls

| Control | Purpose |
| --- | --- |
| ACL filtering | Prevent retrieval from unauthorized documents. |
| Hybrid retrieval | Improve recall for semantic and exact-match queries. |
| Reranking | Reduce noisy context before generation. |
| Citations | Make answers traceable to source chunks. |
| Evaluation logging | Track quality, latency, and drift. |

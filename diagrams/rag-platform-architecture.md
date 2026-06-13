# RAG Platform Architecture

```mermaid
flowchart TB
    USER["User / Application"]
    API["FastAPI Gateway\nquery, retrieve, admin"]
    AUTH["Auth And Access Filter\nuser, tenant, role, document permissions"]

    UPLOAD["Document Upload"]
    INGEST["Ingestion Worker\nparse, clean, chunk, embed"]
    PARSE["Parsing Layer\nDocling, Tika, Unstructured,\nPyMuPDF, OCR"]
    CHUNK["Chunking And Metadata\nsource, page, section, hash"]
    EMBED["Embedding Service\nBGE / E5 / SentenceTransformers"]

    VECTOR[("Vector Store\nQdrant / pgvector")]
    KEYWORD[("Keyword Index\nOpenSearch / PostgreSQL FTS")]
    META[("PostgreSQL\nmetadata, permissions,\nruns, evaluations")]
    OBJECT[("Object Storage\nMinIO / S3")]

    RETRIEVE["Hybrid Retriever\nsemantic + keyword"]
    RERANK["Reranker\ncross-encoder / BGE reranker"]
    GENERATE["Grounded Generator\nLLM adapter"]
    EVAL["Evaluation Worker\nretrieval and answer quality"]
    OBS["Observability\nmetrics, traces, logs"]

    USER --> API
    API --> AUTH
    API --> UPLOAD
    UPLOAD --> OBJECT
    UPLOAD --> META
    UPLOAD --> INGEST
    INGEST --> PARSE
    PARSE --> CHUNK
    CHUNK --> EMBED
    EMBED --> VECTOR
    CHUNK --> KEYWORD
    CHUNK --> META

    AUTH --> RETRIEVE
    RETRIEVE --> VECTOR
    RETRIEVE --> KEYWORD
    RETRIEVE --> META
    RETRIEVE --> RERANK
    RERANK --> GENERATE
    GENERATE --> API

    EVAL --> RETRIEVE
    EVAL --> GENERATE
    EVAL --> META
    API --> OBS
    INGEST --> OBS
    RETRIEVE --> OBS
    GENERATE --> OBS
```

## Flow Summary

1. Documents are uploaded or batch imported.
2. Ingestion workers parse files, extract metadata, chunk content, and create embeddings.
3. Chunks are stored with source lineage and permissions.
4. User query is filtered by access rights.
5. Retriever combines semantic and keyword search.
6. Reranker improves final context order.
7. Generator answers only from retrieved evidence and returns citations.
8. Evaluation jobs measure retrieval and answer quality over time.


# Enterprise Knowledge RAG Platform

This project designs and builds a production-grade Retrieval-Augmented Generation platform using open-source components. The goal is to answer enterprise questions from trusted internal knowledge while preserving source traceability, access control, evaluation quality, and operational reliability.

This is not a simple "chat with PDF" demo. It is an end-to-end RAG platform with ingestion, parsing, chunking, metadata, retrieval, reranking, answer generation, citations, evaluation, observability, and administration.

## Why This Project Matters

Enterprise knowledge is scattered across documents, tickets, wikis, emails, PDFs, spreadsheets, contracts, runbooks, and system exports. Traditional search often returns many links but does not synthesize answers. General LLMs can answer fluently but may hallucinate or ignore internal policy.

A production RAG platform solves this by grounding answers in retrieved enterprise content and by making every answer explainable through citations and retrieval evidence.

## What We Are Building

The first version will support private document ingestion, semantic and lexical retrieval, grounded answer generation, source citation, and RAG evaluation.

Primary capabilities:

| Capability | Purpose |
| --- | --- |
| Document ingestion | Accept files, URLs, and batch imports. |
| Parsing pipeline | Extract text, layout, tables, and metadata from source documents. |
| Chunking strategy | Create retrieval-ready chunks with stable source references. |
| Embedding generation | Convert chunks into vector representations. |
| Hybrid retrieval | Combine vector search with keyword/BM25 search. |
| Reranking | Improve top-k context before generation. |
| Grounded generation | Produce answers using retrieved evidence only. |
| Citations | Link answers back to source document, page, section, and chunk. |
| Access control | Filter retrieval by tenant, user, role, and document permissions. |
| Evaluation | Measure faithfulness, retrieval precision, recall, answer relevance, and latency. |
| Observability | Track ingestion failures, retrieval quality, cost, latency, and model behavior. |

## How We Will Build It

Recommended open-source-first stack:

| Layer | Technology |
| --- | --- |
| API | Python FastAPI |
| Workflow orchestration | LangGraph or LlamaIndex workflows |
| Parsing | Docling, Apache Tika, Unstructured, PyMuPDF, Pandoc where useful |
| OCR | Tesseract or PaddleOCR |
| Embeddings | BGE, E5, or SentenceTransformers |
| Vector store | Qdrant or PostgreSQL with pgvector |
| Keyword search | OpenSearch or PostgreSQL full-text search |
| Reranking | BGE reranker or cross-encoder reranker |
| LLM serving | Ollama, vLLM, or a hosted model behind an adapter |
| Metadata store | PostgreSQL |
| Object storage | MinIO or S3-compatible storage |
| Queue | Redis Queue, Celery, or Kafka/Redpanda |
| Evaluation | RAGAS-style metrics, custom golden set, retrieval tests |
| Observability | OpenTelemetry, Prometheus, Grafana, structured logs |
| Delivery | Docker Compose locally, Kubernetes/OpenShift for production-style deployment |

## Design Principles

- **Ground every answer**: no source evidence, no confident answer.
- **Preserve document lineage**: every chunk knows its source, page, section, parser version, and ingestion run.
- **Separate ingestion from querying**: slow parsing jobs should not block user queries.
- **Use hybrid retrieval**: vector search alone is not enough for enterprise terms, IDs, codes, and exact references.
- **Evaluate continuously**: retrieval and generation quality must be measured, not guessed.
- **Respect permissions**: retrieval must never return content the user cannot access.
- **Keep models replaceable**: embedding, reranker, and LLM providers should be adapters.

## Core APIs

```text
POST /api/v1/documents
GET  /api/v1/documents/{documentId}
POST /api/v1/ingestion-runs
GET  /api/v1/ingestion-runs/{runId}

POST /api/v1/query
POST /api/v1/retrieve
POST /api/v1/evaluations
GET  /api/v1/evaluations/{evaluationId}
```

## Project Structure

```text
enterprise-knowledge-rag-platform/
├── README.md
├── plan.md
├── diagrams/
│   └── rag-platform-architecture.md
└── docs/
```

## Current Status

This project starts with planning and architecture. The next step is to implement the ingestion pipeline, choose the first vector store, and build a minimal query API with citations.


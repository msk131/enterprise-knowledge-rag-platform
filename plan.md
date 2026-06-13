# Enterprise Knowledge RAG Platform Plan

This plan follows four stages: define the business purpose, scope the platform, analyze technical risks, then build in vertical slices.

## Phase 0: Why

The platform exists to make enterprise knowledge searchable, answerable, traceable, and governable.

Business reasons:

- Reduce time spent searching across documents and systems.
- Improve answer quality by grounding responses in approved sources.
- Preserve citations for audit, review, and trust.
- Enable knowledge reuse across operations, engineering, support, risk, and compliance teams.
- Reduce hallucination risk compared with ungrounded LLM usage.

Engineering reasons:

- Demonstrates production RAG design beyond notebooks.
- Covers ingestion, retrieval, generation, evaluation, and operations.
- Shows open-source AI system integration.
- Creates a reusable foundation for document intelligence, assistants, and knowledge workflows.

## Phase 1: What

Initial scope:

- Upload and batch ingest documents.
- Parse documents into structured text and metadata.
- Chunk and embed content.
- Store source files, chunks, embeddings, and ingestion metadata.
- Retrieve context with hybrid semantic and keyword search.
- Rerank retrieved chunks.
- Generate cited answers.
- Evaluate retrieval and answer quality.

Out of scope for first version:

- Autonomous agents that take external actions.
- Multi-modal answer generation.
- Fine-tuning models.
- Complex enterprise identity provider integration.
- Multi-region deployment.

## Phase 2: How

Core services:

| Service | Responsibility |
| --- | --- |
| API service | Query, retrieval, document metadata, and admin APIs. |
| Ingestion worker | Parse, clean, chunk, embed, and index documents. |
| Retrieval service | Hybrid search, filters, reranking, and context assembly. |
| Generation service | Prompt construction, model call, citation enforcement. |
| Evaluation worker | Golden-set evaluation and regression scoring. |
| Admin service | Re-indexing, parser versioning, failed-job replay, dataset management. |

Core stores:

| Store | Purpose |
| --- | --- |
| PostgreSQL | Documents, chunks, users, permissions, ingestion runs, evaluations. |
| Qdrant or pgvector | Vector embeddings and similarity search. |
| OpenSearch or PostgreSQL FTS | Keyword and exact-term retrieval. |
| MinIO/S3 | Original documents and extracted artifacts. |
| Redis or Kafka/Redpanda | Background ingestion and evaluation jobs. |

## Phase 3: Analyze

Key risks and controls:

| Risk | Control |
| --- | --- |
| Hallucinated answer | Require citations, use evidence-bounded prompts, evaluate faithfulness. |
| Poor retrieval | Hybrid search, reranking, chunk evaluation, query expansion tests. |
| Permission leakage | Metadata filters at retrieval time and tests for access boundaries. |
| Bad parsing | Parser versioning, extraction QA, fallback parsers, document-type routing. |
| Chunk drift | Stable chunk IDs, chunking strategy tests, lineage metadata. |
| Slow queries | Cache safe retrieval artifacts, tune top-k, use reranker only after first-pass retrieval. |
| Expensive inference | Model adapter, prompt budget control, context compression. |
| Index inconsistency | Ingestion state machine and re-index jobs. |

## Phase 4: Act

### Slice 1: Project Foundation

- Create FastAPI project.
- Add Docker Compose for PostgreSQL, vector store, object storage, and queue.
- Add health endpoint.
- Add structured logging and OpenTelemetry basics.

Done when:

- API starts locally.
- Dependencies run locally.
- Health endpoint and basic integration test pass.

### Slice 2: Document Ingestion

- Add document upload API.
- Store original file in object storage.
- Create document and ingestion run records.
- Add background ingestion worker.

Done when:

- A document can be uploaded and an ingestion run is tracked.

### Slice 3: Parsing And Chunking

- Parse PDF, DOCX, HTML, TXT, and Markdown first.
- Extract metadata and source references.
- Chunk content with stable chunk IDs.

Done when:

- Parsed chunks preserve source document, page/section, parser version, and hash.

### Slice 4: Embedding And Indexing

- Generate embeddings using an open-source embedding model.
- Store vectors and metadata.
- Add indexing status and retry handling.

Done when:

- Chunks are searchable by vector similarity.

### Slice 5: Retrieval And Reranking

- Add `/retrieve` API.
- Combine semantic search with keyword search.
- Add metadata filters and reranking.

Done when:

- Retrieval returns ranked chunks with source references and permission filters.

### Slice 6: Grounded Answer API

- Add `/query` API.
- Build prompt from retrieved evidence.
- Return answer with citations and confidence signals.

Done when:

- Answers include source references and refuse unsupported claims.

### Slice 7: Evaluation And Operations

- Add golden question set.
- Measure context precision, context recall, faithfulness, answer relevance, and latency.
- Add dashboards for ingestion and query behavior.

Done when:

- Changes can be evaluated against a repeatable benchmark.


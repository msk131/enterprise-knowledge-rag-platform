# Enterprise Knowledge RAG Platform

This repository documents a minimal enterprise RAG platform design. The focus is ingestion, permission-aware retrieval, citations, evaluation, and operational visibility.

## Design Focus

| Area | Design Point |
| --- | --- |
| Ingestion | Parse source documents, extract metadata, chunk content, and preserve lineage. |
| Access control | Filter retrieval by tenant, role, user, and document permissions. |
| Retrieval | Combine vector search, keyword search, and reranking before generation. |
| Grounding | Generate answers only from retrieved evidence and return citations. |
| Evaluation | Track retrieval quality, answer faithfulness, latency, and cost. |
| Operations | Monitor ingestion failures, retrieval drift, model behavior, and system health. |

## Diagrams

| Diagram | Purpose |
| --- | --- |
| [diagrams/01-high-level-design.md](diagrams/01-high-level-design.md) | Platform-level RAG architecture. |
| [diagrams/02-low-level-design.md](diagrams/02-low-level-design.md) | Query-time retrieval and answer flow. |

## Current Status

Design-only repository. Implementation is intentionally out of scope until the ingestion, retrieval, permission, and evaluation boundaries are clear.

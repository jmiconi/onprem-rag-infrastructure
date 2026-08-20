# On-Prem RAG Infrastructure

Reference architecture for running **local LLM and retrieval-augmented generation (RAG) services on-premise**, with an emphasis on data control, observability and reproducible operations.

This repository documents infrastructure patterns rather than proprietary datasets or production configuration.

## What this project demonstrates

- Local LLM serving with Ollama
- Vector search with Qdrant
- Retrieval API design
- Knowledge ingestion and chunking pipelines
- Workflow orchestration with n8n / Flowise
- Web UI integration
- RAG evaluation and operational governance
- Separation between source data, index, retrieval and generation

## Architecture

```text
                 Knowledge sources
                        |
                        v
              +-------------------+
              | Ingestion pipeline|
              | parse / chunk     |
              +---------+---------+
                        |
                 embeddings
                        |
                        v
                  +-----------+
                  |  Qdrant   |
                  +-----+-----+
                        ^
                        |
User -> Web UI -> Retriever API
                        |
                        | selected context
                        v
                   +---------+
                   | Ollama  |
                   | local LLM|
                   +----+----+
                        |
                        v
                     Answer

         n8n / Flowise -> orchestration and workflows
```

## Core design idea

RAG is treated as an **infrastructure pipeline**, not as a single chatbot:

`Source governance → ingestion → indexing → retrieval → context assembly → generation → evaluation`

Each stage can be measured, tested and replaced independently.

## Repository structure

```text
onprem-rag-infrastructure/
├── docs/
│   ├── architecture.md
│   ├── security-and-governance.md
│   └── evaluation.md
└── README.md
```

## Why on-premise

On-premise inference can be useful when requirements include:

- control over data location
- integration with internal systems
- predictable local connectivity
- experimentation without sending source documents to external inference services
- infrastructure-level control over models, indexes and retention

It also introduces operational responsibilities: capacity planning, model lifecycle, index freshness, backups, access control and monitoring.

## RAG quality is more than model quality

A strong model cannot compensate for bad retrieval. The platform should therefore evaluate:

- whether the correct document was indexed
- whether the correct chunks were retrieved
- whether citations/source references are traceable
- whether stale content remains in the index
- whether the generated answer is grounded in retrieved evidence

## Security

This repository contains only sanitized architecture. Production deployments should isolate secrets, protect source documents and vector stores, restrict administrative endpoints and define retention/governance policies.

## Roadmap

- Hybrid retrieval example
- Ingestion job templates
- RAG test harness
- Observability dashboard patterns
- Backup/restore runbook

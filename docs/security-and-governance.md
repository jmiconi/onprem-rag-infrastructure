# Security and governance

RAG platforms inherit the sensitivity of the data they index. Treat the knowledge pipeline as production infrastructure, not as an isolated AI experiment.

## Controls

- Keep secrets outside Git and load them from a protected secret store or environment file.
- Restrict administrative endpoints for the model runtime, vector database and orchestration tools.
- Define which source collections are allowed to enter the index.
- Store source identifiers and revision metadata so retrieved evidence can be traced.
- Rebuild or remove vectors when source content is deleted or superseded.
- Back up configuration and authoritative sources; document how the index is rebuilt.

## Data lifecycle

```text
Source created
    |
    v
Approved for indexing
    |
    v
Parsed / chunked / embedded
    |
    v
Indexed with source metadata
    |
    +--> updated -> re-index
    |
    +--> retired -> remove derived vectors
```

## Access model

Retrieval authorization should match the permissions of the underlying information. A technically correct search result is still a security failure if it exposes evidence the requesting user should not see.

## Logging

Operational logs should make ingestion failures, retrieval latency and model errors observable without copying sensitive document content into logs unnecessarily.

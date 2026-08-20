# Architecture

## Layers

### 1. Source layer

Documents, operational notes and structured records remain the system of record. The vector index is derived data and must be rebuildable.

### 2. Ingestion layer

The ingestion process:

1. discovers allowed sources
2. parses content
3. normalizes metadata
4. chunks text
5. generates embeddings
6. upserts vectors and metadata

### 3. Retrieval layer

A dedicated retriever receives the user query, searches the index and returns ranked evidence. Keeping retrieval outside the UI makes testing and replacement easier.

### 4. Generation layer

The local LLM receives the user request plus retrieved evidence. The generation layer should not have unrestricted access to source systems.

### 5. Orchestration layer

Workflow tools can trigger ingestion, connect APIs and implement business flows, but should not become the only place where core retrieval logic exists.

## Flow

```text
Sources -> Ingestion -> Embeddings -> Vector DB
                                  ^
                                  |
User -> UI -> Retriever ----------+
              |
              v
           Context -> Local LLM -> Response
```

## Operational boundaries

- Source documents are authoritative.
- Vector indexes are rebuildable caches.
- Retriever behavior is versioned and testable.
- Model choice is independent from knowledge ingestion.
- Secrets and internal endpoints stay outside source control.

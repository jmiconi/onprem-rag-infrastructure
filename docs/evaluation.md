# RAG evaluation

A RAG platform should be tested at the retrieval layer before judging the final generated answer.

## Suggested test set

Create a small set of known questions with:

- expected source document
- expected section or fact
- whether the question should be answerable
- known ambiguous cases

## Retrieval checks

For each question record:

- Was the expected source returned?
- At what rank?
- Was the relevant text present in the retrieved chunk?
- Were unrelated chunks ranked above it?
- Was the indexed content current?

## Generation checks

After retrieval passes, evaluate:

- groundedness in retrieved evidence
- unsupported claims
- source traceability
- refusal/uncertainty when evidence is missing
- consistency across repeated runs

## Operational metrics

Useful platform metrics include:

- ingestion duration and failures
- document/chunk counts
- retrieval latency
- top-k retrieval success on the test set
- model response latency
- failed requests

A model upgrade should not be considered an improvement if retrieval quality or operational reliability regresses.

# Close Elasticsearch and OpenSearch clients through cleanup hooks

Elasticsearch and OpenSearch upload/search clients inherit no-op cleanup hooks, while the unused Closable* wrappers do not close the clients actually constructed. Their configurators also leave connections open.

This implements explicit cleanup hooks for the six Elasticsearch/OpenSearch configurator, uploader and searcher classes, closes the active client, clears its reference, and removes the unused destructor wrappers. Repeated cleanup calls are safe.

Addresses https://github.com/qdrant/vector-db-benchmark/issues/92

Validation:
- 20 server-independent cleanup tests passed, including BaseClient cleanup for both engines.
- Same tests against the unchanged baseline: 14 failed and 6 passed.
- Black check on all 7 changed files passed.
- Ruff E4/E7/E9/F checks on all changed files passed.
- git diff --check passed.
- Live server benchmarks and the full multi-engine test suite were not run. This change covers the existing cleanup hooks; exception-path orchestration and worker-finalization behavior are outside this patch.

AI assistance: ChatGPT prepared the implementation, tests and description and ran the validation above. The account owner has not yet personally reviewed the patch.

Upstream baseline and changes are carried in client-cleanup.patch. Local commit: a894efb5279d7301fedeaf3f3abc17ee7d80491a.


Status: patch prepared and tested; upstream PR not yet opened. GitHub connector lacks fork creation capability.

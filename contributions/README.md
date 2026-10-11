# Prepared open-source contributions

These are reproducible fixes prepared for review, not claims of accepted upstream contributions. No forks, upstream issues or upstream pull requests were created in this pass.

| Project | Work | Validation | Status |
| --- | --- | --- | --- |
| [Qdrant vector-db-benchmark](https://github.com/qdrant/vector-db-benchmark/issues/92) | Close Elasticsearch/OpenSearch clients through cleanup hooks | 20 tests pass; baseline 14 fail / 6 pass | [Prepared patch](qdrant-92/PR.md), from the earlier pass |
| [Docker SDK for Python](https://github.com/docker/docker-py) | Honor explicitly empty environment mappings | 94 tests pass, 4 Windows-only skips; new regression fails on baseline | [Prepared patch](docker-empty-environment/PR.md), no upstream issue/PR |
| [Meilisearch Python](https://github.com/meilisearch/meilisearch-python) | Preserve integer zero JSON bodies | 6 tests pass; baseline 1 fail / 5 pass | [Prepared patch and issue draft](meilisearch-zero-body/PR.md), no upstream issue/PR |

The Docker full unit suite is blocked by AF_UNIX socket restrictions. Meilisearch integration tests and type checking are not validated. See each PR draft for commands, baseline commits, limitations and AI disclosure.

## Contribution workflow
1. Recheck upstream main and existing issues/PRs to avoid duplicate work.
2. Review the code, reproduce the tests, and satisfy each project's contribution requirements (including Docker DCO sign-off and Meilisearch issue/AI disclosure).
3. Publish an upstream PR from an authorized upstream branch or a fork. Without either source branch, GitHub cannot create a cross-repository PR from these patch files.
4. Track actual review and merge URLs here when they exist. Prepared patches alone do not establish an accepted contribution to another company project.

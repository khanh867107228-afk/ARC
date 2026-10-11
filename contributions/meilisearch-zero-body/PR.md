# Preserve zero integer request bodies

## Problem and change
`HttpRequests.put(path, 0)` serializes the body as JSON `null` because the serialization condition checks truthiness and only explicitly preserves booleans. This silently changes an integer setting into the reset/null form. `Index.update_search_cutoff_ms(0)` routes through this affected method.

Explicitly preserve integer bodies alongside booleans. The patch is intentionally limited to the integer-capable `send_request` path. Add six mocked transport cases for 0, 1, False, True, None and an empty dictionary.

## Evidence and reproduction
- Source baseline: https://github.com/meilisearch/meilisearch-python/blob/506fd06c3ecc4c729fb894c8258833503681d12a/meilisearch/_httprequests.py
- Public setting method: https://github.com/meilisearch/meilisearch-python/blob/506fd06c3ecc4c729fb894c8258833503681d12a/meilisearch/index.py

```python
from unittest.mock import Mock, patch
import requests
from meilisearch._httprequests import HttpRequests
from meilisearch.config import Config
response = Mock(spec=requests.Response)
response.content = b'{}'
response.json.return_value = {}
with patch('requests.put', return_value=response) as put:
    put.__name__ = 'put'
    HttpRequests(Config('http://localhost:7700', 'test-key', timeout=None)).put(
        'indexes/test/settings/search-cutoff-ms', 0)
    print(put.call_args.kwargs['data'])
# Before: null
# After: 0
```
The mock prevents network requests. This verifies SDK serialization, not server acceptance of any specific setting value.

## Validation
Python 3.12, pytest 9.1.1, camel-converter 6.0.0, Ruff 0.17.0.
- New test file on baseline: 1 failed (zero), 5 passed.
- `python -m pytest --noconftest -o addopts='' tests/client/test_http_request_bodies.py -q`: 6 passed. `--noconftest` isolates transport unit tests from the repository's service-dependent autouse fixtures.
- `ruff check meilisearch/_httprequests.py tests/client/test_http_request_bodies.py`: passed.
- `ruff format --check meilisearch/_httprequests.py tests/client/test_http_request_bodies.py`: passed.
- `git diff --check`: passed.
- Full Meilisearch integration suite and mypy have not been run; no local Meilisearch service was started.

## Submission status
Prepared patch; no upstream issue or PR has been opened. Searches for zero, serialization, null and search-cutoff found no matching fix; search is not exhaustive. Recheck before submission. The contributing guide asks for an issue describing the change, so an issue draft is included below.

Baseline: `506fd06c3ecc4c729fb894c8258833503681d12a`
Local change: `773efe47206b3719981b027646966aacf312e03f`
Apply: `git am zero-body.patch` on that baseline.

AI disclosure: ChatGPT identified the behavior, wrote the implementation, tests and description, and ran the checks above. The account owner has not personally reviewed this patch. This disclosure is required by Meilisearch's contributing guide.

## Issue draft (not posted)
Title: Integer zero request bodies are serialized as null

On baseline `506fd06`, `HttpRequests.put(path, 0)` sends `data="null"`, whereas the expected JSON body is `"0"`. The integer-accepting `update_search_cutoff_ms` method uses this transport path. The mocked reproduction above demonstrates the wrong outbound request without contacting a server. Preserve integer values in the serialization condition and add a regression test. Backend behavior is not claimed here.

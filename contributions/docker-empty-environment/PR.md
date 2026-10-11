# Honor explicitly empty environment mappings

## Problem and change
`kwargs_from_env(environment={})` unexpectedly falls back to `os.environ`. With ambient `DOCKER_HOST`, `DOCKER_CERT_PATH` and `DOCKER_TLS_VERIFY`, the caller receives those host/TLS settings despite explicitly supplying an empty mapping. This contradicts the existing alternate-environment test's stated behavior.

Only use the process environment when `environment is None`. Add a regression test with ambient Docker host and TLS settings; an explicit empty mapping now returns `{}`. Omitted/None and populated mappings retain their behavior.

## Evidence and reproduction
- Source baseline: https://github.com/docker/docker-py/blob/56343ddf8f0c44281e151c2dad016c16cdb8393d/docker/utils/utils.py
- Existing tests: https://github.com/docker/docker-py/blob/56343ddf8f0c44281e151c2dad016c16cdb8393d/tests/unit/utils_test.py
- Historical alternate-environment support: https://github.com/docker/docker-py/pull/1006

```python
import os
from docker.utils import kwargs_from_env
os.environ['DOCKER_HOST'] = 'tcp://localhost:2375'
print(kwargs_from_env(environment={}))
# Before: {'base_url': 'tcp://localhost:2375'}
# After: {}
```
No Docker daemon is contacted by this reproduction.

## Validation
Python 3.12, pytest 9.1.1, requests from the runtime; Ruff 0.1.8 matches the project's pinned linter.
- New regression on baseline: 1 failed.
- `python -m pytest tests/unit/utils_test.py tests/unit/client_test.py -q`: 94 passed, 4 Windows-only tests skipped.
- `ruff check docker/utils/utils.py tests/unit/utils_test.py`: passed with Ruff 0.1.8.
- `git diff --check`: passed.
- Full unit-suite attempt: stopped at 1 failure, 175 passed, 1 skipped; environment prohibits AF_UNIX socket creation in `UnixSocketStreamTest`. The complete suite, Windows tests and Docker integration tests are not validated.

## Submission status
Prepared patch; no upstream issue or pull request has been opened. Issue/PR searches for `kwargs_from_env` and empty environments found no matching current fix, but search is not exhaustive. Check again immediately before submission.

Baseline: `56343ddf8f0c44281e151c2dad016c16cdb8393d`
Local change: `0082e06c2014609a42753eedcee8b69c6c246f4c`
Apply: `git am empty-environment.patch` on that baseline.
Docker asks contributors to sign off their work. This patch deliberately carries no DCO certification on the account owner's behalf; owner review and appropriate sign-off remain before upstream submission.

AI disclosure: ChatGPT identified the behavior, wrote the fix and regression test, and ran the checks above. The account owner has not personally reviewed this patch.

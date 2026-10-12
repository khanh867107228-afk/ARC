# Tari local audit — 2026-10-12

User objective: complete eligible bounty work. Status: initial local review completed; no confirmed vulnerability, submission, accepted reward or payment.

## Verified target

- Official contest: https://community.tari.com/t/extra-contest-ootle-security-bug-hunt/397
- Repository: https://github.com/tari-project/tari-ootle
- Checkout: 392d805579db35fcc8e8bc1aba84adc1fffe0c22
- Local-only testing; no third-party nodes or accounts contacted.
- Rewards are XTM; eligibility, duplicates and severity are decided by the organizer.

## Completed

- Cloned source and checked out the pinned commit.
- Read SECURITY.md and current official contest rules.
- Reviewed wallet JWT verification, scope matcher, API-key management authorization and transaction-request authorization paths.
- Installed Rust stable locally; original pinned-toolchain download failed.
- Ran existing permission tests with Rust stable 1.99.0 and explicit system OpenSSL paths:

```sh
OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu OPENSSL_INCLUDE_DIR=/usr/include cargo +stable test -p tari_ootle_walletd_client --lib permissions::tests --locked
```

Result: 38 passed, 0 failed, 7 filtered out. Working tree unchanged. These are existing regression tests, not a new vulnerability proof of concept. No security finding is claimed.

## Next work and payment dependencies

Continue tracing resource-scoped wallet handlers and transaction approval invariants, then reproduce any credible issue solely in local tests. Compare with public known issues and advisories before claiming originality. Keep all potential vulnerability details private, outside this public repository.

A valid private report requires a reproducible issue with demonstrated impact and a payout address. No Tari payout address has been supplied and private advisory access/whitelist has not been verified for this account. Do not invent payout data, accept financial terms on behalf of the user, or mark rewards as received without payment evidence.

## Paid task screening

- https://algora.io/CapSoftware/bounties?status=open — no open bounties observed.
- https://algora.io/documenso/bounties?status=open — no open bounties observed.
- https://algora.io/challenges/turso — challenge completed, submissions closed.
- https://algora.io/challenges/prettier — winner already announced.

These listings were excluded from payable work. No external report, issue, PR or contact was sent.

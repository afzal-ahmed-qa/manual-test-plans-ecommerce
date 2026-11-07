# Test Execution Report – API, cycle 1

| | |
|---|---|
| Date | 7 Nov 2026 |
| Tester | Afzal Ahmed |
| Build / environment | Production demo, https://automationexercise.com |
| Tools | curl, Node.js fetch |
| Scope of this cycle | API, read-only and negative calls |

## Summary

| | Count |
|---|---|
| Cases written | 38 |
| Executed | 23 |
| Passed | 10 |
| Failed | 12 |
| Query (needs product decision) | 1 |
| Not run | 15 |

**Execution rate: 61%.** **Pass rate of executed cases: 43%** (10 of 23).

The failures come from five defects, and one of them (AE-API-001, HTTP status codes) accounts
for 8 of the 12. Functionally the read endpoints return the right data. Search, filtering
and injection handling all behaved well. The problems are in the HTTP contract (status codes,
headers) and transport security.

## Results by area

| Area | Cases | Pass | Fail | Query | Not run |
|---|---|---|---|---|---|
| Products | 5 | 3 | 2 | 0 | 0 |
| Brands | 3 | 1 | 2 | 0 | 0 |
| Search | 8 | 5 | 2 | 1 | 0 |
| Login verification | 6 | 0 | 2 | 0 | 4 |
| User accounts | 12 | 0 | 1 | 0 | 11 |
| Cross-cutting | 4 | 1 | 3 | 0 | 0 |

## Defects raised

| ID | Severity | Summary |
|---|---|---|
| AE-API-001 | Major | Errors return HTTP 200; real code only in body |
| AE-API-005 | Major | Plain HTTP served, no redirect, no HSTS |
| AE-API-002 | Minor | JSON sent as text/html |
| AE-API-003 | Minor | Brands list has duplicates |
| AE-API-004 | Trivial | Allow header inaccurate |

Details: [defect-reports.md](../defects/defect-reports.md). Three open questions (Q-01 to Q-03)
are logged on the same page.

## Not run, and why

The 15 cases that create, update, log in to or delete accounts (API-LGN-01/03/05/06 and most of
API-ACC) were not executed in this cycle. They write data to a shared public site, and I wanted
the account lifecycle agreed first: dedicated `example.com` addresses, one account per run,
deleted at the end. They're fully specified and ready to run in cycle 2.

## UI status

The [UI test cases](../ui/test-cases.md) (50 cases) are written and reviewed. UI execution is
planned as cycle 2, after the account endpoints are confirmed, since UI signup and checkout
depend on the same account behaviour.

## Recommendation

Not ready to sign off the API. AE-API-001 should be fixed or formally accepted first, because
it changes how every client must handle errors. Then run cycle 2 (account lifecycle + UI) and
retest AE-API-001/002/005.

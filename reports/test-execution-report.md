# Test Execution Report

| | |
|---|---|
| Date | 7 Nov 2026 |
| Tester | Afzal Ahmed |
| Environment | Production demo, https://automationexercise.com |
| Browser / tools | Chrome 130 on Windows 11 (headed, watched live), curl, Node.js fetch, axe-core |
| Cycles | 1: API read-only calls, 2: API account lifecycle and all UI cases |

## Summary

| | UI | API | Total |
|---|---|---|---|
| Cases | 50 | 38 | 88 |
| Executed | 49 | 38 | 87 |
| Pass | 45 | 17 | 62 |
| Fail | 4 | 20 | 24 |
| Blocked / Query | 1 / 0 | 0 / 1 | 1 / 1 |
| Pass rate (executed) | 92% | 45% | 71% |

**10 defects raised:** 5 Major, 4 Minor, 1 Trivial. Five open questions for the product owner.
See [defect-reports.md](../defects/defect-reports.md).

The web shop works well for the main customer journeys. Registration, login, search, filters,
cart, checkout, invoice and account deletion all behaved as expected, and all three end-to-end
journeys passed. The UI failures are in input validation (cart quantity, card details),
accessibility and mobile layout.

The API returns the right data and the account lifecycle works. Most of its failures come from a
single contract problem: errors are reported with HTTP 200 (AE-API-001, 15 of the 20 failures).
Fixing that one defect would raise the API pass rate from 45% to 84%.

## UI results by module

| Module | Cases | Pass | Fail | Blocked | Defects |
|---|---|---|---|---|---|
| Registration, login and account | 10 | 10 | 0 | 0 | – |
| Product catalogue and search | 10 | 10 | 0 | 0 | – (Q-05) |
| Shopping cart | 6 | 5 | 1 | 0 | AE-UI-003 |
| Checkout, payment and invoice | 8 | 7 | 1 | 0 | AE-UI-001 |
| Contact us, subscription and navigation | 7 | 7 | 0 | 0 | – |
| Non-functional | 6 | 3 | 2 | 1 | AE-UI-002, AE-UI-004 |
| End-to-end journeys | 3 | 3 | 0 | 0 | – |

## API results by group

| Group | Cases | Pass | Fail | Query | Defects |
|---|---|---|---|---|---|
| Products | 5 | 3 | 2 | 0 | AE-API-001 |
| Brands | 3 | 1 | 2 | 0 | AE-API-001, AE-API-003 |
| Search | 8 | 5 | 2 | 1 | AE-API-001 (Q-01, Q-02) |
| Login verification | 6 | 2 | 4 | 0 | AE-API-001 |
| User accounts | 12 | 5 | 7 | 0 | AE-API-001, AE-API-006 (Q-04) |
| Cross-cutting | 4 | 1 | 3 | 0 | AE-API-002, AE-API-004, AE-API-005 |

## Requirements

18 of 32 requirements met, 13 not met, 1 blocked. See the
[traceability matrix](../traceability-matrix.md) for the mapping.

## How the run was done

- **API:** each case sent with the exact method and parameters in [api/test-cases.md](../api/test-cases.md).
  The account lifecycle used one new `@example.com` account per run, deleted at the end.
- **UI:** cases were run in a visible Chrome window, slowed down so every step could be watched,
  with a helper script doing the clicking and typing. Each case started from a fresh browser
  session so carts and logins couldn't leak between cases. Ad domains were blocked, so timings
  are slightly better than a typical visitor would see.
- **Re-checks before logging a failure:** UI-AUTH-06 first failed on a page-load timeout; it was
  re-run and passed, so no defect was raised. UI-CART-03 first looked like a pass, but that came
  from my own input method. Re-run by typing the value normally, it exposed AE-UI-003.
- **Test data clean-up:** every account created during the run (UI and API) was deleted at the end.

## Not executed

| Case | Reason |
|---|---|
| UI-NFR-01 Cross-browser smoke | Only Chromium was available for this cycle. Needs Firefox, Edge and Safari (or a cloud grid). |

## Recommendation

Not ready for sign-off.

1. **Fix AE-UI-003 first.** A negative quantity creating a negative total is a revenue risk and
   needs server-side validation.
2. **Agree how to handle AE-API-001.** Either return real HTTP status codes or update the
   documentation so clients know to read `responseCode`. Every API consumer depends on this decision.
3. Fix AE-API-006 and AE-API-005 before any real customer data goes through the API.
4. Schedule AE-UI-002 (accessibility) for the next release.
5. Then retest the failed cases and run UI-NFR-01 across browsers.

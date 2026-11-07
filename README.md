# Manual test plans: e-commerce web shop and REST API

This is the manual testing side of my portfolio. It's a full set of test documentation for
[Automation Exercise](https://automationexercise.com), a public practice shop with a product
catalogue, cart, checkout and a 14-endpoint REST API. There's a test strategy, separate UI and API
test plans, 88 test cases, a traceability matrix, defect reports and an execution report.

The site doesn't publish a specification, which is pretty common on real projects too. So I
worked out the requirements myself from the live site and the API documentation, wrote the cases
from those, ran them, and wrote up what I found.

For automation, see my Playwright framework:
[playwright-typescript-e2e-framework](https://github.com/afzal-ahmed-qa/playwright-typescript-e2e-framework).

## Results

Run on 7 November 2026.

| | Cases | Pass | Fail | Other |
|---|---|---|---|---|
| UI | 50 | 45 | 4 | 1 blocked |
| API | 38 | 17 | 20 | 1 query |

I raised 10 defects (5 Major, 4 Minor, 1 Trivial) and logged 5 questions where the expected
behaviour wasn't clear enough to call it a bug. All 32 requirements have test cases; 18 are
currently met.

The API failure count looks worse than it is. 15 of the 20 come from one problem: the API answers
every error with HTTP 200 and only puts the real code (400, 404, 405) inside the JSON body. The
data itself was correct in those cases.

The one blocked UI case is the cross-browser check. I only had Chrome available for this run.

## Bugs worth looking at

- Typing `-1` as the quantity on a product page puts Rs. -500 in the cart. 0 and 1,000,000 are
  accepted too ([AE-UI-003](defects/defect-reports.md#ae-ui-003-cart-accepts-zero-and-negative-quantities-giving-a-negative-total),
  screenshot included). This is the one I'd fix first.
- The `createAccount` API happily creates an account for the email `not-an-email-123`
  ([AE-API-006](defects/defect-reports.md#ae-api-006-createaccount-accepts-an-email-with-no-valid-format)).
- The API is served over plain HTTP without redirecting to HTTPS, and the account endpoints take
  the password as form data ([AE-API-005](defects/defect-reports.md#ae-api-005-api-answers-over-plain-http-with-no-redirect-or-hsts)).
- The payment form accepts a card number of `abcd` and an expiry date in 2020
  ([AE-UI-001](defects/defect-reports.md#ae-ui-001-payment-accepts-malformed-and-expired-card-details)).

Plenty worked well, too. All three end-to-end purchase journeys passed, the API never returned a
user's password, and a wrong password got exactly the same response as an unknown email, so you
can't use it to find out who has an account.

## What's in here

- [test-strategy.md](test-strategy.md): scope, test types, approach, environments, entry and exit
  criteria, risks.
- [ui/ui-test-plan.md](ui/ui-test-plan.md): features and risk, decision tables, boundary values,
  test data and exploratory charters. The cases are in [ui/test-cases.md](ui/test-cases.md).
- [api/api-test-plan.md](api/api-test-plan.md): the endpoints, what's checked on each one, and
  the account lifecycle. The cases are in [api/test-cases.md](api/test-cases.md).
- [traceability-matrix.md](traceability-matrix.md): requirements mapped to test cases and their
  latest result.
- [defects/defect-reports.md](defects/defect-reports.md): every defect with steps to reproduce,
  plus the open questions.
- [reports/test-execution-report.md](reports/test-execution-report.md): results by area, how the
  run was done, and my recommendation.
- [evidence/](evidence) has the screenshots, and [templates/](templates) has the bug report and
  test case templates I used.

Both case lists are also available as CSV ([UI](ui/test-cases.csv), [API](api/test-cases.csv)).
They use standard columns, so you can import them into TestRail, Xray or Excel.

## How I went about it

I spent the first part just using the site and reading the API docs, then wrote down what it
should do as numbered requirements (`REQ-`, `NFR-`, `API-REQ-`). Checkout, payment and login got
the most cases because that's where failures would cost a real shop money or expose accounts.

For the inputs with rules (cart quantity, mobile number, card details, search) I used equivalence
partitioning and boundary values. Login outcomes are a small decision table in the UI plan.

API cases were run with curl. For the UI I ran each case in a visible Chrome window, starting from
a fresh session so carts and logins couldn't carry over between cases. All test accounts used
throwaway `@example.com` addresses and were deleted at the end.

I re-checked anything that failed before raising it, which caught a mistake of my own. The first
time through, the cart quantity case passed, but that turned out to be my input method rather
than the site. When I typed the value normally, the negative-total bug showed up. Another case
failed on a page timeout and passed when re-run, so I didn't raise a defect for it.

## Naming

Test cases are `UI-<module>-NN` (AUTH, PROD, CART, CHK, MISC, NFR, E2E) and `API-<group>-NN` (PRD,
BRD, SRC, LGN, ACC, GEN). Defects are `AE-UI-NNN` and `AE-API-NNN`, and open questions are `Q-NN`.

---

Afzal Ahmed, [github.com/afzal-ahmed-qa](https://github.com/afzal-ahmed-qa)

Automation Exercise is run by its own team. I'm not connected with them; this was written as
practice and for my portfolio.

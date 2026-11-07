# Manual Test Plans – E-commerce Web Shop and REST API

Test strategy, test plans, test cases, traceability, defect reports and an execution report for
**[Automation Exercise](https://automationexercise.com)**, a public e-commerce practice site with a
web shop and a 14-endpoint REST API.

This repo shows the manual QA side of my work: how I analyse an application with no
specification, turn that into requirements and risk-based test cases, run them and report what I
find. The automation side is in
[playwright-typescript-e2e-framework](https://github.com/afzal-ahmed-qa/playwright-typescript-e2e-framework).

## At a glance

| | UI | API |
|---|---|---|
| Test cases | 50 | 38 |
| Priority (High / Medium / Low) | 22 / 17 / 11 | 20 / 14 / 4 |
| Executed | 49 of 50 | 38 of 38 |
| Result | 45 pass · 4 fail · 1 blocked | 17 pass · 20 fail · 1 query |
| Defects raised | 4 | 6 |

10 defects in total (5 Major, 4 Minor, 1 Trivial). 32 of 32 requirements covered, 18 met
([traceability matrix](traceability-matrix.md)).

## What's inside

| Document | What it covers |
|---|---|
| [Test strategy](test-strategy.md) | Objectives, scope, test types, approach, environments, entry/exit criteria, risks |
| [UI test plan](ui/ui-test-plan.md) | Features, risk rating, decision tables, boundaries, data, schedule, exploratory charters |
| [UI test cases](ui/test-cases.md) · [CSV](ui/test-cases.csv) | 50 cases: registration, catalogue, cart, checkout, contact, non-functional, 3 E2E journeys |
| [API test plan](api/api-test-plan.md) | Endpoint inventory, checks per endpoint, method matrix, input classes, account lifecycle |
| [API test cases](api/test-cases.md) · [CSV](api/test-cases.csv) | 38 cases with request, expected and actual result |
| [Traceability matrix](traceability-matrix.md) | 32 derived requirements → test cases → latest result |
| [Defect reports](defects/defect-reports.md) | 10 reproducible defects with steps, curl commands or screenshots, plus 5 open questions |
| [Execution report](reports/test-execution-report.md) | Results by module, how the run was done, what was re-checked, recommendation |
| [Evidence](evidence) | Screenshots attached to defects |
| [Templates](templates) | Bug report and test case templates |

The CSV files use standard columns (ID, Title, Priority, Steps, Expected, Status...) and import
straight into TestRail, Jira Xray, Zephyr or Excel.

## Highlights

- **A negative quantity gives a negative cart total.** Typing `-1` on a product page puts
  Rs. -500 in the cart ([AE-UI-003](defects/defect-reports.md#ae-ui-003--cart-accepts-zero-and-negative-quantities-giving-a-negative-total), with screenshot).
- **Every API error comes back as HTTP 200.** The documented 400/404/405 codes only appear
  inside the JSON body ([AE-API-001](defects/defect-reports.md#ae-api-001--error-responses-return-http-200-instead-of-the-documented-4xx-code)).
  That single defect accounts for 15 of the 20 API failures.
- **The API creates accounts for emails like `not-an-email-123`** ([AE-API-006](defects/defect-reports.md#ae-api-006--createaccount-accepts-an-email-with-no-valid-format)),
  and it is served over plain HTTP with no redirect ([AE-API-005](defects/defect-reports.md#ae-api-005--api-answers-over-plain-http-with-no-redirect-or-hsts)).
- **What worked:** all three end-to-end customer journeys, registration, login, checkout and
  invoice download. The API never returned the password, wrong passwords got the same reply as
  unknown users, and injection characters were handled safely.

## How I approached it

1. **Explored the app** page by page and read the API documentation to understand what exists.
2. **Wrote requirements** from that, since no specification is published, and rated each area by risk.
3. **Designed cases** with equivalence partitioning, boundary values and decision tables. The
   negative paths get as much attention as the happy paths.
4. **Ran the cases.** API cases with curl; UI cases in a visible Chrome window, each from a fresh
   session. Anything that failed was re-checked before a defect was raised, and every test account
   was deleted afterwards.
5. **Reported** what passed, what failed, what couldn't be run and why, and what I'd recommend next.

## Conventions

| Prefix | Meaning |
|---|---|
| `UI-AUTH`, `UI-PROD`, `UI-CART`, `UI-CHK`, `UI-MISC`, `UI-NFR`, `UI-E2E` | UI test case by module |
| `API-PRD`, `API-BRD`, `API-SRC`, `API-LGN`, `API-ACC`, `API-GEN` | API test case by endpoint group |
| `REQ-`, `NFR-`, `API-REQ-` | Derived functional, non-functional and API requirements |
| `AE-UI-`, `AE-API-` | Defect report |
| `Q-` | Open question for the product owner |

---

Afzal Ahmed · QA Engineer · [GitHub](https://github.com/afzal-ahmed-qa)

*Automation Exercise is a free practice site run by its owners. This repo is independent test
documentation written for learning and portfolio purposes.*

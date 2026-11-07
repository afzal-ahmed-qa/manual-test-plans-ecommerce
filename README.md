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
| Executed | Planned for cycle 2 | 23 of 38 (read-only scope) |
| Result | – | 10 pass · 12 fail · 1 query |
| Defects raised | – | 5 (2 Major, 2 Minor, 1 Trivial) |

Requirements covered: 32 of 32 ([traceability matrix](traceability-matrix.md)).

## What's inside

| Document | What it covers |
|---|---|
| [Test strategy](test-strategy.md) | Objectives, scope, test types, approach, environments, entry/exit criteria, risks |
| [UI test plan](ui/ui-test-plan.md) | Features, risk rating, decision tables, boundaries, data, schedule, exploratory charters |
| [UI test cases](ui/test-cases.md) · [CSV](ui/test-cases.csv) | 50 cases: registration, catalogue, cart, checkout, contact, non-functional, 3 E2E journeys |
| [API test plan](api/api-test-plan.md) | Endpoint inventory, checks per endpoint, method matrix, input classes, account lifecycle |
| [API test cases](api/test-cases.md) · [CSV](api/test-cases.csv) | 38 cases with request, expected and actual result |
| [Traceability matrix](traceability-matrix.md) | 32 derived requirements → test cases → latest result |
| [Defect reports](defects/defect-reports.md) | 5 reproducible defects with curl steps, plus 3 open questions |
| [Execution report](reports/test-execution-report.md) | Cycle 1 results, what wasn't run and why, recommendation |
| [Templates](templates) | Bug report and test case templates |

The CSV files use standard columns (ID, Title, Priority, Steps, Expected, Status...) and import
straight into TestRail, Jira Xray, Zephyr or Excel.

## Highlights from the API run

- **Every error comes back as HTTP 200.** The documented 400/405 codes only appear inside the JSON
  body ([AE-API-001](defects/defect-reports.md#ae-api-001--error-responses-return-http-200-instead-of-the-documented-4xx-code)).
  This one defect caused 8 of the 12 failures.
- **The API is served over plain HTTP** with no redirect or HSTS, and the account endpoints take
  passwords as form data ([AE-API-005](defects/defect-reports.md#ae-api-005--api-answers-over-plain-http-with-no-redirect-or-hsts)).
- **The brands list has duplicates:** 34 entries for 8 brands ([AE-API-003](defects/defect-reports.md#ae-api-003--brandslist-repeats-each-brand-once-per-product)).
- Search handled injection characters and script tags safely, and the read endpoints answered in
  under 2 seconds.

## How I approached it

1. **Explored the app** page by page and read the API documentation to understand what exists.
2. **Wrote requirements** from that, since no specification is published, and rated each area by risk.
3. **Designed cases** with equivalence partitioning, boundary values and decision tables. The
   negative paths get as much attention as the happy paths.
4. **Ran the API cases** that don't change data, recorded actual results and raised defects with
   one-line reproduction steps.
5. **Reported** what passed, what failed, what wasn't run and why, and what I'd recommend next.

## Conventions

| Prefix | Meaning |
|---|---|
| `UI-AUTH`, `UI-PROD`, `UI-CART`, `UI-CHK`, `UI-MISC`, `UI-NFR`, `UI-E2E` | UI test case by module |
| `API-PRD`, `API-BRD`, `API-SRC`, `API-LGN`, `API-ACC`, `API-GEN` | API test case by endpoint group |
| `REQ-`, `NFR-`, `API-REQ-` | Derived functional, non-functional and API requirements |
| `AE-API-` | Defect report |
| `Q-` | Open question for the product owner |

---

Afzal Ahmed · QA Engineer · [GitHub](https://github.com/afzal-ahmed-qa)

*Automation Exercise is a free practice site run by its owners. This repo is independent test
documentation written for learning and portfolio purposes.*

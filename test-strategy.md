# Test Strategy – Automation Exercise

| | |
|---|---|
| Product | Automation Exercise e-commerce demo (web shop + REST API) |
| URL | https://automationexercise.com |
| Author | Afzal Ahmed |
| Version | 1.0 – 7 Nov 2026 |

## 1. Context

Automation Exercise is a public practice shop: customers browse a catalogue of clothing,
search, filter by category and brand, manage a cart, register, check out with a card and
download an invoice. Product, brand, search and account features are also exposed through a
14-endpoint REST API.

There's no written specification, so I derived requirements from the live site, the official
[API list](https://automationexercise.com/api_list) and what a customer would reasonably expect
from any online shop. They're listed in the [traceability matrix](traceability-matrix.md).

## 2. Objectives

1. Confirm the main purchase journey works: find a product → cart → checkout → pay → invoice.
2. Confirm account features behave correctly, including the negative paths (duplicate email,
   wrong password, deleted account).
3. Check the API against its own documentation: status codes, messages, response shape.
4. Flag security and usability risks that would matter for a real shop, even where the demo
   site doesn't need them fixed.

## 3. Scope

| In scope | Out of scope |
|---|---|
| All customer-facing pages in the header menu | Admin or back-office functions (none exposed) |
| The 14 documented API endpoints | Load, stress and soak testing (shared public site) |
| Chrome, Firefox, Edge, Safari (latest) | Real payment processing; only test card data is used |
| Desktop and one mobile viewport | Penetration testing beyond basic input checks |
| Basic accessibility, HTTPS and response-time checks | Email delivery (signup and subscription emails) |

## 4. Test levels and types

| Level / type | Where | How |
|---|---|---|
| Functional, positive and negative | UI and API | Scripted test cases ([UI](ui/test-cases.md), [API](api/test-cases.md)) |
| Boundary and validation | Quantity, signup, payment, search inputs | Equivalence partitioning and boundary values |
| End-to-end journeys | UI | Three customer scenarios across several pages |
| Exploratory | UI | Time-boxed sessions per module, notes kept with the run |
| API contract | API | Status code, message, schema and header checks per endpoint |
| Security (basic) | UI and API | Account enumeration, injection characters, HTTPS, data exposure |
| Compatibility | UI | Cross-browser smoke and one mobile viewport |
| Accessibility | UI | Keyboard-only use plus an automated axe / Lighthouse scan |
| Regression | UI and API | High-priority cases re-run after any fix |

## 5. Approach

- **Risk first.** Checkout, payment and login are High priority because a failure there loses
  sales or exposes accounts. Cosmetic and helper features are Low.
- **Test design.** Equivalence classes and boundaries for every input with rules (quantity,
  mobile number, card fields). Decision tables for login outcomes (known/unknown email ×
  right/wrong password).
- **Data.** Every run uses new, clearly fake customers (`firstname.lastname+<timestamp>@example.com`)
  and the public test card 4111 1111 1111 1111. Test accounts are deleted at the end of a run.
- **API before UI.** API cases run first. A failing endpoint explains failing UI cases quickly.
- **Evidence.** Screenshot or response body for every failed case, attached to the defect.

## 6. Environments and tools

| | |
|---|---|
| Environment | Production demo site (no separate test environment exists) |
| Browsers | Chrome, Firefox, Edge, Safari – latest stable |
| API | Postman or curl; responses compared against the API list |
| Tracking | Test cases in Markdown/CSV (importable into TestRail / Jira Xray), defects in [defect-reports.md](defects/defect-reports.md) |
| Accessibility | axe DevTools, Lighthouse |

## 7. Entry and exit criteria

**Entry:** site and API reachable; test data prepared; test cases reviewed.

**Exit:**
- 100% of High-priority cases executed.
- No open Critical defects; Major defects have an agreed decision (fix / accept / defer).
- Execution report published with pass/fail figures and open risks.

## 8. Defect management

| Severity | Meaning | Example |
|---|---|---|
| Critical | Blocks a core journey, no workaround | Cannot place an order |
| Major | Feature wrong or contract broken, workaround exists | API returns 200 for errors |
| Minor | Small functional or data issue | Duplicate brands in a list |
| Trivial | Cosmetic | Typo in a label |

Priority (P1–P3) is set separately from severity, based on business impact. Every defect has
steps, expected vs actual, evidence and environment. See the [template](templates/bug-report-template.md).

## 9. Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Shared public site; data and behaviour can change at any time | Results not repeatable | Record date and evidence with every run |
| Only one environment (production) | Tests create real records | Use clearly fake data and delete accounts afterwards |
| Ads and third-party scripts on pages | Overlays block clicks, slow loads | Note in results; use an ad blocker for functional runs |
| No specification | Expected results may be disputed | Derived requirements reviewed; open questions logged separately |

## 10. Deliverables

| Deliverable | Location |
|---|---|
| Test strategy | this document |
| UI test plan and cases | [ui/](ui) |
| API test plan and cases | [api/](api) |
| Traceability matrix | [traceability-matrix.md](traceability-matrix.md) |
| Defect reports and open questions | [defects/defect-reports.md](defects/defect-reports.md) |
| Execution report | [reports/test-execution-report.md](reports/test-execution-report.md) |

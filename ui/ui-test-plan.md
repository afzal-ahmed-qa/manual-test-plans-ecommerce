# UI Test Plan – Automation Exercise Web Shop

| | |
|---|---|
| Application | https://automationexercise.com |
| Author | Afzal Ahmed |
| Version | 1.0 – 7 Nov 2026 |
| Related | [Test strategy](../test-strategy.md) · [Test cases](test-cases.md) · [CSV](test-cases.csv) |

## 1. Features under test

| Module | What's covered | Cases | Risk |
|---|---|---|---|
| Registration, login, account | Signup (two-step form), login, logout, delete account | 10 | High: account access |
| Catalogue and search | All products, product detail, search, category and brand filters, reviews, recommended items | 10 | Medium |
| Cart | Add from list and detail page, quantity, remove, empty state, guest cart after login | 6 | High: revenue |
| Checkout and payment | Login gate, address and order review, card payment, validation, invoice | 8 | High: revenue |
| Contact, subscription, navigation | Contact form with upload, footer subscription, header links, scroll-up | 7 | Low |
| Non-functional | Cross-browser, mobile, keyboard, axe scan, load time, HTTPS | 6 | Medium |
| End-to-end journeys | New customer, returning customer, browsing shopper | 3 | High |
| **Total** | | **50** | |

Priority split: 22 High, 17 Medium, 11 Low.

## 2. Out of scope

Email confirmations, real card processing, admin features, the "Video Tutorials" and external
links, and the API (covered by the [API test plan](../api/api-test-plan.md)).

## 3. Test design notes

**Login outcomes (decision table)**

| Email registered? | Password correct? | Expected | Case |
|---|---|---|---|
| Yes | Yes | Logged in | UI-AUTH-05 |
| Yes | No | Generic "email or password is incorrect" | UI-AUTH-06 |
| No | – | Same generic message | UI-AUTH-07 |
| Yes, then account deleted | Yes | Refused | UI-AUTH-09 |

**Cart quantity (boundaries)**

| Partition | Values | Expected |
|---|---|---|
| Below minimum | 0, -1 | Rejected or corrected to 1 |
| Valid | 1, 4, 999 | Accepted, line total = price × qty |
| Very large | 1 000 000 | Capped or refused with a message |
| Not a number | abc | Rejected |

**Payment fields:** each mandatory field is tested empty on its own (UI-CHK-05), then with
malformed values (UI-CHK-06). The demo site isn't expected to validate cards like a real gateway,
so a pass there is reported as a risk, not a defect.

## 4. Test data

| Data | Value |
|---|---|
| New customer | `firstname.lastname+<timestamp>@example.com`, password `Secret#123`, Canada / Ontario / Toronto |
| Search terms | `jean`, `top`, `TOP`, `zzqqxx` |
| Card | Name: test name, number 4111 1111 1111 1111, CVC 123, expiry 12/2030 |
| Upload | small `.txt` file under 1 KB |

## 5. Schedule and order of execution

| Day | Activity |
|---|---|
| 1 | Smoke (UI-AUTH-05, UI-PROD-01, UI-CART-01, UI-CHK-04), then AUTH and PROD modules |
| 2 | CART, CHK and E2E journeys; exploratory session on checkout |
| 3 | MISC and NFR; retest of fixed defects; execution report |

## 6. Exploratory charters

| Charter | Time box |
|---|---|
| Explore the cart with unusual quantities and rapid add/remove clicks to find calculation or state errors | 45 min |
| Explore checkout with long, unicode and empty address values to find layout or validation problems | 45 min |
| Explore navigation with browser back/forward and refresh during checkout to find lost state | 30 min |

## 7. Entry / exit and reporting

As in the [test strategy](../test-strategy.md#7-entry-and-exit-criteria). Results are recorded in
the Status and Actual result columns of [test-cases.csv](test-cases.csv) and summarised in the
[execution report](../reports/test-execution-report.md).

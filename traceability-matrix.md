# Requirements Traceability Matrix

Derived requirements for Automation Exercise, mapped to the test cases that cover them.
The site has no published specification, so requirements were written from its features,
the documented API list and normal e-commerce expectations.

| Requirement | Area | Description | Test cases | Latest result |
|---|---|---|---|---|
| REQ-01 | UI | Visitor can register an account; duplicate and invalid emails are refused | [UI-AUTH-01](ui/test-cases.md#ui-auth-01), [UI-AUTH-02](ui/test-cases.md#ui-auth-02), [UI-AUTH-03](ui/test-cases.md#ui-auth-03), [UI-AUTH-04](ui/test-cases.md#ui-auth-04), [UI-E2E-01](ui/test-cases.md#ui-e2e-01) | Not run |
| REQ-02 | UI | Registered user can log in and log out; wrong credentials are refused | [UI-AUTH-05](ui/test-cases.md#ui-auth-05), [UI-AUTH-06](ui/test-cases.md#ui-auth-06), [UI-AUTH-07](ui/test-cases.md#ui-auth-07), [UI-AUTH-08](ui/test-cases.md#ui-auth-08), [UI-AUTH-10](ui/test-cases.md#ui-auth-10), [UI-E2E-02](ui/test-cases.md#ui-e2e-02) | Not run |
| REQ-03 | UI | Logged-in user can delete their account | [UI-AUTH-09](ui/test-cases.md#ui-auth-09), [UI-E2E-01](ui/test-cases.md#ui-e2e-01) | Not run |
| REQ-04 | UI | Catalogue lists products with a detail page per product | [UI-PROD-01](ui/test-cases.md#ui-prod-01), [UI-PROD-02](ui/test-cases.md#ui-prod-02) | Not run |
| REQ-05 | UI | Products can be searched by keyword | [UI-PROD-03](ui/test-cases.md#ui-prod-03), [UI-PROD-04](ui/test-cases.md#ui-prod-04), [UI-PROD-05](ui/test-cases.md#ui-prod-05), [UI-E2E-02](ui/test-cases.md#ui-e2e-02) | Not run |
| REQ-06 | UI | Products can be filtered by category and brand | [UI-PROD-06](ui/test-cases.md#ui-prod-06), [UI-PROD-07](ui/test-cases.md#ui-prod-07), [UI-E2E-03](ui/test-cases.md#ui-e2e-03) | Not run |
| REQ-07 | UI | Customers can review a product | [UI-PROD-08](ui/test-cases.md#ui-prod-08), [UI-PROD-09](ui/test-cases.md#ui-prod-09) | Not run |
| REQ-08 | UI | Products can be added to the cart with a chosen quantity | [UI-PROD-10](ui/test-cases.md#ui-prod-10), [UI-CART-01](ui/test-cases.md#ui-cart-01), [UI-CART-02](ui/test-cases.md#ui-cart-02), [UI-CART-03](ui/test-cases.md#ui-cart-03), [UI-E2E-01](ui/test-cases.md#ui-e2e-01), [UI-E2E-02](ui/test-cases.md#ui-e2e-02), [UI-E2E-03](ui/test-cases.md#ui-e2e-03) | Not run |
| REQ-09 | UI | Products can be removed from the cart | [UI-CART-04](ui/test-cases.md#ui-cart-04), [UI-CART-05](ui/test-cases.md#ui-cart-05), [UI-E2E-03](ui/test-cases.md#ui-e2e-03) | Not run |
| REQ-10 | UI | Guest cart is kept when the user logs in | [UI-CART-06](ui/test-cases.md#ui-cart-06), [UI-E2E-03](ui/test-cases.md#ui-e2e-03) | Not run |
| REQ-11 | UI | Checkout requires an account and shows address and order review | [UI-CHK-01](ui/test-cases.md#ui-chk-01), [UI-CHK-02](ui/test-cases.md#ui-chk-02), [UI-CHK-03](ui/test-cases.md#ui-chk-03), [UI-E2E-01](ui/test-cases.md#ui-e2e-01), [UI-E2E-02](ui/test-cases.md#ui-e2e-02) | Not run |
| REQ-12 | UI | Order is placed after valid card payment and the cart is emptied | [UI-CHK-04](ui/test-cases.md#ui-chk-04), [UI-CHK-05](ui/test-cases.md#ui-chk-05), [UI-CHK-06](ui/test-cases.md#ui-chk-06), [UI-CHK-08](ui/test-cases.md#ui-chk-08), [UI-E2E-01](ui/test-cases.md#ui-e2e-01), [UI-E2E-02](ui/test-cases.md#ui-e2e-02) | Not run |
| REQ-13 | UI | Invoice can be downloaded after ordering | [UI-CHK-07](ui/test-cases.md#ui-chk-07), [UI-E2E-01](ui/test-cases.md#ui-e2e-01) | Not run |
| REQ-14 | UI | Visitors can send a contact message with an attachment | [UI-MISC-01](ui/test-cases.md#ui-misc-01), [UI-MISC-02](ui/test-cases.md#ui-misc-02) | Not run |
| REQ-15 | UI | Visitors can subscribe to the newsletter | [UI-MISC-03](ui/test-cases.md#ui-misc-03), [UI-MISC-04](ui/test-cases.md#ui-misc-04), [UI-MISC-05](ui/test-cases.md#ui-misc-05) | Not run |
| REQ-16 | UI | Site navigation and page utilities work | [UI-MISC-06](ui/test-cases.md#ui-misc-06), [UI-MISC-07](ui/test-cases.md#ui-misc-07) | Not run |
| NFR-01 | UI | Works on current Chrome, Firefox, Edge and Safari | [UI-NFR-01](ui/test-cases.md#ui-nfr-01) | Not run |
| NFR-02 | UI | Usable on mobile screen sizes | [UI-NFR-02](ui/test-cases.md#ui-nfr-02) | Not run |
| NFR-03 | UI | Meets WCAG 2.1 AA for core flows | [UI-NFR-03](ui/test-cases.md#ui-nfr-03), [UI-NFR-04](ui/test-cases.md#ui-nfr-04) | Not run |
| NFR-04 | UI | Main pages load in under 3 seconds | [UI-NFR-05](ui/test-cases.md#ui-nfr-05) | Not run |
| NFR-05 | UI | Site is served over HTTPS only | [UI-NFR-06](ui/test-cases.md#ui-nfr-06) | Not run |
| API-REQ-01 | API | GET productsList returns the product catalogue; other methods return 405 | API-PRD-01, API-PRD-02, API-PRD-03, API-PRD-04, API-PRD-05 | Fail |
| API-REQ-02 | API | GET brandsList returns the brands; other methods return 405 | API-BRD-01, API-BRD-02, API-BRD-03 | Fail |
| API-REQ-03 | API | POST searchProduct returns products matching search_product; missing parameter returns 400 | API-SRC-01, API-SRC-02, API-SRC-03, API-SRC-04, API-SRC-05, API-SRC-06, API-SRC-07, API-SRC-08 | Fail |
| API-REQ-04 | API | POST verifyLogin confirms valid credentials (200) or reports missing (400) / unknown (404) | API-LGN-01, API-LGN-02, API-LGN-03, API-LGN-04, API-LGN-05, API-LGN-06 | Fail |
| API-REQ-05 | API | POST createAccount registers a user (201) | API-ACC-01, API-ACC-02, API-ACC-03, API-ACC-04 | Not run |
| API-REQ-06 | API | GET getUserDetailByEmail returns a user by email | API-ACC-05, API-ACC-06, API-ACC-07 | Fail |
| API-REQ-07 | API | PUT updateAccount updates a user (200) | API-ACC-08, API-ACC-09 | Not run |
| API-REQ-08 | API | DELETE deleteAccount removes a user (200) | API-ACC-10, API-ACC-11, API-ACC-12 | Not run |
| API-REQ-09 | API | Responses use correct HTTP status codes and headers | API-GEN-01, API-GEN-02 | Fail |
| API-REQ-10 | API | API is served over HTTPS only | API-GEN-03 | Fail |
| API-REQ-11 | API | Read endpoints respond in under 2 seconds | API-GEN-04 | Pass |

**Coverage:** 32 of 32 requirements have at least one test case.

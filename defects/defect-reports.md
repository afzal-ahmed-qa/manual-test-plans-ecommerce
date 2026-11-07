# Defect Reports and Open Questions

Found while running the [API test cases](../api/test-cases.md) on 7 Nov 2026.
Every defect can be reproduced with the curl command shown.

| ID | Title | Severity | Priority | Status |
|---|---|---|---|---|
| AE-API-001 | Error responses return HTTP 200 instead of the documented 4xx code | Major | P1 | Open |
| AE-API-002 | JSON responses are sent with `Content-Type: text/html` | Minor | P2 | Open |
| AE-API-003 | `brandsList` repeats each brand once per product | Minor | P3 | Open |
| AE-API-004 | `Allow` header lists methods the endpoint rejects | Trivial | P3 | Open |
| AE-API-005 | API answers over plain HTTP with no redirect or HSTS | Major | P2 | Open |

---

## AE-API-001 · Error responses return HTTP 200 instead of the documented 4xx code

| | |
|---|---|
| Severity / Priority | Major / P1 |
| Found by | API-PRD-04, API-PRD-05, API-BRD-03, API-SRC-05, API-SRC-08, API-LGN-02, API-LGN-04, API-ACC-06 |
| Environment | Production, 7 Nov 2026, curl 8.x |

**Steps**
```bash
curl -i -X POST https://automationexercise.com/api/productsList
curl -i -X POST https://automationexercise.com/api/searchProduct
```

**Expected:** HTTP 405 for the first call and HTTP 400 for the second, as stated in the API list.

**Actual:** both return `HTTP/1.1 200 OK`. The intended code only appears inside the body:
```json
{"responseCode": 405, "message": "This request method is not supported."}
```

**Impact:** clients, gateways and monitoring that rely on the HTTP status treat every error as
success. Any consumer has to parse the body to find out whether a call failed.

---

## AE-API-002 · JSON responses are sent with `Content-Type: text/html`

| | |
|---|---|
| Severity / Priority | Minor / P2 |
| Found by | API-GEN-01 (all endpoints tested) |

**Steps**
```bash
curl -s -D - -o /dev/null https://automationexercise.com/api/productsList
```

**Expected:** `Content-Type: application/json`

**Actual:** `Content-Type: text/html; charset=utf-8`, while the body is JSON.

**Impact:** HTTP clients that parse by content type (and browsers rendering the response)
treat the payload as HTML. Tools like Postman don't pretty-print or validate it as JSON by default.

---

## AE-API-003 · `brandsList` repeats each brand once per product

| | |
|---|---|
| Severity / Priority | Minor / P3 |
| Found by | API-BRD-02 |

**Steps:** `curl -s https://automationexercise.com/api/brandsList`, then count distinct `brand` values.

**Expected:** one entry per brand (8 brands appear in the web shop's Brands sidebar).

**Actual:** 34 entries, one for every product, with only 8 distinct brand names. "Polo" alone
appears six times, with ids 1, 8, 29, 30, 33 and 37.

**Impact:** a client building a brand filter from this endpoint shows duplicates, and the ids
don't identify a brand.

---

## AE-API-004 · `Allow` header lists methods the endpoint rejects

| | |
|---|---|
| Severity / Priority | Trivial / P3 |
| Found by | API-GEN-02 |

**Steps:** `curl -I https://automationexercise.com/api/productsList`

**Expected:** `Allow: GET` (plus HEAD/OPTIONS if supported).

**Actual:** `HTTP/1.1 405` with `Allow: GET, OPTIONS, PUT, POST, DELETE`, yet POST, PUT and
DELETE all answer "This request method is not supported."

**Impact:** misleading for API discovery tools and for anyone reading the headers.

---

## AE-API-005 · API answers over plain HTTP with no redirect or HSTS

| | |
|---|---|
| Severity / Priority | Major / P2 |
| Found by | API-GEN-03 |

**Steps**
```bash
curl -s -D - -o /dev/null http://automationexercise.com/api/productsList
curl -s -D - -o /dev/null https://automationexercise.com/api/productsList | grep -i strict-transport
```

**Expected:** the http:// call is redirected (301/308) to https://, and HTTPS responses include
`Strict-Transport-Security`.

**Actual:** http:// returns `200 OK` with the full product list; no HSTS header on HTTPS.

**Impact:** for a real shop this matters most on the account endpoints, which take email and
password as form data: on plain HTTP they would travel unencrypted.

---

## Open questions for the product owner

These aren't clearly defects because the behaviour isn't specified. They need a decision.

| ID | Question | Observed | Case |
|---|---|---|---|
| Q-01 | Should search match on category as well as product name? | Searching "top" returns two shirts because their category is "Kids > Tops & Shirts" | API-SRC-01 |
| Q-02 | What should an empty `search_product` return? | All 34 products. A missing parameter returns an error, but an empty one doesn't | API-SRC-04 |
| Q-03 | Should price be numeric in the API? | `"price": "Rs. 500"`: currency and amount in one string, so clients must parse it | API-PRD-02 |

# API Test Cases – Automation Exercise

REST API at https://automationexercise.com/api. Scope and approach are in the [API test plan](api-test-plan.md).
CSV version: [test-cases.csv](test-cases.csv). Results are from the run on 2026-11-07, see the [execution report](../reports/test-execution-report.md).

**38 cases**, Pass 17, Fail 20, Query 1

Where a case expects a specific HTTP status, the HTTP status line is what is judged, even if the JSON body carries the right code.

## Products

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-PRD-01 | Get all products<br>*High, Positive* | `GET /api/productsList` | HTTP 200. JSON with a non-empty "products" array. | Pass | HTTP 200, body responseCode 200, 34 products. |
| API-PRD-02 | Product objects follow the documented shape<br>*High, Schema* | `GET /api/productsList` | Every product has id (number), name, price, brand (strings) and category.usertype.usertype + category.category. | Pass | All 34 products match, e.g. {"id":1,"name":"Blue Top","price":"Rs. 500","brand":"Polo",...}. Note: price is a display string ("Rs. 500"), not a number (Q-03). |
| API-PRD-03 | Product ids are unique<br>*Medium, Data integrity* | `GET /api/productsList` | No duplicate id values. | Pass | 34 distinct ids. |
| API-PRD-04 | POST to products list is rejected<br>*High, Negative, method* | `POST /api/productsList` | HTTP 405 "This request method is not supported." (as documented). | Fail | HTTP 200; the 405 only appears as responseCode inside the body. **AE-API-001** |
| API-PRD-05 | DELETE to products list is rejected<br>*Medium, Negative, method* | `DELETE /api/productsList` | HTTP 405. | Fail | HTTP 200 with responseCode 405 in the body. **AE-API-001** |

## Brands

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-BRD-01 | Get all brands<br>*High, Positive* | `GET /api/brandsList` | HTTP 200. JSON with a "brands" array of {id, brand}. | Pass | HTTP 200, 34 entries, shape correct. |
| API-BRD-02 | Each brand appears once<br>*Medium, Data integrity* | `GET /api/brandsList` | Brand names are unique (the list is used to build the brand filter). | Fail | 34 entries but only 8 distinct brand names; the list repeats a brand once per product. **AE-API-003** |
| API-BRD-03 | PUT to brands list is rejected<br>*Medium, Negative, method* | `PUT /api/brandsList` | HTTP 405. | Fail | HTTP 200 with responseCode 405 in the body. **AE-API-001** |

## Search

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-SRC-01 | Search by keyword<br>*High, Positive* | `POST /api/searchProduct  search_product=top` | HTTP 200. Only products related to "top". | Pass | 14 products. Includes two shirts ("Little Girls Mr. Panda Shirt", "Colour Blocked Shirt") which match on category "Tops & Shirts" rather than name (Q-01). |
| API-SRC-02 | Search is case-insensitive<br>*Low, Positive* | `POST /api/searchProduct  search_product=TOP` | Same results as lower case. | Pass | 14 products, same as API-SRC-01. |
| API-SRC-03 | Search with no match<br>*Medium, Negative* | `POST /api/searchProduct  search_product=zzzzqqq` | HTTP 200 with an empty products array. | Pass | 0 products, no error. |
| API-SRC-04 | Search with an empty value<br>*Medium, Boundary* | `POST /api/searchProduct  search_product=` | Behaviour is not documented. Either 400 (like a missing parameter) or an empty result. | Query | Returns all 34 products (Q-02). |
| API-SRC-05 | Missing search parameter<br>*High, Negative* | `POST /api/searchProduct  (no body)` | HTTP 400 "Bad request, search_product parameter is missing in POST request." | Fail | Correct message, but HTTP 200 with responseCode 400 in the body. **AE-API-001** |
| API-SRC-06 | SQL meta-characters are handled safely<br>*High, Security* | `POST /api/searchProduct  search_product=' OR 1=1 --` | No server error, no extra data returned. | Pass | HTTP 200, 0 products. |
| API-SRC-07 | Script tags are not reflected<br>*Medium, Security* | `POST /api/searchProduct  search_product=<script>alert(1)</script>` | No error; input is not echoed back unescaped. | Pass | HTTP 200, 0 products, input not reflected. |
| API-SRC-08 | GET is not allowed on search<br>*Low, Negative, method* | `GET /api/searchProduct?search_product=top` | HTTP 405. | Fail | HTTP 200 with responseCode 405 in the body. **AE-API-001** |

## Login verification

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-LGN-01 | Verify login with valid credentials<br>*High, Positive* | `POST /api/verifyLogin  email, password` | HTTP 200 "User exists!" | Pass | HTTP 200, responseCode 200, "User exists!" |
| API-LGN-02 | Missing email parameter<br>*High, Negative* | `POST /api/verifyLogin  password only` | HTTP 400 "Bad request, email or password parameter is missing in POST request." | Fail | Correct message, HTTP 200 with responseCode 400 in the body. **AE-API-001** |
| API-LGN-03 | Missing password parameter<br>*Medium, Negative* | `POST /api/verifyLogin  email only` | HTTP 400 with the same message. | Fail | HTTP 200, responseCode 400, "Bad request, email or password parameter is missing in POST request." Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |
| API-LGN-04 | DELETE is not allowed on verifyLogin<br>*Medium, Negative, method* | `DELETE /api/verifyLogin` | HTTP 405. | Fail | HTTP 200 with responseCode 405 in the body. **AE-API-001** |
| API-LGN-05 | Unknown user<br>*High, Negative* | `POST /api/verifyLogin  unregistered email` | HTTP 404 "User not found!" | Fail | HTTP 200, responseCode 404, "User not found!" Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |
| API-LGN-06 | Wrong password for an existing user<br>*High, Negative, security* | `POST /api/verifyLogin  valid email, wrong password` | Same response as an unknown user, so valid emails cannot be discovered. | Pass | Wrong password and unknown user get the identical response (responseCode 404, "User not found!"). |

## User accounts

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-ACC-01 | Create an account with all fields<br>*High, Positive* | `POST /api/createAccount  name, email, password, title, birth_date/month/year, firstname, lastname, company, address1/2, country, zipcode, state, city, mobile_number` | HTTP 201 "User created!" | Fail | Account created: responseCode 201 "User created!", but HTTP 200 (AE-API-001). **AE-API-001** |
| API-ACC-02 | Duplicate email is rejected<br>*High, Negative* | `POST /api/createAccount  same email as API-ACC-01` | 4xx with a clear "already exists" message; no second account. | Fail | HTTP 200, responseCode 400, "Email already exists!"; no second account. Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |
| API-ACC-03 | Missing mandatory field<br>*High, Negative* | `POST /api/createAccount  without email (then without password, name...)` | HTTP 400 naming the missing parameter. | Fail | HTTP 200, responseCode 400, "Bad request, email parameter is missing in POST request." Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |
| API-ACC-04 | Invalid email format<br>*Medium, Validation* | `POST /api/createAccount  email=not-an-email` | HTTP 400; account not created. | Fail | Account created with email "not-an-email-<timestamp>": responseCode 201 "User created!". No format validation. (Account deleted straight after.) **AE-API-006** |
| API-ACC-05 | Get account details by email<br>*High, Positive, security* | `GET /api/getUserDetailByEmail?email=<email>` | HTTP 200 with the details sent at creation. The password must not be returned. | Pass | HTTP 200; 16 fields returned, all match the values sent; no password field. |
| API-ACC-06 | Account details: missing email<br>*Medium, Negative* | `GET /api/getUserDetailByEmail` | HTTP 400 "Bad request, email parameter is missing in GET request." | Fail | Correct message, HTTP 200 with responseCode 400 in the body. **AE-API-001** |
| API-ACC-07 | Account details: unknown email<br>*Medium, Negative* | `GET /api/getUserDetailByEmail?email=<unregistered>` | HTTP 404. | Fail | HTTP 200, responseCode 404, "Account not found with this email, try another email!" Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |
| API-ACC-08 | Update an account<br>*High, Positive* | `PUT /api/updateAccount  all fields, city changed` | HTTP 200 "User updated!"; API-ACC-05 then returns the new city. | Pass | HTTP 200, responseCode 200, "User updated!"; city now "Ottawa". |
| API-ACC-09 | Update with a wrong password is refused<br>*High, Negative, security* | `PUT /api/updateAccount  wrong password` | Rejected; account unchanged. | Pass | Rejected with responseCode 404 "Account not found!"; city unchanged ("Ottawa"). Message is misleading since the account exists (Q-04). |
| API-ACC-10 | Delete with a wrong password is refused<br>*High, Negative, security* | `DELETE /api/deleteAccount  wrong password` | Rejected; account still exists. | Pass | Rejected with responseCode 404 "Account not found!"; account still exists (Q-04). |
| API-ACC-11 | Delete an account<br>*High, Positive* | `DELETE /api/deleteAccount  email, password` | HTTP 200 "Account deleted!"; verifyLogin then reports "User not found!". | Pass | HTTP 200 "Account deleted!"; verifyLogin then returns "User not found!". |
| API-ACC-12 | Delete an account that no longer exists<br>*Low, Negative* | `DELETE /api/deleteAccount  same credentials again` | HTTP 404. | Fail | HTTP 200, responseCode 404, "Account not found!" Functionally correct, but HTTP 200 (AE-API-001). **AE-API-001** |

## Cross-cutting (protocol, headers, performance)

| ID | Title | Request | Expected | Status | Actual / notes |
|---|---|---|---|---|---|
| API-GEN-01 | JSON responses declare a JSON content type<br>*Medium, Headers* | `Any successful GET, e.g. /api/productsList` | Content-Type: application/json | Fail | Content-Type: text/html; charset=utf-8 on every endpoint tested. **AE-API-002** |
| API-GEN-02 | Allow header matches supported methods<br>*Low, Headers* | `HEAD /api/productsList` | Allow lists only methods the endpoint really supports (GET). | Fail | HEAD gets a real 405, but Allow says "GET, OPTIONS, PUT, POST, DELETE" while PUT/POST/DELETE answer "not supported". **AE-API-004** |
| API-GEN-03 | API is only served over HTTPS<br>*High, Security* | `GET http://automationexercise.com/api/productsList` | Redirect (301/308) to https://, and a Strict-Transport-Security header on HTTPS responses. | Fail | Plain HTTP answers 200 with data, no redirect; no HSTS header. **AE-API-005** |
| API-GEN-04 | Response time for read endpoints<br>*Medium, Performance (indicative)* | `GET productsList, GET brandsList, POST searchProduct` | Under 2 s per call from a home broadband connection. | Pass | First call 1.7 s (cold), then 0.4 to 0.6 s. |


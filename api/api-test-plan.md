# API Test Plan – Automation Exercise REST API

| | |
|---|---|
| Base URL | https://automationexercise.com/api |
| Reference | https://automationexercise.com/api_list (14 documented endpoints) |
| Author | Afzal Ahmed |
| Version | 1.0 – 7 Nov 2026 |
| Related | [Test strategy](../test-strategy.md), [Test cases](test-cases.md), [CSV](test-cases.csv) |

## 1. Endpoints in scope

| # | Method | Endpoint | Purpose | Documented response |
|---|---|---|---|---|
| 1 | GET | /productsList | All products | 200, products list |
| 2 | POST | /productsList | Unsupported method | 405 |
| 3 | GET | /brandsList | All brands | 200, brands list |
| 4 | PUT | /brandsList | Unsupported method | 405 |
| 5 | POST | /searchProduct | Search by `search_product` | 200, matching products |
| 6 | POST | /searchProduct (no parameter) | Missing parameter | 400 |
| 7 | POST | /verifyLogin | Check `email` + `password` | 200 "User exists!" |
| 8 | POST | /verifyLogin (no email) | Missing parameter | 400 |
| 9 | DELETE | /verifyLogin | Unsupported method | 405 |
| 10 | POST | /verifyLogin (invalid) | Unknown user | 404 "User not found!" |
| 11 | POST | /createAccount | Register user (17 fields) | 201 "User created!" |
| 12 | DELETE | /deleteAccount | Delete user | 200 "Account deleted!" |
| 13 | PUT | /updateAccount | Update user | 200 "User updated!" |
| 14 | GET | /getUserDetailByEmail | User by email | 200, user detail |

All requests send parameters as form data (`application/x-www-form-urlencoded`). No
authentication token is used; account endpoints rely on email + password in the request.

## 2. What is checked for every endpoint

1. **Status code.** The HTTP status matches the documentation, not just a code inside the body.
2. **Body.** `responseCode` and `message` match the documentation.
3. **Schema.** Required fields present with the right types; no unexpected nulls.
4. **Headers.** `Content-Type: application/json`; `Allow` consistent with supported methods.
5. **Negative paths.** Wrong method, missing parameter, empty value, invalid value.
6. **Security basics.** HTTPS only, injection characters handled, no password in responses,
   no difference between "unknown user" and "wrong password".
7. **Response time.** Under 2 s for read endpoints (indicative only, no load generation).

## 3. Test design

**Method matrix:** each endpoint is called with its documented method plus at least one
unsupported method, so the 405 behaviour is checked across the API rather than only where the
docs mention it.

**Search input classes**

| Class | Example | Expected |
|---|---|---|
| Matching keyword | `top` | Matching products |
| Different case | `TOP` | Same as lower case |
| No match | `zzzzqqq` | Empty list |
| Empty string | `` | Not documented; raised as a question |
| Missing parameter | – | 400 |
| Injection characters | `' OR 1=1 --`, `<script>` | Handled safely, not reflected |

**Account lifecycle:** create → verify login → get details → update → verify update →
delete with wrong password (must fail) → delete → verify login fails → delete again (404).
Each run uses a new email so runs never depend on each other.

## 4. Test data

| Field | Value |
|---|---|
| email | `qa.portfolio+<timestamp>@example.com` |
| password | `Secret#123` |
| title / name | Mr, "QA Portfolio" |
| birth date | 10 / May / 1995 |
| address | 1 Main St, Toronto, Ontario, A1B2C3, Canada |
| mobile_number | 5550100 |

## 5. Tools

curl or Postman for manual execution. Each case in [test-cases.md](test-cases.md) gives the
exact method, endpoint and parameters, so it can be pasted straight into either.

```bash
# API-SRC-05: missing search parameter
curl -i -X POST https://automationexercise.com/api/searchProduct
```

## 6. Execution status

All 38 cases were run on 7 Nov 2026, in two cycles: read-only calls first, then the account
lifecycle with a throwaway `@example.com` account that was deleted at the end. Results are in
[test-cases.md](test-cases.md) and summarised in the [execution report](../reports/test-execution-report.md).

# UI Test Cases – Automation Exercise

Web shop at https://automationexercise.com. Scope and approach are in the [UI test plan](ui-test-plan.md).
The same cases are in [test-cases.csv](test-cases.csv) for import into TestRail, Xray or Excel.
Results are from the run on 2026-11-07, see the [execution report](../reports/test-execution-report.md).

**50 cases** · Pass 45 · Fail 4 · Blocked 1

| Module | Cases | High | Medium | Low | Pass | Fail | Blocked |
|---|---|---|---|---|---|---|---|
| Registration, login and account | 10 | 7 | 2 | 1 | 10 | 0 | 0 |
| Product catalogue and search | 10 | 3 | 4 | 3 | 10 | 0 | 0 |
| Shopping cart | 6 | 4 | 1 | 1 | 5 | 1 | 0 |
| Checkout, payment and invoice | 8 | 5 | 3 | 0 | 7 | 1 | 0 |
| Contact us, subscription and navigation | 7 | 0 | 3 | 4 | 7 | 0 | 0 |
| Non-functional (compatibility, accessibility, performance) | 6 | 1 | 3 | 2 | 3 | 2 | 1 |
| End-to-end customer journeys | 3 | 2 | 1 | 0 | 3 | 0 | 0 |
| **Total** | **50** | **22** | **17** | **11** | **45** | **4** | **1** |

## Registration, login and account

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-AUTH-01](#ui-auth-01) | Register a new customer with all mandatory details | High | Functional, positive | Pass |
| [UI-AUTH-02](#ui-auth-02) | Signup is refused for an email that is already registered | High | Functional, negative | Pass |
| [UI-AUTH-03](#ui-auth-03) | Signup email must be a valid address | Medium | Validation, negative | Pass |
| [UI-AUTH-04](#ui-auth-04) | Account information form enforces mandatory fields | High | Validation, negative | Pass |
| [UI-AUTH-05](#ui-auth-05) | Log in with a valid email and password | High | Functional, positive | Pass |
| [UI-AUTH-06](#ui-auth-06) | Login is refused with a wrong password | High | Functional, negative | Pass |
| [UI-AUTH-07](#ui-auth-07) | Unknown email gets the same message as a wrong password | Medium | Security, negative | Pass |
| [UI-AUTH-08](#ui-auth-08) | Logout ends the session | High | Functional | Pass |
| [UI-AUTH-09](#ui-auth-09) | Delete account removes access | High | Functional | Pass |
| [UI-AUTH-10](#ui-auth-10) | Password is masked and never appears in the URL | Low | Security | Pass |

### UI-AUTH-01

**Register a new customer with all mandatory details**

| | |
|---|---|
| Priority | High |
| Type | Functional, positive |
| Requirement | REQ-01 |
| Preconditions | Email address not registered before. |
| Test data | Name: Priya Shah; Email: unique, e.g. priya.shah+<timestamp>@example.com; Password: Secret#123; Country: Canada |

1. Open the home page and click "Signup / Login".
2. Under "New User Signup!" enter name and email, click "Signup".
3. Check "ENTER ACCOUNT INFORMATION" is shown with name and email pre-filled.
4. Fill title, password, date of birth, first/last name, address, country, state, city, zipcode and mobile number.
5. Click "Create Account".
6. Click "Continue".

**Expected:** "ACCOUNT CREATED!" is shown. After Continue the header reads "Logged in as Priya Shah".

**Result: Pass**. "ACCOUNT CREATED!" shown; header "Logged in as Priya Shah".

### UI-AUTH-02

**Signup is refused for an email that is already registered**

| | |
|---|---|
| Priority | High |
| Type | Functional, negative |
| Requirement | REQ-01 |
| Preconditions | An account exists for the email used. |
| Test data | Existing email |

1. Go to "Signup / Login".
2. Enter any name and the existing email under "New User Signup!".
3. Click "Signup".

**Expected:** Stays on the page with the message "Email Address already exist!". No second account is created.

**Result: Pass**. "Email Address already exist!" shown, stayed on signup.

### UI-AUTH-03

**Signup email must be a valid address**

| | |
|---|---|
| Priority | Medium |
| Type | Validation, negative |
| Requirement | REQ-01 |
| Preconditions | - |
| Test data | priya.shah, priya@, @example.com |

1. Go to "Signup / Login".
2. Enter a name and each invalid email in turn.
3. Click "Signup".

**Expected:** Form is not submitted; the browser flags the email field as invalid.

**Result: Pass**. All three invalid emails blocked by browser validation.

### UI-AUTH-04

**Account information form enforces mandatory fields**

| | |
|---|---|
| Priority | High |
| Type | Validation, negative |
| Requirement | REQ-01 |
| Preconditions | On "ENTER ACCOUNT INFORMATION" after step 2 of UI-AUTH-01. |
| Test data | Leave password, first name, last name, address, state, city, zipcode, mobile empty one at a time |

1. Leave one mandatory field empty and fill the rest.
2. Click "Create Account".
3. Repeat for each mandatory field.

**Expected:** Account is not created while any mandatory field is empty; the empty field is highlighted.

**Result: Pass**. Not created with any mandatory field empty (8/8 flagged by validation).

### UI-AUTH-05

**Log in with a valid email and password**

| | |
|---|---|
| Priority | High |
| Type | Functional, positive |
| Requirement | REQ-02 |
| Preconditions | Registered account. |
| Test data | Valid credentials |

1. Go to "Signup / Login".
2. Enter email and password under "Login to your account".
3. Click "Login".

**Expected:** Home page opens and the header shows "Logged in as <name>", plus "Logout" and "Delete Account" links.

**Result: Pass**. Logged in; Logout and Delete Account links shown.

### UI-AUTH-06

**Login is refused with a wrong password**

| | |
|---|---|
| Priority | High |
| Type | Functional, negative |
| Requirement | REQ-02 |
| Preconditions | Registered account. |
| Test data | Valid email, wrong password |

1. Enter the email and a wrong password.
2. Click "Login".

**Expected:** "Your email or password is incorrect!" is shown and the user stays logged out.

**Result: Pass**. "Your email or password is incorrect!" shown, user stays logged out. (First attempt hit a page-load timeout on the shared site; re-run passed.)

### UI-AUTH-07

**Unknown email gets the same message as a wrong password**

| | |
|---|---|
| Priority | Medium |
| Type | Security, negative |
| Requirement | REQ-02 |
| Preconditions | - |
| Test data | Unregistered email |

1. Enter an email that has never been registered and any password.
2. Click "Login".

**Expected:** Same generic message as UI-AUTH-06, so the page does not reveal which emails are registered.

**Result: Pass**. Same generic message as for a wrong password.

### UI-AUTH-08

**Logout ends the session**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-02 |
| Preconditions | Logged in. |
| Test data | - |

1. Click "Logout".
2. Press the browser Back button.
3. Reload the page.

**Expected:** Login page opens. Going back and reloading does not show "Logged in as".

**Result: Pass**. Redirected to login; Back + reload did not restore the session.

### UI-AUTH-09

**Delete account removes access**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-03 |
| Preconditions | Logged in with a throwaway account. |
| Test data | - |

1. Click "Delete Account".
2. Click "Continue".
3. Try to log in with the same credentials.

**Expected:** "ACCOUNT DELETED!" is shown; logging in again fails with the incorrect-credentials message.

**Result: Pass**. "ACCOUNT DELETED!" shown; logging in again refused.

### UI-AUTH-10

**Password is masked and never appears in the URL**

| | |
|---|---|
| Priority | Low |
| Type | Security |
| Requirement | REQ-02 |
| Preconditions | - |
| Test data | - |

1. Type a password in the login form.
2. Submit and look at the address bar and browser history.

**Expected:** Characters are masked while typing; the password is not part of any URL.

**Result: Pass**. Field type=password; password not in URL after submit.

## Product catalogue and search

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-PROD-01](#ui-prod-01) | All Products page lists the catalogue | High | Functional | Pass |
| [UI-PROD-02](#ui-prod-02) | Product detail page shows full product information | High | Functional | Pass |
| [UI-PROD-03](#ui-prod-03) | Search returns matching products only | High | Functional, positive | Pass |
| [UI-PROD-04](#ui-prod-04) | Search with no matches shows an empty result | Medium | Functional, negative | Pass |
| [UI-PROD-05](#ui-prod-05) | Search ignores letter case | Low | Functional | Pass |
| [UI-PROD-06](#ui-prod-06) | Filter by category | Medium | Functional | Pass |
| [UI-PROD-07](#ui-prod-07) | Filter by brand and check the product count | Medium | Functional | Pass |
| [UI-PROD-08](#ui-prod-08) | Write a product review | Medium | Functional | Pass |
| [UI-PROD-09](#ui-prod-09) | Review cannot be submitted with empty fields | Low | Validation, negative | Pass |
| [UI-PROD-10](#ui-prod-10) | Add to cart from "Recommended Items" | Low | Functional | Pass |

### UI-PROD-01

**All Products page lists the catalogue**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-04 |
| Preconditions | - |
| Test data | - |

1. Click "Products" in the header.

**Expected:** "ALL PRODUCTS" heading with product cards, each showing image, name, price, "Add to cart" and "View Product".

**Result: Pass**. "ALL PRODUCTS" with 34 product cards; image, name, price, Add to cart, View Product present.

### UI-PROD-02

**Product detail page shows full product information**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-04 |
| Preconditions | - |
| Test data | First product (Blue Top) |

1. On All Products click "View Product" for the first item.

**Expected:** Detail page shows name, category, price, availability, condition and brand.

**Result: Pass**. Name, category, price, availability, condition and brand shown.

### UI-PROD-03

**Search returns matching products only**

| | |
|---|---|
| Priority | High |
| Type | Functional, positive |
| Requirement | REQ-05 |
| Preconditions | - |
| Test data | jean |

1. On All Products type the keyword in "Search Product".
2. Click the search button.

**Expected:** "SEARCHED PRODUCTS" heading; every result contains the keyword in its name or category.

**Result: Pass**. "SEARCHED PRODUCTS" with 3 results: Soft Stretch Jeans, Regular Fit Straight Jeans, Grunt Blue Slim Fit Jeans.

### UI-PROD-04

**Search with no matches shows an empty result**

| | |
|---|---|
| Priority | Medium |
| Type | Functional, negative |
| Requirement | REQ-05 |
| Preconditions | - |
| Test data | zzqqxx |

1. Search for a keyword that matches nothing.

**Expected:** No product cards and no error page. (A helpful "no results" message is expected; record what is actually shown.)

**Result: Pass**. No product cards and no error. Only the "SEARCHED PRODUCTS" heading is shown, with no "no results" message (usability note).

### UI-PROD-05

**Search ignores letter case**

| | |
|---|---|
| Priority | Low |
| Type | Functional |
| Requirement | REQ-05 |
| Preconditions | - |
| Test data | top, TOP, ToP |

1. Search for each variant.

**Expected:** Same results for every variant.

**Result: Pass**. Same 14 results for top / TOP / ToP.

### UI-PROD-06

**Filter by category**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-06 |
| Preconditions | - |
| Test data | Women → Dress, Men → Tshirts |

1. In the left sidebar expand "Women" and click "Dress".
2. Then expand "Men" and click "Tshirts".

**Expected:** Headings "WOMEN - DRESS PRODUCTS" and "MEN - TSHIRTS PRODUCTS"; only products from that category are listed.

**Result: Pass**. "WOMEN - DRESS PRODUCTS" and "MEN - TSHIRTS PRODUCTS" shown.

### UI-PROD-07

**Filter by brand and check the product count**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-06 |
| Preconditions | - |
| Test data | Polo, then H&M |

1. Click a brand under "Brands".
2. Count the products shown and compare with the number next to the brand.

**Expected:** "BRAND - POLO PRODUCTS" heading; number of cards equals the count in brackets in the sidebar.

**Result: Pass**. Polo: sidebar 6, shown 6; H&M: sidebar 5, shown 5.

### UI-PROD-08

**Write a product review**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-07 |
| Preconditions | On a product detail page. |
| Test data | Name, email, review text |

1. Fill "Your Name", "Email Address" and "Add Review Here!".
2. Click "Submit".

**Expected:** "Thank you for your review." is shown and the form is cleared.

**Result: Pass**. "Thank you for your review." shown.

### UI-PROD-09

**Review cannot be submitted with empty fields**

| | |
|---|---|
| Priority | Low |
| Type | Validation, negative |
| Requirement | REQ-07 |
| Preconditions | On a product detail page. |
| Test data | Empty name / email / review |

1. Leave one field empty at a time and click "Submit".

**Expected:** Review is not submitted; the empty field is flagged.

**Result: Pass**. Not submitted with any field empty (3/3 flagged).

### UI-PROD-10

**Add to cart from "Recommended Items"**

| | |
|---|---|
| Priority | Low |
| Type | Functional |
| Requirement | REQ-08 |
| Preconditions | - |
| Test data | - |

1. Scroll to "RECOMMENDED ITEMS" at the bottom of the home page.
2. Click "Add to cart" on a product.
3. Click "View Cart".

**Expected:** The recommended product is in the cart.

**Result: Pass**. "Stylish Dress" from Recommended Items is in the cart.

## Shopping cart

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-CART-01](#ui-cart-01) | Add two products and check the cart lines | High | Functional | Pass |
| [UI-CART-02](#ui-cart-02) | Quantity chosen on the detail page is kept | High | Functional | Pass |
| [UI-CART-03](#ui-cart-03) | Quantity field boundaries on the detail page | Medium | Boundary | Fail (AE-UI-003) |
| [UI-CART-04](#ui-cart-04) | Remove a product from the cart | High | Functional | Pass |
| [UI-CART-05](#ui-cart-05) | Empty cart message | Low | Functional | Pass |
| [UI-CART-06](#ui-cart-06) | Guest cart is kept after logging in | High | Functional | Pass |

### UI-CART-01

**Add two products and check the cart lines**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-08 |
| Preconditions | Empty cart. |
| Test data | Blue Top (Rs. 500), Men Tshirt (Rs. 400) |

1. On All Products hover the first product and click "Add to cart", then "Continue Shopping".
2. Add the second product and click "View Cart".

**Expected:** Both products listed with correct name, price, quantity 1 and line total.

**Result: Pass**. Blue Top Rs. 500 ×1; Men Tshirt Rs. 400 ×1.

### UI-CART-02

**Quantity chosen on the detail page is kept**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-08 |
| Preconditions | Empty cart. |
| Test data | Quantity 4 |

1. Open a product detail page.
2. Set quantity to 4 and click "Add to cart".
3. Click "View Cart".

**Expected:** Cart shows quantity 4 and total = price × 4.

**Result: Pass**. Quantity 4, total Rs. 2000.

### UI-CART-03

**Quantity field boundaries on the detail page**

| | |
|---|---|
| Priority | Medium |
| Type | Boundary |
| Requirement | REQ-08 |
| Preconditions | On a product detail page. |
| Test data | 0, -1, 1, 999, 1000000, "abc" |

1. Enter each value in Quantity and click "Add to cart".
2. Check the cart after each attempt.

**Expected:** 1 and 999 are accepted. 0, negative and non-numeric values are rejected or corrected to 1. Very large values are capped or refused with a message. Record actual behaviour; anything accepted below 1 is a defect.

**Result: Fail** · AE-UI-003. Quantity 1 and 999 accepted correctly. 0 added a line with total Rs. 0; -1 added a line with total Rs. -500; 1000000 accepted (total Rs. 500,000,000); "abc" leaves the field empty and adds nothing.

### UI-CART-04

**Remove a product from the cart**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-09 |
| Preconditions | Cart has two products. |
| Test data | - |

1. Open the cart.
2. Click the "X" next to one product.

**Expected:** That line disappears without a page reload; the other product remains.

**Result: Pass**. Line removed without reload; other product remains.

### UI-CART-05

**Empty cart message**

| | |
|---|---|
| Priority | Low |
| Type | Functional |
| Requirement | REQ-09 |
| Preconditions | Cart has one product. |
| Test data | - |

1. Remove the last product.

**Expected:** "Cart is empty! Click here to buy products." with a link back to Products.

**Result: Pass**. "Cart is empty! Click here to buy products." with link to Products.

### UI-CART-06

**Guest cart is kept after logging in**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-10 |
| Preconditions | Logged out; registered account available. |
| Test data | - |

1. As a guest search "top" and add every result to the cart.
2. Log in.
3. Open the cart.

**Expected:** All products added as a guest are still in the cart.

**Result: Pass**. 14 products added as guest; all 14 still in cart after login.

## Checkout, payment and invoice

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-CHK-01](#ui-chk-01) | Guest is asked to register or log in at checkout | High | Functional | Pass |
| [UI-CHK-02](#ui-chk-02) | Delivery and billing address match the account details | High | Functional | Pass |
| [UI-CHK-03](#ui-chk-03) | Review order totals before paying | High | Functional | Pass |
| [UI-CHK-04](#ui-chk-04) | Pay with complete card details | High | Functional, positive | Pass |
| [UI-CHK-05](#ui-chk-05) | Payment form enforces mandatory fields | High | Validation, negative | Pass |
| [UI-CHK-06](#ui-chk-06) | Payment rejects malformed or expired cards | Medium | Validation, negative | Fail (AE-UI-001) |
| [UI-CHK-07](#ui-chk-07) | Download the invoice after ordering | Medium | Functional | Pass |
| [UI-CHK-08](#ui-chk-08) | Cart is emptied after a successful order | Medium | Functional | Pass |

### UI-CHK-01

**Guest is asked to register or log in at checkout**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-11 |
| Preconditions | Logged out, cart not empty. |
| Test data | - |

1. Open the cart and click "Proceed To Checkout".

**Expected:** A "Checkout" modal says "Register / Login account to proceed on checkout." with a link to the login page.

**Result: Pass**. Checkout modal with "Register / Login account to proceed on checkout." and login link.

### UI-CHK-02

**Delivery and billing address match the account details**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-11 |
| Preconditions | Logged in, cart not empty. |
| Test data | Account created in UI-AUTH-01 |

1. Proceed to checkout.
2. Compare "Your delivery address" and "Your billing address" with the details entered at signup.

**Expected:** Both addresses show the same title, name, company, address lines, city, state, zipcode, country and phone.

**Result: Pass**. Delivery and billing address both match the signup details.

### UI-CHK-03

**Review order totals before paying**

| | |
|---|---|
| Priority | High |
| Type | Functional |
| Requirement | REQ-11 |
| Preconditions | Logged in, two products in cart. |
| Test data | - |

1. Proceed to checkout.
2. Check "Review Your Order".

**Expected:** Same products, quantities and totals as the cart; the total amount is the sum of the lines.

**Result: Pass**. Review lists 2 products; lines sum to Rs. 900, matching the total.

### UI-CHK-04

**Pay with complete card details**

| | |
|---|---|
| Priority | High |
| Type | Functional, positive |
| Requirement | REQ-12 |
| Preconditions | On the checkout page. |
| Test data | Name on card, card number 4111 1111 1111 1111, CVC 123, expiry 12/2030 |

1. Add a comment, click "Place Order".
2. Fill all payment fields.
3. Click "Pay and Confirm Order".

**Expected:** "ORDER PLACED!" and "Congratulations! Your order has been confirmed!" are shown.

**Result: Pass**. "ORDER PLACED!" and "Congratulations! Your order has been confirmed!" shown.

### UI-CHK-05

**Payment form enforces mandatory fields**

| | |
|---|---|
| Priority | High |
| Type | Validation, negative |
| Requirement | REQ-12 |
| Preconditions | On the payment page. |
| Test data | Each field empty in turn |

1. Leave one payment field empty and click "Pay and Confirm Order".

**Expected:** Order is not placed while any field is empty.

**Result: Pass**. Order not placed while any of the 5 payment fields was empty.

### UI-CHK-06

**Payment rejects malformed or expired cards**

| | |
|---|---|
| Priority | Medium |
| Type | Validation, negative |
| Requirement | REQ-12 |
| Preconditions | On the payment page. |
| Test data | Card number "abcd", CVC "1", expiry 01/2020 |

1. Enter each invalid value with the other fields valid.
2. Click "Pay and Confirm Order".

**Expected:** Order is refused with a clear message. On a demo site this may be accepted; if so, raise it as a risk for any real payment integration.

**Result: Fail** · AE-UI-001. Order placed with card number "abcd", with CVC "1" and with expiry 01/2020. No client-side card validation. Raised as a risk for a real payment integration.

### UI-CHK-07

**Download the invoice after ordering**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-13 |
| Preconditions | Order just placed (UI-CHK-04). |
| Test data | - |

1. Click "Download Invoice".
2. Open the downloaded file.

**Expected:** A file is downloaded and states the customer name and the total amount of the order.

**Result: Pass**. Downloaded invoice.txt: "Hi Priya Shah, Your total purchase amount is 500. Thank you"

### UI-CHK-08

**Cart is emptied after a successful order**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-12 |
| Preconditions | Order just placed. |
| Test data | - |

1. Click "Continue", then open the cart.

**Expected:** Cart is empty.

**Result: Pass**. Cart empty after the order.

## Contact us, subscription and navigation

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-MISC-01](#ui-misc-01) | Contact form with a file attachment | Medium | Functional | Pass |
| [UI-MISC-02](#ui-misc-02) | Contact form requires a valid email | Low | Validation, negative | Pass |
| [UI-MISC-03](#ui-misc-03) | Subscribe from the home page footer | Medium | Functional | Pass |
| [UI-MISC-04](#ui-misc-04) | Subscribe from the cart page footer | Low | Functional | Pass |
| [UI-MISC-05](#ui-misc-05) | Subscription rejects an invalid email | Low | Validation, negative | Pass |
| [UI-MISC-06](#ui-misc-06) | Header navigation reaches every main page | Medium | Navigation | Pass |
| [UI-MISC-07](#ui-misc-07) | Scroll-up arrow returns to the top | Low | UI | Pass |

### UI-MISC-01

**Contact form with a file attachment**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-14 |
| Preconditions | - |
| Test data | Name, email, subject, message, small .txt file |

1. Click "Contact us".
2. Fill every field and choose a file.
3. Click "Submit" and accept the browser confirmation.

**Expected:** "Success! Your details have been submitted successfully." "Home" returns to the home page.

**Result: Pass**. Success message shown; Home returned to the home page.

### UI-MISC-02

**Contact form requires a valid email**

| | |
|---|---|
| Priority | Low |
| Type | Validation, negative |
| Requirement | REQ-14 |
| Preconditions | - |
| Test data | Empty email, "not-an-email" |

1. Fill the form without a valid email and submit.

**Expected:** Form is not submitted; email field flagged.

**Result: Pass**. Not submitted with empty or invalid email.

### UI-MISC-03

**Subscribe from the home page footer**

| | |
|---|---|
| Priority | Medium |
| Type | Functional |
| Requirement | REQ-15 |
| Preconditions | - |
| Test data | Valid email |

1. Scroll to the footer, enter an email under "SUBSCRIPTION", click the arrow.

**Expected:** "You have been successfully subscribed!" is shown.

**Result: Pass**. "You have been successfully subscribed!" shown.

### UI-MISC-04

**Subscribe from the cart page footer**

| | |
|---|---|
| Priority | Low |
| Type | Functional |
| Requirement | REQ-15 |
| Preconditions | - |
| Test data | Valid email |

1. Open the cart, subscribe from the footer.

**Expected:** Same success message as UI-MISC-03.

**Result: Pass**. Same success message from the cart page.

### UI-MISC-05

**Subscription rejects an invalid email**

| | |
|---|---|
| Priority | Low |
| Type | Validation, negative |
| Requirement | REQ-15 |
| Preconditions | - |
| Test data | priya@ |

1. Enter the invalid email and click the arrow.

**Expected:** No success message; the field is flagged.

**Result: Pass**. No success message; field flagged invalid.

### UI-MISC-06

**Header navigation reaches every main page**

| | |
|---|---|
| Priority | Medium |
| Type | Navigation |
| Requirement | REQ-16 |
| Preconditions | - |
| Test data | - |

1. Click each header link: Home, Products, Cart, Signup / Login, Test Cases, API Testing, Contact us.

**Expected:** Each link opens the right page with its heading; no broken links.

**Result: Pass**. All 7 header links open the right page.

### UI-MISC-07

**Scroll-up arrow returns to the top**

| | |
|---|---|
| Priority | Low |
| Type | UI |
| Requirement | REQ-16 |
| Preconditions | - |
| Test data | - |

1. Scroll to the bottom of the home page.
2. Click the arrow in the bottom-right corner.

**Expected:** Page scrolls to the top and the banner text "Full-Fledged practice website for Automation Engineers" is visible.

**Result: Pass**. Scrolled back to the top; banner text visible.

## Non-functional (compatibility, accessibility, performance)

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-NFR-01](#ui-nfr-01) | Cross-browser smoke test | High | Compatibility | Blocked |
| [UI-NFR-02](#ui-nfr-02) | Usable on a mobile screen | Medium | Responsive | Fail (AE-UI-004) |
| [UI-NFR-03](#ui-nfr-03) | Login and signup with the keyboard only | Medium | Accessibility | Pass |
| [UI-NFR-04](#ui-nfr-04) | Automated accessibility scan | Low | Accessibility | Fail (AE-UI-002) |
| [UI-NFR-05](#ui-nfr-05) | Page load time | Low | Performance (indicative) | Pass |
| [UI-NFR-06](#ui-nfr-06) | Site is served over HTTPS | Medium | Security | Pass |

### UI-NFR-01

**Cross-browser smoke test**

| | |
|---|---|
| Priority | High |
| Type | Compatibility |
| Requirement | NFR-01 |
| Preconditions | Latest Chrome, Firefox, Edge and Safari. |
| Test data | - |

1. Run UI-AUTH-05, UI-PROD-03, UI-CART-01 and UI-CHK-04 in each browser.

**Expected:** Same behaviour and no layout breakage in every browser.

**Result: Blocked**. Only Chromium is available on this machine; Firefox, Edge and Safari runs not possible in this cycle.

### UI-NFR-02

**Usable on a mobile screen**

| | |
|---|---|
| Priority | Medium |
| Type | Responsive |
| Requirement | NFR-02 |
| Preconditions | DevTools device mode, 375 × 812 (iPhone X class). |
| Test data | - |

1. Open home, products, cart and checkout pages.

**Expected:** Navigation collapses into a menu; no horizontal scrolling; buttons are large enough to tap.

**Result: Fail** · AE-UI-004. No horizontal scrolling at 375 × 812, but the header never collapses: 8 menu links wrap over three rows above the content. Usable, not the expected collapsed menu.

### UI-NFR-03

**Login and signup with the keyboard only**

| | |
|---|---|
| Priority | Medium |
| Type | Accessibility |
| Requirement | NFR-03 |
| Preconditions | - |
| Test data | - |

1. Use Tab, Shift+Tab and Enter only to fill and submit the login form.

**Expected:** Every field and button is reachable in a logical order with a visible focus indicator.

**Result: Pass**. Tab order email → password → Login; Enter logs in. Focus is shown by the button changing from orange to grey (no outline).

### UI-NFR-04

**Automated accessibility scan**

| | |
|---|---|
| Priority | Low |
| Type | Accessibility |
| Requirement | NFR-03 |
| Preconditions | axe DevTools or Lighthouse. |
| Test data | Home, product detail, login, checkout |

1. Run the scan on each page.

**Expected:** No critical WCAG 2.1 AA violations. Record score and issues.

**Result: Fail** · AE-UI-002. /: 3 violations (1 critical: button-name); /product_details/1: 3 violations (2 critical: button-name, label); /login: 2 violations (1 critical: button-name). button-name: icon-only buttons (e.g. the subscribe arrow) have no accessible name; label: form field without a label on the product page.

### UI-NFR-05

**Page load time**

| | |
|---|---|
| Priority | Low |
| Type | Performance (indicative) |
| Requirement | NFR-04 |
| Preconditions | DevTools Network tab, cache disabled, no throttling. |
| Test data | - |

1. Load home, products and a product detail page three times each.

**Expected:** DOMContentLoaded under 3 s on average. This is a sanity check, not a load test.

**Result: Pass**. Average DOMContentLoaded (ads blocked): / 1524 ms, /products 2069 ms, /product_details/1 759 ms.

### UI-NFR-06

**Site is served over HTTPS**

| | |
|---|---|
| Priority | Medium |
| Type | Security |
| Requirement | NFR-05 |
| Preconditions | - |
| Test data | http://automationexercise.com |

1. Open the http:// address.

**Expected:** Browser is redirected to https:// and shows a valid certificate.

**Result: Pass**. Redirected to https://automationexercise.com/

## End-to-end customer journeys

| ID | Title | Priority | Type | Result |
|---|---|---|---|---|
| [UI-E2E-01](#ui-e2e-01) | New customer: register at checkout, pay, download invoice, delete account | High | End-to-end | Pass |
| [UI-E2E-02](#ui-e2e-02) | Returning customer: log in, search, buy | High | End-to-end | Pass |
| [UI-E2E-03](#ui-e2e-03) | Browsing shopper: filter, compare, edit cart, then log in | Medium | End-to-end | Pass |

### UI-E2E-01

**New customer: register at checkout, pay, download invoice, delete account**

| | |
|---|---|
| Priority | High |
| Type | End-to-end |
| Requirement | REQ-01, REQ-03, REQ-08, REQ-11, REQ-12, REQ-13 |
| Preconditions | Logged out, empty cart. |
| Test data | New customer details, test card |

1. Add two products to the cart as a guest.
2. Proceed to checkout and choose Register / Login.
3. Sign up and complete account information.
4. Return to the cart and proceed to checkout.
5. Verify address and order review, add a comment, place the order.
6. Pay with the test card.
7. Download the invoice.
8. Delete the account.

**Expected:** Order confirmed, invoice downloaded with the right total, account deleted.

**Result: Pass**. Guest cart (2 items) kept through signup at checkout; address shown; order placed; invoice reads "Your total purchase amount is 900"; account deleted.

### UI-E2E-02

**Returning customer: log in, search, buy**

| | |
|---|---|
| Priority | High |
| Type | End-to-end |
| Requirement | REQ-02, REQ-05, REQ-08, REQ-11, REQ-12 |
| Preconditions | Registered account, empty cart. |
| Test data | Search term "jean" |

1. Log in.
2. Search for jeans, open one and set quantity 2.
3. Add to cart and check out.
4. Pay with the test card.

**Expected:** Order confirmed; cart is empty afterwards.

**Result: Pass**. Bought 2 × "Soft Stretch Jeans"; order confirmed; cart empty afterwards.

### UI-E2E-03

**Browsing shopper: filter, compare, edit cart, then log in**

| | |
|---|---|
| Priority | Medium |
| Type | End-to-end |
| Requirement | REQ-06, REQ-08, REQ-09, REQ-10 |
| Preconditions | Logged out. |
| Test data | Category Women → Tops, brand Polo |

1. Filter by category and add one product.
2. Filter by brand and add one product.
3. Remove the first product from the cart.
4. Log in and open the cart.

**Expected:** Only the brand product remains in the cart after login.

**Result: Pass**. Category product 1 removed; only brand product 8 remains after login.


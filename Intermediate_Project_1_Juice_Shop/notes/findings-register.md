# Vulnerability Findings Register

**Project:** Intermediate Project 1: OWASP Juice Shop Security Assessment

**Assessment target:** [http://localhost:3000](http://localhost:3000)

**Assessment type:** Authorized security testing of a local OWASP Juice Shop training application

**Testing tools:** Kali Linux, Mozilla Firefox, Burp Suite Community Edition, OWASP Juice Shop

**Assessment scope:** Search functionality, login and authentication behaviour, basket functionality, public information pages, and application challenge tracking.

## Assessment Summary

The assessment involved interacting with the OWASP Juice Shop web application and observing its behaviour through Firefox and Burp Suite HTTP history.

The supplied screenshots document search input testing, a browser alert associated with an XSS test, basket contents, login attempts, HTTP requests and responses, public information pages, and the challenge scoreboard.

The scoreboard screenshot (`screenshots/23.png`) indicates that four challenges had been solved at the time of capture. Challenge completion is evidence of progress through the training application and does not, by itself, establish four independently verified security vulnerabilities.

One XSS issue appears to be demonstrated by the browser alert shown in `screenshots/06.png`. The remaining investigation areas require additional evidence before vulnerabilities can be confirmed.

---

## Finding 1: Cross-Site Scripting (XSS)

**Title:** JavaScript execution through the search input

**Category:** Cross-Site Scripting (XSS), OWASP Injection

**Affected feature:** Product search functionality

**Steps performed:**

1. Opened the OWASP Juice Shop homepage in Firefox.
2. Entered search terms into the search field and observed the resulting page.
3. Tested a crafted HTML/JavaScript input in the search field within the local training application.
4. Observed the browser's response to the crafted input.
5. Reviewed application traffic using Burp Suite HTTP history.

**Expected behaviour:**

The application should treat user-supplied search text as data and display it safely. Input must not be interpreted as executable JavaScript or HTML capable of executing scripts.

**Actual observed behaviour:**

The screenshot `screenshots/06.png` shows a browser alert associated with `localhost:3000` displaying the text `xss`. The search-related screenshots (`screenshots/02.png` and `screenshots/03.png`) document the search interaction.

This is evidence of JavaScript execution associated with the search testing flow. The precise execution path and XSS subtype should be confirmed from the corresponding request and application behaviour before assigning a definitive subtype.

**Evidence filename:**

- `screenshots/02.png`: Normal product search.
- `screenshots/03.png`: Crafted search input and search results.
- `screenshots/06.png`: Browser alert showing `xss`.
- `screenshots/01.png`: Burp Suite HTTP history and selected request.

**Security impact:**

If the behaviour is reproducible for other users through an application-accessible input, an attacker may be able to execute JavaScript in the application's origin. Depending on the application's security controls and the victim's session, this could enable page manipulation, phishing, or access to information available to the injected script.

The demonstrated impact in the supplied evidence is JavaScript execution in the local training application. No theft of credentials or session tokens is demonstrated.

**Severity and justification:**

Provisional severity: Medium.

The observed script execution supports an XSS finding in the training application. A production severity assessment would require confirming exploitability, affected users, the execution context, and the practical impact. No formal CVSS score has been calculated.

**Recommended mitigation:**

- Apply context-aware output encoding to untrusted data.
- Avoid unsafe DOM APIs and inserting untrusted strings as executable HTML.
- Use safe text-rendering methods such as `textContent` where appropriate.
- Implement a restrictive Content Security Policy as a defence-in-depth measure.
- Add regression tests to ensure crafted search input is rendered safely.

**Status:** Apparent XSS demonstrated in the local lab. Confirm the exact execution path and affected code before finalising the technical subtype.

---

## Finding 2: Broken Access Control

**Title:** Access-control enforcement review of basket resources

**Category:** Broken Access Control, Authorization

**Affected feature:** Shopping basket functionality

**Steps performed:**

1. Opened the Juice Shop application in Firefox.
2. Navigated to the basket page.
3. Observed the products displayed in the basket.
4. Reviewed related application traffic in Burp Suite HTTP history.

**Expected behaviour:**

The server should enforce authorization for every protected resource and action. A user should only be able to view or modify resources that their account is authorized to access.

**Actual observed behaviour:**

The screenshots `screenshots/08.png` and `screenshots/09.png` show a basket containing product entries and associated controls.

The supplied evidence demonstrates basket functionality for the session being used. It does not demonstrate that one user can access or modify another user's basket, nor does it show a successful authorization bypass.

**Evidence filename:**

- `screenshots/08.png`: Basket containing product entries.
- `screenshots/09.png`: Basket view.
- `screenshots/10.png`: Burp Suite HTTP history.

**Security impact:**

No unauthorized access or modification has been demonstrated by the supplied evidence. If a resource-level authorization weakness were confirmed, it could potentially expose private basket information or allow unauthorized changes to another user's data.

**Severity and justification:**

Not assigned. A severity rating would be premature without evidence of an authorization failure.

**Recommended mitigation:**

- Enforce authorization checks on the server for every protected request.
- Verify resource ownership before returning or modifying basket data.
- Do not rely solely on client-side controls or user-supplied resource identifiers.
- Add automated tests for cross-account access attempts.

**Status:** Not confirmed. The evidence shows normal basket functionality, but does not establish a broken access-control vulnerability.

---

## Finding 3: Authentication Weakness

**Title:** Login failure and authentication error-handling review

**Category:** Authentication

**Affected feature:** Login form and authentication endpoint

**Steps performed:**

1. Opened the Juice Shop login page.
2. Entered test account credentials and submitted login attempts.
3. Observed the application's response to unsuccessful authentication.
4. Reviewed login-related requests and responses in Burp Suite HTTP history.

**Expected behaviour:**

The application should reject invalid credentials, avoid creating an authenticated session after a failed login, handle errors safely, and apply appropriate protections against automated password guessing.

**Actual observed behaviour:**

The screenshots `screenshots/11.png`, `screenshots/12.png`, and `screenshots/14.png` through `screenshots/17.png` document the login interface and different input states. The accompanying Burp screenshots show authentication-related traffic.

The evidence includes a visible login error and unsuccessful login behaviour. It does not establish an authentication bypass, successful login with invalid credentials, or the absence of effective rate limiting.

**Evidence filename:**

- `screenshots/11.png`: Login page.
- `screenshots/12.png`: Login and account registration interface.
- `screenshots/13.png`: Burp Suite request and response evidence.
- `screenshots/17.png`: Login form displaying an error after unusual input.

**Security impact:**

No authentication compromise has been demonstrated. An actual authentication weakness could permit unauthorized account access or facilitate password guessing, depending on the failure being verified.

**Severity and justification:**

Not assigned. The observed failed-login behaviour alone does not constitute an authentication vulnerability.

**Recommended mitigation:**

- Use secure password hashing and verification.
- Apply appropriate rate limiting and monitoring to authentication endpoints.
- Maintain secure session handling and invalidate sessions when necessary.
- Return safe, consistent authentication errors.
- Test authentication controls using controlled, documented test cases.

**Status:** Not confirmed. Failed-login behaviour was observed, but no authentication bypass or other specific authentication weakness has been demonstrated.

---

## Finding 4: Injection and SQL Injection

**Title:** Input-handling behaviour of the login endpoint

**Category:** Injection, Input Validation

**Affected feature:** Login form and authentication request processing

**Steps performed:**

1. Submitted a normal failed-login attempt using the test account's email and an incorrect password.
2. Submitted a separate login attempt using a single quote character in the email field and an incorrect password.
3. Observed the application's response.
4. Reviewed the corresponding authentication traffic in Burp Suite HTTP history.
5. Compared the observed behaviour with the normal failed-login response.

**Expected behaviour:**

The application should process user input safely, reject invalid authentication attempts, and avoid exposing internal implementation details or allowing input to alter the intended database query.

**Actual observed behaviour:**

The screenshots document login attempts and related HTTP traffic. `screenshots/17.png` appears to show an error associated with unusual input, while `screenshots/13.png`, `screenshots/18.png`, and `screenshots/22.png` document Burp Suite traffic.

The available screenshots do not conclusively establish that the error originated from a database query, that SQL query logic was altered, or that authentication was bypassed.

A malformed-input error may indicate an input-handling problem, but it is not sufficient proof of SQL injection.

**Evidence filename:**

- `screenshots/13.png`: Authentication request and response.
- `screenshots/17.png`: Login error after unusual input.
- `screenshots/18.png`: Burp Suite HTTP history.
- `screenshots/22.png`: Additional request and response evidence.

**Security impact:**

SQL injection has not been demonstrated. If a database injection flaw were confirmed, its impact would depend on the vulnerable query and could include unauthorized data access or modification.

**Severity and justification:**

Not assigned. The evidence is insufficient to confirm SQL injection or determine its impact.

**Recommended mitigation:**

- Use parameterized queries and safe query-building mechanisms.
- Validate input on the server without relying on validation as the sole defence.
- Avoid exposing detailed internal exceptions to users.
- Log diagnostic details securely on the server.
- Test relevant database operations using a documented challenge designed for the application's actual backend technology.

**Status:** Not confirmed. An unusual-input response was observed, but SQL injection has not been established.

---

## Finding 5: Sensitive Information Exposure

**Title:** Review of publicly accessible application information

**Category:** Sensitive Information Exposure, Information Disclosure

**Affected feature:** Public About Us and Privacy Policy pages

**Steps performed:**

1. Opened the application's public information pages in Firefox.
2. Inspected the About Us page.
3. Inspected the Privacy Policy page.
4. Observed the information presented through the application's own interface.
5. Reviewed related traffic in Burp Suite HTTP history.

**Expected behaviour:**

Public pages should disclose only information intended for public access. Credentials, authentication tokens, private personal information, and other sensitive internal data should not be exposed through public pages or error responses.

**Actual observed behaviour:**

The screenshots `screenshots/19.png` and `screenshots/21.png` show the About Us and Privacy Policy pages. The visible content includes corporate information text and privacy-related information.

The screenshots do not demonstrate the disclosure of credentials, session tokens, private customer information, or other clearly sensitive data. Publicly accessible policy text is not, by itself, a security vulnerability.

**Evidence filename:**

- `screenshots/19.png`: About Us page.
- `screenshots/21.png`: Privacy Policy page.
- `screenshots/20.png`: Related Burp Suite HTTP history.

**Security impact:**

No sensitive information exposure has been confirmed. Additional evidence would be required to establish that the application exposes information that should be restricted.

**Severity and justification:**

Not assigned. No sensitive disclosure has been demonstrated in the supplied screenshots.

**Recommended mitigation:**

- Minimize information exposed in public responses.
- Never include credentials, API secrets, or session tokens in client-accessible code or public pages.
- Restrict sensitive endpoints using server-side authorization.
- Return generic error messages to users while keeping detailed diagnostics in protected logs.
- Review public responses for unintended data disclosure.

**Status:** Not confirmed. Public information pages were inspected, but no specific sensitive information disclosure was demonstrated.

## Overall Conclusion

The supplied evidence documents a practical security-testing exercise using OWASP Juice Shop and Burp Suite. The search testing produced a browser alert consistent with XSS, which warrants documenting as the primary finding after confirming its execution path.

The basket, authentication, injection, and public-information investigations provide useful testing evidence, but the supplied screenshots do not establish four additional vulnerabilities.&#x20;

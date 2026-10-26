# OWASP Juice Shop Security Assessment

## Intern Details

**Name:** Shaurya Gupta  
**Email:** shauryagupta0106@gmail.com  
**Reference Number:** CSN-OC-OL-26194  
**Registration Number:** CSN-OC-2026-194  
**Domain:** Cyber Security  
**Project Level:** Intermediate  
**Project Number:** 1  
**Internship Month/Year:** October 2026

---

## 1. Project Description

The objective of this project was to conduct a practical web application security assessment of OWASP Juice Shop, an intentionally vulnerable web application designed for security education and authorized penetration-testing practice.

The assessment focused on exploring common web application security risks, observing how the application processes user input, examining authentication behaviour, inspecting HTTP requests and responses, and reviewing the application's publicly accessible information.

The primary investigation areas were:

1. Cross-Site Scripting (XSS)
2. Broken Access Control
3. Authentication Weaknesses
4. Injection and SQL Injection
5. Sensitive Information Exposure

Web application security testing is important because modern applications handle user accounts, personal information, transactions, and other potentially sensitive data. Security weaknesses in input handling, authentication, authorization, and information disclosure can expose applications to unauthorized access, data manipulation, or other security incidents.

Manual testing combined with HTTP traffic inspection helps identify unexpected application behaviour and provides evidence for evaluating whether a security weakness exists.

The project also provided practical experience with Kali Linux, Firefox, Burp Suite Community Edition, HTTP request analysis, evidence collection, and technical security reporting.

The assessment was performed as a training exercise against the local Juice Shop instance documented in the supplied screenshots.

---

## 2. Scope and Methodology

### 2.1 Assessment Scope

**Target application:** OWASP Juice Shop  
**Target URL documented in evidence:** `http://localhost:3000`  
**Assessment type:** Authorized security assessment of a local training application  
**Testing approach:** Manual application exploration and HTTP request inspection

The original project template references `https://demo.owasp-juice.shop/`. However, the screenshots previously reviewed show the local application at `http://localhost:3000`. Therefore, this report documents the local environment. The external demo is not represented as having been tested.

### 2.2 Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Security testing environment |
| Mozilla Firefox | Accessing and interacting with the web application |
| Burp Suite Community Edition | Intercepting and inspecting HTTP requests and responses |
| OWASP Juice Shop | Intentionally vulnerable application used for testing |
| Terminal utilities | Checking application connectivity and supporting troubleshooting |
| Screenshots | Recording observed application behaviour and supporting findings |

### 2.3 Testing Methodology

The assessment followed a manual testing process.

**Step 1: Application access**

The Juice Shop application was opened in Firefox. The local application address was used for the tests documented in the screenshots.

**Step 2: Proxy configuration**

Burp Suite was configured to use a local proxy listener at `127.0.0.1:8080`, and Firefox was configured to route browser traffic through the proxy.

**Step 3: Application exploration**

The login interface, product search functionality, shopping basket, About Us page, and Privacy Policy page were examined.

**Step 4: HTTP traffic inspection**

Burp Suite HTTP history was used to inspect browser-generated requests and corresponding responses.

**Step 5: Security investigation**

The application was examined for potential XSS, authorization weaknesses, authentication problems, injection issues, and sensitive information exposure.

**Step 6: Evidence collection**

Screenshots were collected to document the observed behaviour, relevant application interfaces, and HTTP traffic.

**Step 7: Finding assessment**

Each investigation was evaluated according to the available evidence. An attempted test was not treated as a confirmed vulnerability unless the observed behaviour supported the claim.

### 2.4 Scope Limitations

The assessment was limited to the functionality and evidence documented in the supplied screenshots. No claim is made that every endpoint, application component, or security control was exhaustively tested.

No destructive testing, production-system exploitation, or unauthorized access to another user's private information is claimed.

---

## 3. Implementation Details

### 3.1 Environment Setup

The assessment environment consisted of Kali Linux running in a virtualized environment, with Firefox used to access OWASP Juice Shop and Burp Suite Community Edition used for HTTP inspection.

The application address documented in the evidence was:

`http://localhost:3000`

This address refers to a local application instance. The assessment report therefore distinguishes the local instance from the separately hosted public demo.

### 3.2 Burp Suite Configuration

Burp Suite Community Edition was opened and configured using its default settings.

The intended proxy listener configuration was:

- Interface: `127.0.0.1`
- Port: `8080`
- Browser: Firefox running in Kali Linux

Firefox was configured to use the local Burp proxy. The Proxy Intercept feature was switched off during ordinary browsing so that requests could pass through without requiring manual forwarding.

The HTTP history interface was then used to inspect requests generated by the browser.

The supplied screenshots document HTTP history and selected requests. This report does not claim that every request was captured or that every HTTPS response was successfully decrypted.

### 3.3 Cross-Site Scripting Investigation

The product search feature was examined using ordinary search text and crafted test input.

The supplied screenshot `screenshots/02.png` documents a normal search interaction. `screenshots/03.png` documents the crafted search input and resulting search interaction. `screenshots/06.png` shows a browser alert displaying `xss`.

The alert is consistent with JavaScript execution in the tested application. This is the strongest potential vulnerability evidence in the supplied material.

The exact XSS subtype and execution path should be verified before final classification. The screenshot does not, by itself, demonstrate credential theft, session compromise, or impact on another user.

### 3.4 Broken Access Control Investigation

The shopping basket functionality was examined to observe the application's handling of basket data and related interface controls.

The supplied screenshots `screenshots/08.png` and `screenshots/09.png` document the basket interface and product entries.

The evidence demonstrates normal basket functionality. It does not establish that the application permitted unauthorized access to another user's basket or other protected resources.

### 3.5 Authentication Investigation

The login interface was examined using test inputs and unsuccessful authentication attempts.

Screenshots `screenshots/11.png`, `screenshots/12.png`, and `screenshots/14.png` through `screenshots/17.png` document the login interface and input states.

Related request and response evidence appears in `screenshots/13.png`, `screenshots/18.png`, and other Burp Suite screenshots.

The observed errors and failed-login behaviour were recorded. The available evidence does not establish successful authentication bypass, account compromise, or ineffective rate limiting.

### 3.6 Injection Investigation

The login input-handling investigation included a single-quote test and comparison with ordinary failed-login behaviour.

The purpose was to observe whether unusual input produced evidence of unsafe query processing or unexpected authentication behaviour.

The screenshots document unusual input and application errors. However, the supplied evidence does not establish that a database query was altered, that SQL injection occurred, or that authentication was bypassed.

### 3.7 Information Exposure Investigation

Public application information was reviewed through the About Us and Privacy Policy pages.

The supplied screenshots `screenshots/19.png` and `screenshots/21.png` document these pages. Related traffic is shown in `screenshots/20.png`.

The visible information was reviewed for evidence of sensitive data exposure. The screenshots do not demonstrate disclosure of credentials, authentication tokens, or private customer information.

### 3.8 Evidence Collection

The evidence was supplied as a ZIP archive containing screenshots. The filenames cited in this report refer to the screenshots as described in the previous evidence review.

Before external submission, screenshots should be checked for visible passwords, session cookies, authorization tokens, or other secrets. Sensitive values must be redacted without changing the underlying security evidence.

---

## 4. Findings Summary

The following table summarizes the five investigation areas. Only the XSS investigation produced evidence consistent with a security vulnerability. The other four areas remain unconfirmed.

| ID | Finding or Investigation | Severity | Affected Feature | Evidence | Status | Recommended Action |
|---|---|---|---|---|---|---|
| F-01 | JavaScript execution through search input | Medium, provisional | Product search | `screenshots/02.png`, `screenshots/03.png`, `screenshots/06.png` | Apparent XSS demonstrated; confirm exact execution path | Context-aware output encoding and safe DOM handling |
| F-02 | Broken access control | Not assigned | Shopping basket | `screenshots/08.png`, `screenshots/09.png`, `screenshots/10.png` | Not confirmed | Verify server-side authorization and resource ownership |
| F-03 | Authentication weakness | Not assigned | Login interface | `screenshots/11.png`, `screenshots/12.png`, `screenshots/13.png`, `screenshots/17.png` | Not confirmed | Review password verification, rate limiting, and session handling |
| F-04 | Injection and SQL injection | Not assigned | Login input processing | `screenshots/13.png`, `screenshots/17.png`, `screenshots/18.png`, `screenshots/22.png` | Not confirmed | Verify safe query construction and investigate with a documented challenge |
| F-05 | Sensitive information exposure | Not assigned | Public information pages | `screenshots/19.png`, `screenshots/20.png`, `screenshots/21.png` | Not confirmed | Review public responses and restrict sensitive information |

**Confirmed-finding count:** Zero formally verified findings based on a complete, reproducible technical validation process.

**Apparent vulnerability requiring further verification:** One XSS finding.

**Other investigation areas not confirmed:** Four.

The XSS evidence is consistent with script execution in the local training application. It is retained as a provisional finding pending confirmation of the exact execution path and reproducibility.

---

## 5. Detailed Findings

### 5.1 Finding F-01: Potential Cross-Site Scripting Through Search

**Title:** JavaScript execution associated with crafted search input

**Category:** Cross-Site Scripting (XSS)

**Affected feature:** Product search

**Target:** `http://localhost:3000`

**Preconditions:**

- The local OWASP Juice Shop application is accessible.
- The tester can interact with the product search field.
- Firefox can display the application response.

**Steps performed:**

1. Opened the Juice Shop homepage.
2. Entered ordinary search text and observed the result.
3. Entered crafted HTML/JavaScript input into the search field.
4. Observed the browser's behaviour.
5. Reviewed the related application traffic and screenshots.

**Expected behaviour:**

User-supplied search input should be treated as data and rendered safely. It should not be interpreted as executable script.

**Actual observed behaviour:**

The screenshot `screenshots/06.png` shows a browser alert displaying `xss` associated with the local Juice Shop application. This is consistent with JavaScript execution during the XSS investigation.

The available evidence does not independently establish the precise execution path, whether the issue is DOM-based or reflected, or whether an attacker could reliably exploit the behaviour against another user's session.

**Security impact:**

If the behaviour is reproducible and exploitable through an application-accessible input, an attacker may be able to execute JavaScript in the application's origin.

Depending on the affected context and security controls, possible consequences include unauthorized page manipulation, deceptive interface content, phishing, or access to information available to the injected script.

No evidence demonstrates that credentials were stolen, session tokens were compromised, or another user's account was affected.

**Severity and justification:**

**Provisional severity: Medium.**

This provisional rating reflects the apparent script execution and potential consequences of an XSS vulnerability. A formal severity rating requires confirmation of the execution context, exploitability, affected users, and demonstrated impact.

**Evidence screenshots:**

- `screenshots/02.png`: Normal search interaction.
- `screenshots/03.png`: Crafted search input and search result.
- `screenshots/06.png`: Browser alert displaying `xss`.
- `screenshots/01.png`: Burp Suite HTTP history.

**Recommended mitigation:**

1. Apply context-aware output encoding.
2. Avoid unsafe DOM APIs that interpret untrusted input as HTML or JavaScript.
3. Use safe DOM methods such as `textContent` where appropriate.
4. Sanitize HTML only when HTML rendering is necessary, using a maintained sanitizer.
5. Implement a restrictive Content Security Policy as defence in depth.
6. Add regression tests to verify that crafted search input is rendered safely.

**Verification and retest result:**

The supplied evidence documents the browser alert but does not contain a complete remediation and retest cycle.

A retest should repeat the original input after remediation and verify that it is displayed safely without executing script.

**Status:** Provisional. Apparent XSS behaviour documented; exact execution path and reproducibility require verification.

---

### 5.2 Finding F-02: Broken Access Control Investigation

**Title:** Authorization enforcement for basket resources

**Category:** Broken Access Control

**Affected feature:** Shopping basket

**Preconditions:**

- The Juice Shop application is accessible.
- The tester can access the basket interface.
- A test account and test data are available where required.

**Steps performed:**

1. Opened the application in Firefox.
2. Navigated to the basket.
3. Observed the displayed products and available controls.
4. Reviewed related HTTP traffic.

**Expected behaviour:**

The server should verify that each user is authorized to view or modify the requested resource. Access to another user's basket must not be granted solely because a request contains a particular resource identifier.

**Actual observed behaviour:**

The supplied screenshots show basket entries and normal basket functionality.

No successful unauthorized access to another user's data, cross-account basket modification, or other authorization bypass is demonstrated.

**Security impact:**

No security impact from a broken access-control flaw has been established by the supplied evidence.

If a resource-level authorization failure were confirmed, possible consequences could include exposure of private basket information or unauthorized modification of another user's data.

**Severity and justification:**

Not assigned because the required security failure has not been demonstrated.

**Evidence screenshots:**

- `screenshots/08.png`: Basket interface and product entries.
- `screenshots/09.png`: Basket view.
- `screenshots/10.png`: Related HTTP traffic.

**Recommended mitigation:**

- Enforce authorization on the server for each protected request.
- Validate resource ownership before returning or modifying data.
- Do not rely solely on client-side controls.
- Add automated tests for unauthorized cross-account access.

**Verification and retest result:**

No confirmed access-control failure or remediation retest is documented.

**Status:** Not confirmed. Additional authorized testing is required.

---

### 5.3 Finding F-03: Authentication Weakness Investigation

**Title:** Login failure handling and authentication controls

**Category:** Authentication

**Affected feature:** Login interface and authentication processing

**Preconditions:**

- The application login page is accessible.
- A test account or deliberately invalid credentials can be used.
- Burp Suite can inspect browser-generated requests.

**Steps performed:**

1. Opened the login interface.
2. Submitted authentication attempts using test input.
3. Observed the application's response to unsuccessful attempts.
4. Reviewed the related request and response evidence.

**Expected behaviour:**

Invalid credentials should not result in an authenticated session. Authentication controls should handle errors safely, protect passwords, and implement appropriate protections against automated guessing.

**Actual observed behaviour:**

The screenshots document the login form, unsuccessful authentication behaviour, and related HTTP traffic.

The supplied evidence does not establish successful authentication with invalid credentials, a session being granted without authentication, or the absence of effective rate limiting.

**Security impact:**

No authentication compromise has been demonstrated.

A confirmed authentication weakness could potentially enable unauthorized access or increase the risk of password guessing, depending on the weakness and its exploitability.

**Severity and justification:**

Not assigned because no specific authentication vulnerability has been confirmed.

**Evidence screenshots:**

- `screenshots/11.png`: Login page.
- `screenshots/12.png`: Login and registration interface.
- `screenshots/13.png`: Authentication request and response evidence.
- `screenshots/17.png`: Login error following unusual input.

**Recommended mitigation:**

- Use secure password hashing and verification.
- Implement appropriate authentication rate limiting and monitoring.
- Maintain secure session creation, storage, and invalidation.
- Avoid revealing unnecessary internal details in authentication errors.
- Test authentication controls using documented, controlled test cases.

**Verification and retest result:**

No successful authentication bypass or other specific weakness is established by the supplied evidence.

**Status:** Not confirmed.

---

### 5.4 Finding F-04: Injection and SQL Injection Investigation

**Title:** Login input handling and potential injection behaviour

**Category:** Injection

**Affected feature:** Login input processing

**Preconditions:**

- The application login form is accessible.
- The tester can submit controlled test input.
- Burp Suite HTTP history is available for request and response inspection.

**Steps performed:**

1. Submitted a normal failed-login attempt.
2. Submitted another attempt with a single quote in the email field and an incorrect password.
3. Observed the application response.
4. Compared the behaviour with the normal failed-login attempt.
5. Reviewed the relevant HTTP request and response evidence.

**Expected behaviour:**

Input should be processed safely. Invalid input should not alter the intended database query or authentication logic, and internal implementation details should not be exposed unnecessarily.

**Actual observed behaviour:**

The supplied screenshots document unusual login input, application errors, and related HTTP traffic.

The available evidence does not demonstrate that the single quote changed a database query, caused an exploitable SQL injection condition, or bypassed authentication.

An ordinary authentication error or malformed-input response is insufficient to establish SQL injection.

**Security impact:**

No SQL injection vulnerability or related unauthorized database access has been demonstrated.

If injection were confirmed, the impact would depend on the affected query and could include unauthorized data access or modification.

**Severity and justification:**

Not assigned because the existence and impact of an injection vulnerability have not been established.

**Evidence screenshots:**

- `screenshots/13.png`: Authentication request and response.
- `screenshots/17.png`: Login error after unusual input.
- `screenshots/18.png`: Burp Suite HTTP history.
- `screenshots/22.png`: Additional HTTP traffic evidence.

**Recommended mitigation:**

- Use parameterized queries and safe query-building mechanisms.
- Validate input on the server.
- Avoid exposing detailed database errors to users.
- Store diagnostic information in protected server logs.
- Test relevant operations against documented injection challenges appropriate to the application's backend technology.

**Verification and retest result:**

The supplied evidence does not confirm SQL injection. A further test using an appropriate documented Juice Shop challenge would be required.

**Status:** Not confirmed.

---

### 5.5 Finding F-05: Sensitive Information Exposure Investigation

**Title:** Review of publicly accessible application information

**Category:** Sensitive Information Exposure

**Affected feature:** About Us and Privacy Policy pages

**Preconditions:**

- The application is accessible through Firefox.
- Public information pages can be viewed.
- Burp Suite HTTP history is available for examining related responses.

**Steps performed:**

1. Opened the application's public information pages.
2. Reviewed the About Us page.
3. Reviewed the Privacy Policy page.
4. Observed the content presented to the browser.
5. Inspected related HTTP traffic.

**Expected behaviour:**

Public pages should contain only information intended for public access. Sensitive credentials, private customer information, authentication tokens, and other restricted data should not be disclosed without authorization.

**Actual observed behaviour:**

The supplied screenshots show the About Us and Privacy Policy pages and related HTTP history.

The available evidence does not demonstrate the exposure of credentials, session tokens, private customer data, or other clearly sensitive information.

Publicly accessible informational text is not automatically a vulnerability.

**Security impact:**

No sensitive information disclosure has been confirmed.

If restricted information were exposed, potential consequences could include privacy violations, unauthorized access, or misuse of confidential data.

**Severity and justification:**

Not assigned because no specific sensitive disclosure has been demonstrated.

**Evidence screenshots:**

- `screenshots/19.png`: About Us page.
- `screenshots/20.png`: Related Burp Suite HTTP history.
- `screenshots/21.png`: Privacy Policy page.

**Recommended mitigation:**

- Minimize information exposed through public responses.
- Never place credentials or private keys in client-accessible code.
- Restrict sensitive endpoints through server-side authorization.
- Use safe error responses and protect diagnostic logs.
- Review responses for unintended disclosure of private information.

**Verification and retest result:**

No confirmed sensitive information exposure or remediation retest is documented.

**Status:** Not confirmed.

---

## 6. Results

### 6.1 Summary of Results

The assessment documented manual testing of search functionality, login behaviour, basket functionality, public information pages, and HTTP traffic.

The main observations were:

- A browser alert displaying `xss` was captured during the search-input investigation.
- Normal basket functionality was observed.
- Unsuccessful login attempts and application errors were observed.
- A single-quote input test was performed as a preliminary injection investigation.
- Public About Us and Privacy Policy pages were inspected.
- Burp Suite screenshots documented selected application traffic.
- The supplied scoreboard screenshot showed four solved challenges at the time of capture.

### 6.2 Investigation Statistics

| Metric | Result |
|---|---:|
| Investigation areas documented | 5 |
| Formally verified vulnerabilities | 0 |
| Apparent XSS findings requiring technical verification | 1 |
| Other investigation areas not confirmed | 4 |
| Confirmed remediation and retest cycles | 0 |

The distinction between a completed investigation and a confirmed vulnerability is important. A test may be completed without demonstrating a security flaw.

The apparent XSS behaviour is the strongest finding in the supplied evidence. It should be retained as a provisional finding until its execution path and reproducibility are verified.

### 6.3 Limitations

1. The assessment was limited to the functions and screenshots documented in the supplied evidence.
2. The precise execution path and subtype of the apparent XSS issue were not fully established.
3. Broken access control was not demonstrated through an unauthorized resource-access test.
4. Authentication bypass and ineffective rate limiting were not established.
5. SQL injection was not confirmed by the single-quote input test.
6. Sensitive information exposure was not demonstrated through the public information pages.
7. The scoreboard's solved-challenge count does not independently establish the number of distinct vulnerabilities confirmed.
8. No comprehensive source-code review, automated vulnerability scan, formal CVSS calculation, or remediation retest is claimed.
9. The screenshots document the local target. They do not establish that the external demo website was tested.
10. The assessment should not be considered a comprehensive security certification of the application.

---

## 7. Summary and Recommendations

### 7.1 Prioritized Recommendations

**Priority 1: Verify and remediate the apparent XSS behaviour**

The browser alert is the strongest security signal in the supplied evidence.

Recommended actions:

- Reproduce the original test in the local training application.
- Identify the exact input-handling path responsible for script execution.
- Determine whether the issue is DOM-based, reflected, or another XSS variant.
- Apply context-aware output encoding and safe DOM handling.
- Retest the same input and verify that it no longer executes JavaScript.

The provisional severity should be reassessed after confirming exploitability and impact.

**Priority 2: Complete authorization testing**

The basket screenshots show ordinary functionality rather than unauthorized access.

Recommended actions:

- Review server-side ownership checks for protected resources.
- Test authorization using separate accounts and controlled test data.
- Verify that one account cannot retrieve or modify another account's resources.
- Record both the request and the observed authorization result.

**Priority 3: Review authentication controls**

The login tests document unsuccessful authentication attempts but do not establish an authentication vulnerability.

Recommended actions:

- Verify that failed authentication never grants an authenticated session.
- Review password verification and session handling.
- Test appropriate rate limiting using controlled attempts.
- Record any specific weakness only after observing reproducible evidence.

**Priority 4: Investigate injection using an appropriate challenge**

The single-quote test did not establish SQL injection.

Recommended actions:

- Identify the actual backend processing path for the tested input.
- Use a documented Juice Shop injection challenge.
- Examine the application response and relevant request behaviour.
- Avoid classifying generic errors as SQL injection.
- Apply parameterized queries and safe input processing where applicable.

**Priority 5: Continue reviewing information disclosure**

The public information pages did not demonstrate sensitive data exposure.

Recommended actions:

- Inspect relevant public responses for unintended sensitive values.
- Verify whether any exposed information is genuinely confidential.
- Check that secrets are not included in client-accessible resources.
- Record the exact exposed data and security impact if a disclosure is found.

### 7.2 Retesting Strategy

After remediation, each confirmed issue should be retested using the original reproducible steps.

The retest should document:

1. The original test input or request.
2. The application response before remediation.
3. The change applied.
4. The application response after remediation.
5. Evidence showing whether the issue remains reproducible.

A finding should be marked as fixed only when the retest demonstrates that the original security failure no longer occurs.

### 7.3 General Security Recommendations

- Adopt secure coding practices for user input and output rendering.
- Enforce authorization checks on the server.
- Protect authentication and session-management processes.
- Use safe database query construction.
- Minimize sensitive information in public responses.
- Maintain regression tests for identified security issues.
- Keep technical evidence and findings traceable to reproducible tests.

---

## 8. Conclusion

This project provided practical experience in assessing a deliberately vulnerable web application using Kali Linux, Firefox, and Burp Suite Community Edition.

The assessment covered search functionality, authentication behaviour, basket operations, input handling, public information pages, and HTTP request inspection.

The strongest security signal in the supplied evidence was a browser alert displaying `xss` during the search-input investigation. This is consistent with JavaScript execution and warrants further technical verification before finalizing the finding's subtype and severity.

The other four investigation areas provided useful testing observations, but the supplied screenshots do not establish additional confirmed vulnerabilities. They therefore remain unconfirmed rather than being reported as successful exploits.

The project reinforced several important principles of web application security testing:

- Security claims must be supported by reproducible evidence.
- A test attempt is not equivalent to a confirmed vulnerability.
- HTTP request inspection helps explain application behaviour but does not independently prove exploitability.
- Severity should be based on demonstrated impact and realistic exploitability.
- Remediation must be verified through retesting.

The assessment demonstrates practical familiarity with manual web application testing, proxy-based HTTP inspection, evidence collection, and structured vulnerability reporting. Further testing and validation would be required to establish additional findings and complete a comprehensive security assessment.

---

## 9. References

1. **OWASP Juice Shop Project**  
   https://owasp.org/www-project-juice-shop/

2. **OWASP Web Security Testing Guide**  
   https://owasp.org/www-project-web-security-testing-guide/

3. **OWASP Top 10**  
   https://owasp.org/www-project-top-ten/

4. **OWASP Juice Shop Companion Guide**  
   https://pwning.owasp-juice.shop/

5. **PortSwigger Burp Suite Documentation**  
   https://portswigger.net/burp/documentation

6. **OWASP Cross Site Scripting Prevention Cheat Sheet**  
   https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

7. **OWASP SQL Injection Prevention Cheat Sheet**  
   https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html

8. **OWASP Authorization Cheat Sheet**  
   https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html

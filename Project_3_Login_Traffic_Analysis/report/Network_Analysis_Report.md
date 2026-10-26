# NETWORK ANALYSIS REPORT

**CyberSNova | Cyber Security Internship | Beginner Level | Project 3**

## 1. Intern Details

- **Name:** Shaurya Gupta
- **Email:** shauryagupta0106@gmail.com
- **Reference Number:** CSN-OC-OL-26194
- **Registration Number:** CSN-OC-2026-194
- **Domain:** Cyber Security
- **Project Level:** Beginner
- **Project Number:** 3
- **Month/Year:** October 2026

---

## 2. Project Description

The objective of this project was to capture and analyze login-related network traffic to understand how web applications transmit information between a client and a server. The project focused on identifying HTTP requests, examining login-related POST requests, inspecting server responses where available, and understanding the importance of encryption when transmitting sensitive information.

The designated training website, `http://testasp.vulnweb.com/`, was used to generate and inspect web traffic. Wireshark and TShark were used for packet capture and analysis, while Burp Suite Community Edition was used to inspect HTTP traffic at the application layer.

The main objectives were:

1. Verify network connectivity to the assigned training website.
2. Capture HTTP and HTTPS-related TCP traffic.
3. Identify HTTP request methods, hostnames, request URIs, and content types.
4. Identify a POST request directed to the login endpoint.
5. Inspect traffic using Wireshark/TShark and Burp Suite.
6. Understand the security implications of transmitting sensitive information over unencrypted HTTP.
7. Document the observed results, limitations, and security recommendations.

---

## 3. Tools and Environment
-------------------------------------------------------------------------------------------------------------------------------
| Component                    | Details                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------|
| Operating System             | Kali Linux                                                                                   |
| Kernel Version               | Linux 7.1.5+kali-amd64                                                                       |
| Virtualization Platform      | Oracle VirtualBox                                                                            |
| Packet Capture Tool          | Dumpcap, part of Wireshark                                                                   |
| Wireshark Version            | 4.6.6                                                                                        |
| TShark Version               | 4.6.6                                                                                        |
| Burp Suite Version           | Burp Suite Community Edition 2026.9-53998 (Early Adopter)                                    |
| Browser                      | Burp Suite built-in browser, Chromium-based; observed User-Agent identifies Chrome 151.0.0.0 |
| Network Interface            | eth0                                                                                         |
| Local IPv4 Address           | 10.0.2.15                                                                                    |
| Default Gateway              | 10.0.2.2                                                                                     |
| Target Website               | http://testasp.vulnweb.com/                                                                  |
| Target Host Observed         | testasp.vulnweb.com                                                                          |
| Target IPv4 Address Observed | 44.238.29.244                                                                                |
| Burp Proxy Listener          | 127.0.0.1:8080                                                                               |
| Capture Format               | PCAPNG                                                                                       |
| Analysis Tools               | TShark, Wireshark, Burp Suite Community Edition                                              |
-------------------------------------------------------------------------------------------------------------------------------

The target IP address is the address observed during this exercise and may change over time.

---

## 4. Methodology

### 4.1 Connectivity Verification

The network configuration was inspected using the `ip` commands to identify the active interface, assigned IP address, and default gateway.

The target website was tested using `curl`. The recorded HTTP response included a `200 OK` status, confirming that the target responded to the connectivity request at that time.

### 4.2 Packet Capture

Dumpcap was used to capture traffic on the `eth0` network interface. A capture filter was applied to record TCP traffic on ports 80 and 443.

The capture was saved to:

`evidence/03_login_session.pcapng`

The capture summary reported:

- Total packets captured: 1,230
- Packets dropped: 0
- Capture duration: approximately 70.43 seconds
- File size: approximately 694 kB
- Capture interface: eth0
- Capture filter: `tcp port 80 or tcp port 443`

### 4.3 HTTP Request Filtering

TShark was used to filter HTTP requests and extract packet numbers, source and destination IP addresses, hostnames, request methods, and request URIs.

The extracted request metadata was saved to:

`evidence/04_http_requests.txt`

The analysis identified requests to the training website, including the homepage, supporting resources, and the login endpoint.

### 4.4 POST Request Inspection

TShark was used to filter HTTP POST requests and record their metadata.

The captured traffic included a POST request in frame 1213 directed to:

`/Login.asp?RetURL=%2FDefault%2Easp%3F`

The reported content type was:

`application/x-www-form-urlencoded`

The extracted POST request metadata was saved to:

`evidence/05_http_post_analysis.txt`

This confirms that a form-encoded POST request was observed. The request metadata alone does not prove that authentication succeeded or that credentials were accepted by the application.

### 4.5 Wireshark/TShark Protocol Analysis

The capture was analyzed using TShark's protocol hierarchy statistics. The results included HTTP and TLS traffic.

The analysis demonstrated that the capture contained web traffic suitable for examining request methods, destinations, and protocol usage.

The presence of TLS traffic elsewhere in the capture does not establish that the login request itself was protected by HTTPS.

### 4.6 Burp Suite Analysis

Burp Suite Community Edition was launched and configured with a proxy listener bound to `127.0.0.1:8080`.

The screenshots show the Burp HTTP history interface displaying requests to `testasp.vulnweb.com`, including GET requests with recorded HTTP 200 responses. The listener settings screenshot shows the proxy listener configured at `127.0.0.1:8080`.

The supplied screenshots establish that Burp was configured and that web requests were visible in its HTTP history. They do not independently establish that the login POST request and its associated response were captured in Burp. That finding should only be added after verifying the corresponding request in Burp's HTTP history.

---

## 5. Implementation and Evidence

The following evidence files and screenshots document the implementation. Screenshot numbers refer to the images supplied in the project screenshots folder.

### Figure 1: Project Directory and Initial Setup

**Evidence:** `screenshots/01.png`

This screenshot shows the creation of the project directories, including the report, evidence, and results folders. It documents the initial organization of the project files.

### Figure 2: Wireshark Version Verification

**Evidence:** `screenshots/02.png`

This screenshot records the installed Wireshark version as 4.6.6. It establishes the packet-analysis software version used during the exercise.

### Figure 3: TShark Version Verification

**Evidence:** `screenshots/03.png`

This screenshot records TShark version 4.6.6, confirming the command-line packet-analysis tool used to filter and examine network traffic.

### Figure 4: Network and Tool Information

**Evidence:** `screenshots/04.png`

This screenshot contains terminal output documenting the installed packet-analysis utilities and the Burp Suite command-line availability.

### Figure 5: Network Interface Configuration

**Evidence:** `screenshots/06.png`

This screenshot records the active `eth0` interface, local IPv4 address `10.0.2.15`, and default gateway `10.0.2.2`. These details establish the network environment used for packet capture.

### Figure 6: Target Connectivity Verification

**Evidence:** `screenshots/07.png`

This screenshot shows an HTTP response from `testasp.vulnweb.com`, including the `200 OK` status and HTTP response headers. It demonstrates that the target responded to the connectivity check.

### Figure 7: Packet Capture Troubleshooting

**Evidence:** `screenshots/08.png`, `screenshots/16.png`, `screenshots/17.png`, `screenshots/18.png`

These screenshots document troubleshooting of the initial capture attempts, including permission errors and zero-packet captures. They also show the investigation used to identify the working interface and capture method.

### Figure 8: Successful Packet Capture

**Evidence:** `screenshots/19.png`, `screenshots/20.png`, `screenshots/21.png`

These screenshots show the successful capture and subsequent analysis. The capture summary records 1,230 packets, zero reported dropped packets, and a duration of approximately 70.43 seconds.

### Figure 9: Burp Suite HTTP History

**Evidence:** `screenshots/13.png`, `screenshots/14.png`

These screenshots show Burp Suite Community Edition's HTTP history interface and requests to the assigned training website. The displayed entries include GET requests and recorded HTTP 200 responses.

### Figure 10: Burp Suite Proxy Listener

**Evidence:** `screenshots/15.png`

This screenshot shows the Burp proxy listener configuration at `127.0.0.1:8080`. It documents the local proxy configuration used for application-layer inspection.

Additional screenshots may be included where they show unique evidence relevant to the final analysis. All screenshots should be genuine captures from the lab, and any sensitive values must be redacted before submission.

---

## 6. Results and Findings

### 6.1 Packet Capture Results

The PCAPNG file contained 1,230 packets captured over approximately 70.43 seconds. Dumpcap reported zero dropped packets during the capture.

The protocol hierarchy showed HTTP and TLS traffic, confirming that multiple types of web traffic were present in the capture.

### 6.2 HTTP Request Findings

The request listing contained traffic to `testasp.vulnweb.com`, including requests to:

- `/`
- `/styles.css`
- `/favicon.ico`
- `/Images/logo.gif`
- `/Login.asp?RetURL=%2FDefault%2Easp%3F`

The login endpoint was requested using HTTP.

### 6.3 POST Request Findings

A POST request was observed in frame 1213 with the URI:

`/Login.asp?RetURL=%2FDefault%2Easp%3F`

Its content type was `application/x-www-form-urlencoded`.

This indicates that the request used a form-encoded body format. However, the extracted metadata does not establish the exact body contents, whether valid credentials were submitted, or whether authentication succeeded.

### 6.4 Response Findings

The target connectivity check recorded an HTTP `200 OK` response. Burp HTTP history screenshots also show GET requests with recorded `200` status codes.

The response status specifically associated with the login POST request has not been established by the supplied evidence. Therefore, no claim is made that the login was successful or unsuccessful.

### 6.5 Transport Security Findings

The capture contains HTTP and TLS traffic. The login endpoint was accessed over HTTP, which does not provide HTTP-layer encryption.

If sensitive login information was transmitted in the HTTP request body, it could potentially be readable by parties able to observe that traffic. The POST request metadata alone does not prove that a password or other secret was transmitted in readable form.

The presence of TLS traffic elsewhere in the capture does not establish that the login exchange used TLS.

### 6.6 Limitations

- The supplied POST analysis contains request metadata, not a verified authentication outcome.
- The login response code and authentication result have not been confirmed.
- The supplied Burp screenshots show GET requests and the proxy listener configuration, but do not independently prove that Burp captured the login POST request.
- The presence of a POST request does not by itself establish a vulnerability.
- No successful account compromise or unauthorized access was demonstrated.

---

## 7. Risk Assessment

### Finding 1: Login Endpoint Accessible over HTTP

**Observation:** The login endpoint was requested over HTTP, and a POST request was observed in the packet capture.

**Affected Data:** Any credentials, session-related information, or other sensitive fields transmitted in the request, if present.

**Potential Impact:** If sensitive information is transmitted without encryption, a party able to observe the relevant network traffic may be able to read or intercept that information.

**Evidence:** `evidence/03_login_session.pcapng`, `evidence/04_http_requests.txt`, and `evidence/05_http_post_analysis.txt`.

**Severity:** Potentially high if actual credentials are transmitted in cleartext over an untrusted network. A definitive severity rating requires confirmation of the transmitted fields, the application's HTTPS behavior, and the relevant threat environment.

**Status:** Security concern requiring verification. The supplied metadata does not by itself prove that credentials were exposed.

### Finding 2: Authentication Outcome Not Verified

**Observation:** A POST request to the login endpoint was identified, but the corresponding authentication result has not been established.

**Potential Impact:** Without examining the response and relevant application behavior, it is not possible to determine whether the submitted login attempt succeeded or failed.

**Evidence:** The POST request metadata and available response records.

**Severity:** Informational limitation, not a confirmed vulnerability.

**Status:** Further inspection is required.

### Finding 3: TLS Traffic Present in the Capture

**Observation:** TLS traffic was identified in the protocol hierarchy.

**Potential Impact:** TLS can protect the confidentiality and integrity of supported connections when correctly configured. However, unrelated TLS traffic does not demonstrate that the login exchange was encrypted.

**Evidence:** `results/capture_summary.txt` and the TShark protocol hierarchy output.

**Severity:** No vulnerability can be assigned from this observation alone.

**Status:** The transport security of the specific login exchange requires verification.

---

## 8. Recommendations

1. **Enforce HTTPS:** Configure login pages and all pages handling sensitive information to use HTTPS. Redirect HTTP requests to HTTPS and consider HTTP Strict Transport Security where appropriate.

2. **Protect Authentication Data:** Ensure that passwords and other sensitive fields are never transmitted over unencrypted HTTP connections.

3. **Review Session Cookies:** Where applicable, configure session cookies with the `Secure` and `HttpOnly` attributes and an appropriate `SameSite` policy.

4. **Avoid Mixed Content:** Ensure that HTTPS pages do not load insecure HTTP resources that could undermine page security.

5. **Use Secure Session Management:** Generate unpredictable session identifiers, rotate session identifiers after authentication, invalidate sessions on logout, and apply suitable expiration controls.

6. **Verify Login Responses:** Inspect the actual server response to each test login request before classifying the attempt as successful or unsuccessful.

7. **Protect Evidence:** Store PCAPNG files and screenshots securely. Redact passwords, cookies, authorization headers, and other sensitive values before including evidence in the report.

8. **Validate Findings:** Confirm the exact request body, transport protection, and server behavior before assigning a final vulnerability severity.

9. **Maintain Authorized Testing Scope:** Perform packet capture and proxy inspection only against approved training systems or systems for which explicit authorization has been granted.

---

## 9. Summary and Conclusion

This project demonstrated the capture and analysis of web traffic using Kali Linux, Dumpcap, Wireshark, and TShark. The successful capture contained 1,230 packets, and the protocol analysis identified both HTTP and TLS traffic.

The HTTP request analysis identified requests to the designated training website and a POST request to the login endpoint. The POST request used the `application/x-www-form-urlencoded` content type. Burp Suite Community Edition was configured with a local proxy listener, and the supplied screenshots show HTTP history entries for the target website.

These results demonstrate how packet-analysis tools can be used to identify web requests and investigate application-layer traffic. The observations also highlight why login endpoints should use HTTPS when handling sensitive information.

However, the available evidence does not establish whether credentials were transmitted in readable form, whether the login attempt succeeded, or whether the login exchange was protected by TLS. Those conclusions require inspection of the relevant request body, corresponding response, and actual transport security.

The project provided practical experience in network configuration, packet capture, HTTP filtering, protocol analysis, proxy configuration, and evidence-based security reporting. It also demonstrated the importance of distinguishing observed facts from assumptions when evaluating potential security risks.

---

## 10. References

The following official documentation provides references for the tools and techniques used in this project. Retain only the sources you actually consulted while performing the exercise.

1. **Wireshark Documentation:** https://www.wireshark.org/docs/
2. **TShark Manual:** https://www.wireshark.org/docs/man-pages/tshark.html
3. **Burp Suite Documentation:** https://portswigger.net/burp/documentation
4. **PortSwigger Web Security Academy:** https://portswigger.net/web-security
5. **Kali Linux Documentation:** https://www.kali.org/docs/
6. **Training Website Used:** http://testasp.vulnweb.com/

**End of Report**

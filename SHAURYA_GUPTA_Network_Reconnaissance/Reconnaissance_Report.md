# CYBERSNOVA INTERNSHIP PROJECT REPORT

## Intern Details

| Field                 | Details                    |
|-----------------------|----------------------------|
| Name                  | SHAURYA GUPTA              |
| Email                 | shauryagupta0106@gmail.com |
| Reference No.         | CSN-OC-OL-26194            |
| Registration No.      | CSN-OC-2026-194            |
| Domain                | Cyber Security             |
| Project Level         | Beginner                   |
| Internship Month/Year | October 2026               |

**Project Title:** Network Reconnaissance and Service Enumeration Using Nmap, WhatWeb, Netcat, and curl

**Target:** testphp.vulnweb.com

**Assessment Type:** Basic external reconnaissance and conservative TCP service enumeration.

**Assessment Environment:** Kali GNU/Linux Rolling, Version 2026.3, running in a virtualized environment.

**Report Status:** Completed with documented connectivity limitations.


---

## 1. Introduction

Network reconnaissance is the process of collecting information about a target system, its network presence, accessible services, and externally observable characteristics. It is commonly performed during the initial stages of an authorized cybersecurity assessment.

Service enumeration builds upon reconnaissance by examining network ports and attempting to identify the services associated with them. Tools such as Nmap help determine whether TCP ports appear open, closed, or filtered. Web fingerprinting tools such as WhatWeb attempt to identify technologies from HTTP responses, while curl and Netcat can be used to inspect HTTP connectivity and server responses.

This project demonstrates a basic external reconnaissance workflow against the designated training target, testphp.vulnweb.com. The assessment included DNS resolution, HTTP and HTTPS connectivity checks, TCP port scanning, web technology fingerprinting, and attempts to establish direct TCP and TLS connections.

The primary objective was to collect reproducible evidence, interpret the results accurately, and understand the limitations of network scanning when the target does not respond to connection attempts.

The assessment also demonstrates an important principle of cybersecurity: a tool failure is not proof of a vulnerability, and an unresponsive service must not be reported as a confirmed security weakness without supporting evidence.

## 2. Problem Statement

The objective of this project was to perform basic network reconnaissance and service enumeration against the designated practice website.

The assessment aimed to:

1. Resolve the target hostname to its associated IP address.
2. Check basic HTTP and HTTPS connectivity.
3. Scan the top 100 TCP ports to identify their observable states.
4. Examine ports 80 and 443 for web service availability.
5. Attempt to identify web technologies using WhatWeb.
6. Attempt to retrieve HTTP response information using curl and Netcat.
7. Preserve the results and explain any limitations encountered.

The assessment encountered repeated connection timeouts and other connection failures. Consequently, the project documents the information successfully collected and distinguishes it from information that could not be verified.

## 3. Scope and Target

| Field                  | Details                                                                |
|------------------------|------------------------------------------------------------------------|
| Target hostname        | testphp.vulnweb.com                                                    |
| Target URL             | http://testphp.vulnweb.com/                                            |
| Resolved IPv4 address  | 44.228.249.3                                                           |
| Reverse DNS            | ec2-44-228-249-3.us-west-2.compute.amazonaws.com                       |
| Assessment type        | Basic external reconnaissance                                          |
| Port scope             | Top 100 TCP ports                                                      |
| Additional port checks | TCP ports 80 and 443                                                   |
| DNS checks             | A, AAAA, CNAME records and system resolver lookup                      |
| Assessment date        | 23 October 2026, as recorded in terminal evidence                      |
| Nmap scan start        | 23 October 2026, 12:08:23 -0400, for the completed top-100 scan        |
| Nmap scan duration     | 202.87 seconds                                                         |
| Operating system       | Kali GNU/Linux Rolling 2026.3                                          |
| Authorization basis    | Training target identified in the supplied CyberSNova task information |

Only the hostname, ports, and checks documented in the supplied terminal evidence are included in the reported scope. No successful exploitation, authentication testing, or vulnerability confirmation was performed.

## 4. Tools Used

### 4.1 Nmap

Nmap is a network exploration and security auditing tool. It sends network probes to identify hosts and determine the observable state of ports.

In this project, Nmap was used to scan the top 100 TCP ports with a conservative timing profile. A separate scan examined ports 80 and 443 with lightweight service detection.

The completed scan reported all 100 TCP ports as filtered, with no responses received. The separate web-port scan also reported ports 80 and 443 as filtered.

These findings indicate that Nmap could not establish the port states through the expected responses. They do not establish that the services are permanently unavailable or that the target has no running services.

### 4.2 WhatWeb

WhatWeb is a web technology fingerprinting tool. It attempts to identify web servers, content management systems, frameworks, libraries, and other technologies by examining responses and recognizable characteristics.

Multiple WhatWeb attempts were made against the target. The observed output included:

ERROR Opening: http://testphp.vulnweb.com/ - execution expired

A subsequent attempt reported:

ERROR Opening: http://testphp.vulnweb.com/ - Connection refused - connect(2) for "44.228.249.3" port 80

Other attempts again ended with an execution timeout.

No successful WhatWeb fingerprint was obtained in the supplied evidence. Therefore, no specific web server, framework, programming language, or content management system is claimed as detected by this assessment.

### 4.3 Netcat

Netcat is a network utility that can establish TCP connections and exchange data with a remote endpoint.

The project attempted to send a basic HTTP HEAD request to the target on TCP port 80.

The observed output was:

nc: getaddrinfo for host "testphp.vulnweb.com" port 80: Name or service not known

This particular Netcat attempt failed during hostname resolution. It did not return an HTTP response, and it cannot be used as evidence of a successful TCP connection.

The failure also contrasts with the successful DNS lookups performed using dig and getent at other points in the assessment. This may indicate a transient or resolver-specific problem, but the available evidence does not establish its exact cause.

### 4.4 curl

curl is a command-line utility for transferring data using protocols including HTTP and HTTPS. It can retrieve response headers, report HTTP status codes, and display connection diagnostics.

In this project, curl was used to test HTTP connectivity, save response headers, summarize the HTTP status, and attempt an HTTPS header request.

The HTTP connectivity test reported:

curl: (28) Connection timed out after 10009 milliseconds

The HTTP status summary reported:

HTTP status: 000
Remote IP:
HTTP version: 0

The HTTPS request also timed out.

HTTP status 000 is curl's indication that no HTTP response status was received. It is not an HTTP status code returned by the target.

The response-header file was empty when inspected. Therefore, no server response headers were confirmed.

### 4.5 dig and getent

dig was used to query DNS A, AAAA, and CNAME records. getent was used to check hostname resolution through the system's configured name-service mechanisms.

These tools successfully resolved the target to the IPv4 address 44.228.249.3 during the recorded checks.

The AAAA and CNAME queries returned no answer records in the captured output. This means that no matching records were displayed in those particular responses. It should not be interpreted as proof that such records can never exist.

### 4.6 OpenSSL

OpenSSL's s_client utility was used to attempt a TLS connection to the target on TCP port 443.

The attempt ended with a timeout:

timeout 12 openssl s_client -connect testphp.vulnweb.com:443 -servername testphp.vulnweb.com -brief </dev/null

Observed terminal result:

zsh: exit 124

The command timed out before a successful TLS session was confirmed. No TLS certificate details or negotiated protocol were verified.

## 5. Methodology

The following workflow was followed using the supplied terminal evidence.

### Step 1: Prepare the project directory

The project structure was created with directories for reports, screenshots, results, and logs.

The working directory was confirmed as:

/home/kali/SHAURYA_GUPTA/Project_1_Network_Reconnaissance

### Step 2: Record the environment

The operating system and installed tool versions were checked. The recorded environment was Kali GNU/Linux Rolling, Version 2026.3.

Nmap, WhatWeb, Netcat, curl, DNS utilities, and OpenSSL were confirmed as installed.

### Step 3: Resolve the target hostname

DNS queries were made for A, AAAA, and CNAME records. The system resolver was also tested using getent.

The A-record query returned 44.228.249.3.

### Step 4: Check HTTP and HTTPS connectivity

curl was used to request HTTP response headers and summarize the response status. A separate HTTPS request was attempted.

Both checks timed out in the supplied evidence. The HTTP status summary recorded status 000, with no remote IP or HTTP version reported.

### Step 5: Scan the top 100 TCP ports

The following Nmap command was used:

nmap -sT -Pn -T2 --top-ports 100 --version-light --reason -oA results/nmap_top100 testphp.vulnweb.com

The completed scan identified the target IP address and reported all 100 scanned ports as filtered with no response.

### Step 6: Examine web ports

A separate Nmap scan examined TCP ports 80 and 443:

nmap -sT -Pn -T2 -sV --version-light --reason -p 80,443 -oA results/nmap_web_services testphp.vulnweb.com

Both ports were reported as filtered, with the reason recorded as no-response.

### Step 7: Perform web fingerprinting

WhatWeb was executed multiple times. The observed attempts ended with execution timeouts or a connection-refused error. No successful technology fingerprint was obtained.

### Step 8: Attempt direct HTTP and TLS connections

Netcat was used to send an HTTP HEAD request, while OpenSSL was used to attempt a TLS handshake.

The Netcat attempt failed during hostname resolution. The OpenSSL attempt timed out.

### Step 9: Preserve and interpret the evidence

Command output was saved in the results directory using tee and Nmap's output options. The available evidence was reviewed to separate confirmed observations from unverified assumptions.

The assessment did not treat missing responses as proof of a vulnerability or as proof that no services exist.

## 6. Findings

### 6.1 DNS and Connectivity

**DNS resolution**

The following A-record result was captured:

testphp.vulnweb.com. 4319 IN A 44.228.249.3

The getent lookup also returned:

44.228.249.3 testphp.vulnweb.com

The reverse DNS name reported by Nmap was:

ec2-44-228-249-3.us-west-2.compute.amazonaws.com

**DNS findings**

| Check              | Observed result        | Interpretation                         |
|--------------------|------------------------|----------------------------------------|
| DNS A record       | 44.228.249.3           | IPv4 address resolved                  |
| DNS AAAA record    | No answer record shown | No IPv6 address confirmed by this quer |
| DNS CNAME record   | No answer record shown | No CNAME confirmed by this query       |
| getent lookup      | 44.228.249.3           | System hostname resolution succeeded   |
| HTTP connectivity  | Connection timeout     | No HTTP response obtained              |
| HTTPS connectivity | Connection timeout     | No successful HTTPS response obtained  |

The target hostname resolved successfully, so DNS resolution was possible during the successful queries. However, successful hostname resolution did not lead to a successful HTTP or HTTPS connection.

**HTTP response status**

The recorded summary was:

HTTP status: 000
Remote IP:
HTTP version: 0

This confirms that curl did not obtain an HTTP response in that attempt. It does not establish that the target returned an HTTP error page.

**Connectivity conclusion**

The available evidence establishes a difference between DNS reachability and TCP application connectivity. DNS returned an address, but the attempted web connections did not produce HTTP responses.

Possible explanations include network filtering, an unavailable destination service, an intermediate firewall, routing issues, or transient network conditions. The supplied evidence does not establish which explanation is correct.

**Evidence files:**
- results/dns.txt
- results/dns_ipv6.txt
- results/dns_cname.txt
- results/resolved_addresses.txt
- results/http_headers.txt
- results/http_status.txt
- results/https_check.txt

### 6.2 Nmap Port and Service Enumeration

**Top 100 TCP ports**

The completed Nmap scan reported:

Nmap scan report for testphp.vulnweb.com (44.228.249.3)
Host is up, received user-set.
rDNS record for 44.228.249.3: ec2-44-228-249-3.us-west-2.compute.amazonaws.com
All 100 scanned ports on testphp.vulnweb.com (44.228.249.3) are in ignored states.
Not shown: 100 filtered tcp ports (no-response)

Nmap completed the scan in 202.87 seconds.

**Results table**

| Field                  | Result            |
|------------------------|-------------------|
| Scan type              | TCP connect scan  |
| Target IP              | 44.228.249.3      |
| Port scope             | Top 100 TCP ports |
| Open ports confirmed   | None              |
| Closed ports confirmed | None              |
| Filtered ports         | 100               |
| Reported reason        | No response       |
| Scan duration          | 202.87 seconds    |

No open TCP ports were confirmed in the completed top-100 scan. The scan also did not confirm any closed ports. All scanned ports were reported as filtered.

A filtered result generally means that Nmap could not determine whether the port was open or closed because expected network responses were not received or were blocked. This can occur when firewalls, packet filters, or network conditions prevent probes from reaching the destination or their responses from returning.

The output does not prove that every service on the destination is disabled.

**Web-port scan**

The separate Nmap scan reported:

PORT    STATE    SERVICE REASON      VERSION
80/tcp  filtered http    no-response
443/tcp filtered https   no-response

The service labels indicate the conventional services associated with those port numbers. They do not confirm that an HTTP or HTTPS service was successfully identified on the target.

No service versions were identified.

**Evidence files:**
- results/nmap_top100.nmap
- results/nmap_top100.xml
- results/nmap_top100.gnmap
- results/nmap_web_services.nmap
- results/nmap_web_services.xml
- results/nmap_web_services.gnmap

### 6.3 Web Technology Fingerprinting

**Tool:** WhatWeb 0.6.4

WhatWeb was run repeatedly against the target URL.

The recorded output included execution-expired errors and one connection-refused error involving port 80.

The extended-timeout attempt also failed, despite the use of increased connection and read timeouts.

| Item                      | Finding                          |
|---------------------------|----------------------------------|
| WhatWeb version           | 0.6.4                            |
| Successful page retrieval | Not confirmed                    |
| Web server identification | Not confirmed                    |
| Framework identification  | Not confirmed                    |
| CMS identification        | Not confirmed                    |
| Technology fingerprint    | Not obtained                     |
| Main limitation           | Connection failures and timeouts |

Since WhatWeb did not successfully retrieve a usable response, the assessment cannot reliably identify the target's web technologies from the supplied output.

The absence of a fingerprint must not be interpreted as evidence that the website does not use a web server, programming language, or framework.

**Evidence file:** results/whatweb.txt

### 6.4 HTTP and Netcat Checks

**curl observations**

curl timed out while attempting to contact the target over HTTP and HTTPS.

The HTTP status summary recorded status 000, no remote IP, and HTTP version 0. The saved HTTP header file was empty when inspected.

**Netcat observation**

The Netcat command failed with:

nc: getaddrinfo for host "testphp.vulnweb.com" port 80: Name or service not known

This attempt failed before establishing the intended TCP connection. No HTTP status line, response headers, or response body were obtained.

**TLS observation**

The OpenSSL connection attempt did not complete within the 12-second timeout. No TLS handshake or certificate was confirmed.

**Comparison**

The curl and Netcat attempts both failed to retrieve HTTP response information. However, the specific errors differed: curl timed out while Netcat reported a hostname-resolution failure. These are separate observations and should not be treated as identical failures.

Because no HTTP response or TLS session was obtained, the project cannot confirm the target's response headers, HTTP server banner, certificate details, or negotiated TLS version.

**Evidence files:**
- results/http_headers.txt
- results/http_status.txt
- results/https_check.txt
- results/netcat_http.txt
- results/tls_check.txt

## 7. Security Observations

### 7.1 External visibility

The target's IPv4 address and reverse DNS name were visible through the recorded DNS and Nmap results. This demonstrates that hostname resolution and some basic external information gathering were possible.

The availability of DNS information alone does not indicate a security vulnerability.

### 7.2 Filtered TCP ports

Nmap reported all 100 scanned TCP ports as filtered. Ports 80 and 443 were also filtered in the separate web-port scan.

From a defensive perspective, filtering can be consistent with a firewall or network policy that restricts unsolicited connections. However, the scan results do not identify the specific filtering mechanism or prove that the filtering was intentional.

### 7.3 Web service identification

WhatWeb could not obtain a usable response. Therefore, no web technology, software version, or application component was confirmed by this assessment.

The inability to fingerprint the website is an assessment limitation, not a confirmed vulnerability.

### 7.4 HTTP and TLS availability

The attempted HTTP and HTTPS connections did not complete successfully in the recorded tests. It is therefore not possible to determine from these results whether the target's web services were temporarily unavailable, blocked from the testing environment, or affected by another connectivity issue.

No conclusion about insecure TLS configuration, missing HTTPS support, or insecure HTTP behavior should be drawn solely from these failed connections.

### 7.5 Evidence-based reporting

This assessment highlights the importance of separating observed facts from assumptions. A filtered port is not necessarily closed, a timeout is not proof of a vulnerability, and a missing fingerprint is not proof that a technology is absent.

Further verification from an authorized and functioning assessment path would be required before making stronger claims.

## 8. Limitations

The following limitations affected the assessment:

1. **Limited port scope:** The completed general Nmap scan examined the top 100 TCP ports rather than every possible TCP port or any UDP ports.

2. **Filtered ports:** All 100 scanned TCP ports were reported as filtered, so the scan could not confirm open or closed states for those ports.

3. **Web connectivity failures:** HTTP and HTTPS requests timed out, preventing retrieval of web response data.

4. **Unsuccessful fingerprinting:** WhatWeb did not produce a successful technology fingerprint.

5. **Netcat resolution error:** The recorded Netcat attempt failed during hostname resolution and did not establish an HTTP connection.

6. **Unsuccessful TLS check:** The OpenSSL attempt timed out before a TLS session could be verified.

7. **Incomplete response evidence:** The saved HTTP header file was empty, and no HTTP response status line or TLS certificate was confirmed.

8. **Potential environmental restrictions:** Firewall rules, network routing, DNS behavior, transient outages, or restrictions between the virtual machine and the target may have influenced the results. The available evidence does not identify the precise cause.

9. **Time-specific results:** The observations describe the target's apparent behavior during the recorded assessment attempts. Network and service states can change over time.

10. **No vulnerability confirmation:** No successful exploitation, authenticated testing, or independent verification of a vulnerability was performed.

These limitations mean that the project can document DNS resolution and observed network filtering, but it cannot provide a complete inventory of the target's active web technologies or services.

## 9. Recommendations

The following recommendations are intended for the authorized owner or administrator of the assessed environment. They are defensive recommendations, not claims that a particular vulnerability was discovered.

### 9.1 Verify firewall and network policies

Review firewall, security group, and network access-control rules to ensure that only intended services are reachable from the public internet.

If the observed filtering is intentional, confirm that it aligns with the documented security policy.

### 9.2 Maintain an accurate external service inventory

Maintain an inventory of public-facing services and their business purposes. Disable unnecessary services and restrict administrative interfaces to trusted networks.

The current scan did not identify any open ports, so no specific exposed service can be recommended for removal based on these results alone.

### 9.3 Maintain supported and patched software

Keep operating systems, web servers, application frameworks, and dependencies updated with supported security fixes.

The assessment did not identify a specific software version, so it cannot establish whether the target is running vulnerable or outdated software.

### 9.4 Configure HTTP and HTTPS appropriately

Where web services are intended to be publicly accessible, verify HTTP and HTTPS availability from an authorized monitoring location. Configure HTTPS and TLS securely, and redirect HTTP traffic to HTTPS where appropriate.

The failed connection attempts do not prove that HTTPS is missing or incorrectly configured.

### 9.5 Minimize unnecessary information disclosure

Review server banners and response headers to avoid disclosing unnecessary implementation details. Such information should be evaluated only after a successful response has been obtained.

### 9.6 Investigate connectivity from an authorized environment

If the target is expected to be reachable, verify the service status, routing, firewall rules, and access restrictions from an approved network. Compare results with server-side logs where available.

Repeat the assessment only within the permitted scope and use genuine outputs to establish whether the earlier timeouts were transient.

### 9.7 Preserve and compare evidence

Retain dated scan outputs and repeat authorized checks periodically. Comparing verified results helps distinguish genuine changes in exposure from temporary connectivity problems.

## 10. Summary

This project performed basic external reconnaissance and TCP service enumeration against testphp.vulnweb.com using Kali Linux.

DNS resolution succeeded during the recorded checks, identifying the IPv4 address 44.228.249.3. The reverse DNS name was also recorded.

The completed Nmap scan examined the top 100 TCP ports and reported all 100 as filtered with no response. A separate scan found ports 80 and 443 filtered as well.

HTTP and HTTPS connection attempts using curl timed out. WhatWeb failed to retrieve the target, Netcat reported a hostname-resolution error during its HTTP request attempt, and the OpenSSL TLS connection attempt timed out.

As a result, the assessment did not confirm any open TCP ports, web technology fingerprints, HTTP response headers, server versions, or TLS certificate details.

The main outcome was a documented set of DNS, port-state, and connectivity observations. The project also demonstrated why scan results must be interpreted cautiously: filtering and connection failures limit what can be concluded about the target's services.

## 11. Screenshots and Evidence 

Inserted genuine screenshots captured from the actual Kali terminal.


## 12. Conclusion

The project provided practical experience with the initial stages of an external cybersecurity assessment. It covered DNS resolution, TCP port scanning, web technology fingerprinting, HTTP connectivity checks, direct TCP connection attempts, and TLS connection testing.

The most important confirmed result was that the target hostname resolved to an IPv4 address, while the completed Nmap scans reported the tested TCP ports as filtered. Attempts to retrieve HTTP responses, identify web technologies, and establish a TLS session were unsuccessful in the supplied evidence.

The project demonstrated the importance of using multiple tools, saving outputs, interpreting errors accurately, and distinguishing confirmed observations from assumptions. It also showed that the absence of a response can restrict the conclusions an assessor can draw.

Further authorized testing from a verified network path would be necessary to establish the target's actual service availability and obtain reliable application-level information.

The findings in this report are limited to the commands and outputs provided. No vulnerability is claimed without supporting evidence.

## Appendix A: Commands Used

The following commands are based on the supplied terminal history. Commands that were attempted are documented even when they did not produce successful results.

### A.1 Project setup and environment

```bash
mkdir -p ~/SHAURYA_GUPTA/Project_1_Network_Reconnaissance/{report,screenshots,results,logs}
cd ~/SHAURYA_GUPTA/Project_1_Network_Reconnaissance
pwd
date
cat /etc/os-release
whoami
```

### A.2 Tool installation and verification

```bash
sudo apt update
sudo apt install -y nmap whatweb netcat-openbsd curl dnsutils openssl

nmap --version
whatweb --version
nc -h
curl --version
dig -v
```

### A.3 DNS resolution

```bash
dig testphp.vulnweb.com A +noall +answer
dig testphp.vulnweb.com AAAA +noall +answer
dig testphp.vulnweb.com CNAME +noall +answer
getent ahosts testphp.vulnweb.com
getent hosts testphp.vulnweb.com
```

### A.4 HTTP and HTTPS connectivity

```bash
curl --connect-timeout 10 --max-time 20 -sS \
  -D results/http_headers.txt -o /dev/null \
  http://testphp.vulnweb.com/

curl --connect-timeout 10 --max-time 20 -sS \
  -o /dev/null \
  -w 'HTTP status: %{http_code}\nRemote IP: %{remote_ip}\nHTTP version: %{http_version}\n' \
  http://testphp.vulnweb.com/

curl --connect-timeout 10 --max-time 20 -sS -I \
  https://testphp.vulnweb.com/
```

### A.5 Nmap enumeration

```bash
nmap -sT -Pn -T2 --top-ports 100 --version-light --reason \
  -oA results/nmap_top100 testphp.vulnweb.com

nmap -sT -Pn -T2 -sV --version-light --reason -p 80,443 \
  -oA results/nmap_web_services testphp.vulnweb.com
```

### A.6 WhatWeb fingerprinting

```bash
whatweb --color=never http://testphp.vulnweb.com/

whatweb --color=never --open-timeout=20 --read-timeout=30 \
  --max-threads=1 http://testphp.vulnweb.com/
```

### A.7 Netcat HTTP request

```bash
printf 'HEAD / HTTP/1.1\r\nHost: testphp.vulnweb.com\r\nConnection: close\r\n\r\n' |
  nc -nv -w 8 testphp.vulnweb.com 80
```

### A.8 TLS connection attempt

```bash
timeout 12 openssl s_client \
  -connect testphp.vulnweb.com:443 \
  -servername testphp.vulnweb.com \
  -brief </dev/null
```

### A.9 Save command history

The original command log was saved as:

logs/commands.txt

The log contains the commands recorded in the terminal history. Additional troubleshooting commands should be appended if executed later.

## Appendix B: Evidence Files

The following files were referenced or created by the commands shown in the supplied terminal history.

### B.1 DNS and environment

- results/target.txt
- results/environment.txt
- results/dns.txt
- results/dns_ipv6.txt
- results/dns_cname.txt
- results/resolved_addresses.txt

### B.2 HTTP, HTTPS, and connection checks

- results/http_headers.txt
- results/http_status.txt
- results/https_check.txt
- results/whatweb.txt
- results/netcat_http.txt
- results/tls_check.txt

### B.3 Nmap outputs

- results/nmap_top100.nmap
- results/nmap_top100.xml
- results/nmap_top100.gnmap
- results/nmap_web_services.nmap
- results/nmap_web_services.xml
- results/nmap_web_services.gnmap

Nmap's -oA option requests output in normal, XML, and grepable formats. Confirm that each file exists before including it in the final submission.

### B.4 Command log

- logs/commands.txt

### B.5 Screenshots

The screenshots directory was created.

### B.6 Final verification

Before submission, verify the actual directory contents using:

```bash
find results logs screenshots -maxdepth 2 -type f -printf '%p\n' | sort
```

---

**End of Report**

Prepared by: SHAURYA GUPTA  
Domain: Cyber Security  
Project: Network Reconnaissance and Service Enumeration  
Organization: CyberSNova


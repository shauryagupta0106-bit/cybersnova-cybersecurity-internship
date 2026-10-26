# CyberSNova Beginner Project 1

## Network Reconnaissance & Service Enumeration

### Intern Details
- Name: SHAURYA GUPTA
- Email: shauryagupta0106@gmail.com
- Reference Number: CSN-OC-OL-26194
- Registration Number: CSN-OC-2026-194
- Domain: Cyber Security
- Project Level: Beginner
- Internship Month/Year: October 2026

### Objective
Perform basic network reconnaissance and service enumeration on
the training website specified in the CyberSNova task list.

### Target
- Hostname: testphp.vulnweb.com
- URL: http://testphp.vulnweb.com/

### Tools
- Nmap: TCP port and service enumeration
- WhatWeb: Web technology fingerprinting
- Netcat: TCP connectivity and HTTP request testing
- curl: HTTP response inspection

### Project Structure
- results/: Raw command outputs and scan evidence
- screenshots/: Genuine screenshots from the Kali environment
- notes/: Project information and evidence inventory
- commands_used.txt: Commands and their purposes
- Reconnaissance_Report.md: Detailed project report

### Methodology
1. Verify network connectivity and DNS resolution.
2. Check the web application's HTTP response.
3. Scan the top 100 TCP ports using Nmap.
4. Inspect detected web services where appropriate.
5. Fingerprint the web application using WhatWeb.
6. Test HTTP communication with Netcat.
7. Save the results and compare findings across tools.

### Authorization and Scope
This exercise is limited to the practice target provided in the
CyberSNova task list. No unrelated hosts are included.

### Evidence
Refer to the results/ directory for command outputs and the
screenshots/ directory for visual evidence.

### Limitations
The results represent the services visible during the scan.
A limited TCP scan cannot establish that every port or service
on the target has been discovered. Fingerprinting can be incomplete.


---
title: "Three-Day Cybersecurity Challenge"
date: 2024-04-29
slug: "three-day-cybersecurity-challenge"
type: post
categories: ["projects"]
---

## Project Overview

I completed a three-day penetration testing challenge spanning web applications, Linux containers, and Windows systems. Each day, I targeted a different environment, with complexity escalating across all three.

---

## Day 1: Web Application Vulnerability Exploitation

I started with a web application known for layered vulnerabilities. The goal was to exploit as many security flaws as possible.

- **XSS Reflected:** I crafted payloads that split the word "script" into parts to bypass input sanitization.
- **Local File Inclusion (LFI):** I exploited an unvalidated file include parameter in a form to execute server-side scripts and access the server's filesystem.
- **SQL Injection:** I injected SQL queries through the login form to bypass authentication.
- **Sensitive Data Exposure:** HTTP response headers and HTML source exposed sensitive data, including server version strings and internal paths.
- **Command Injection:** I executed server-side commands through vulnerable application inputs.

## Flags Captured

- Reflected XSS on Welcome.php and Memory-Planner.php
- Stored XSS in comments.php
- Local file inclusion in multiple parameters of Memory-Planner.php
- SQL injection and sensitive data exposure in Login.php
- Directory traversal on Disclaimer.php

---

## Day 2: Attacking Linux Containers

I shifted focus to Linux servers hosting Apache Tomcat, Struts, and CGI services, using OSINT, network scanning, and known CVE exploits.

- **Open Source Data Exposure:** I pulled sensitive data from WHOIS records, DNS TXT entries, and SSL certificate details that led to further access points.
- **CVE Exploitation:** I used Metasploit to exploit Apache Tomcat's CVE-2017-12617 and Shellshock in CGI scripts.
- **Service Enumeration:** I used Nmap to identify exposed services and then escalated privileges through misconfigured SUID binaries.

## Flags Captured

- Flags from WHOIS data, DNS TXT records, and SSL/TLS certificates
- Multiple flags from exploiting Apache Struts and Tomcat vulnerabilities
- Flags from exploiting misconfigured SUID binaries for privilege escalation

---

## Day 3: Windows Network and Domain Controller Penetration

The final day targeted a Windows network — a Windows 10 PC and a Domain Controller.

- **Credential Harvesting:** I retrieved and cracked passwords from an exposed GitHub repository and captured network traffic.
- **Windows Service Exploitation:** I used known exploits for SLMail and took advantage of misconfigurations in scheduled tasks and service permissions.
- **Domain Controller Attack:** I performed a DCSync attack to extract the NTLM hash of the Domain Administrator.

## Flags Captured

- Accessed sensitive files through credentials found on GitHub
- Exploited SLMail to access email services
- Captured the Domain Administrator's NTLM hash via DCSync

---

## Conclusion

Day 3 was the hardest. Moving from the initial GitHub credential find to the DCSync attack required chaining together techniques across different parts of the Windows environment, and each step had to work cleanly or the chain broke. If I ran this again, I'd spend more time on Day 1 automating payload generation — I wrote too many XSS payloads by hand.

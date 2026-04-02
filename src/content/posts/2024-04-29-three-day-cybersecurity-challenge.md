---
title: "Three-Day Cybersecurity Challenge"
date: 2024-04-29
slug: "three-day-cybersecurity-challenge"
type: post
categories: ["projects"]
---

## Project Overview

An intensive three-day penetration testing challenge across web applications, Linux containers, and Windows systems. Each day targeted a different environment with escalating complexity.

---

## Day 1: Web Application Vulnerability Exploitation

I started with a web application known for layered vulnerabilities. The goal was to exploit as many security flaws as possible.

- **XSS Reflected:** I crafted payloads that split the word "script" into parts to bypass input sanitization and execute malicious scripts.
- **Local File Inclusion (LFI):** I exploited weak file handling in forms to execute server-side scripts and access the server's filesystem.
- **SQL Injection:** I injected SQL queries through the login form to bypass authentication.
- **Sensitive Data Exposure:** I found sensitive data leaked through HTTP response headers and HTML content, which opened further attack vectors.
- **Command Injection:** I executed server-side commands through vulnerable application inputs.

## Flags Captured

- Reflected XSS on Welcome.php and Memory-Planner.php
- Stored XSS in comments.php
- Local file inclusion in multiple parameters of Memory-Planner.php
- SQL injection and sensitive data exposure in Login.php
- Directory traversal on Disclaimer.php

---

## Day 2: Attacking Linux Containers

I shifted focus to Linux servers hosting various services, using OSINT, network scanning, and known CVE exploits.

- **Open Source Data Exposure:** I pulled sensitive data from publicly accessible records that led to further access points.
- **CVE Exploitation:** I used Metasploit to exploit Apache Tomcat's CVE-2017-12617 and Shellshock in CGI scripts.
- **Service Enumeration:** I identified critical services through port scanning, exploited them to gain access, and escalated privileges.

## Flags Captured

- Flags from WHOIS data, DNS TXT records, and SSL/TLS certificates
- Multiple flags from exploiting Apache Struts and Tomcat vulnerabilities
- Additional flags from service exploitation and network scanning

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

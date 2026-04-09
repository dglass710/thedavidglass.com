---
title: "Three-Day Cybersecurity Challenge"
date: 2024-04-29
slug: "three-day-cybersecurity-challenge"
type: post
description: "Web apps, Linux containers, Windows domain. Three days, ending with a DCSync on the Domain Controller."
categories: ["projects"]
---

## Overview

A three-day penetration testing challenge that escalated from web application exploits to a DCSync attack on a Windows Domain Controller. Each day targeted a different environment — web applications, Linux containers, Windows systems — with complexity building across all three.

---

## Day 1: Web Application Vulnerability Exploitation

I started with a web application known for layered vulnerabilities. The goal was to exploit as many security flaws as possible.

- **XSS:** I crafted payloads splitting "script" across concatenated strings to bypass input sanitization on Welcome.php and Memory-Planner.php. Also found stored XSS in comments.php.
- **Local File Inclusion:** I exploited unvalidated file include parameters in Memory-Planner.php to execute server-side scripts and traverse the filesystem to Disclaimer.php.
- **SQL Injection:** I injected queries through Login.php to bypass authentication and extract database contents.
- **Sensitive Data Exposure:** I pulled server version strings and internal paths from HTTP response headers and HTML source.
- **Command Injection:** I executed server-side commands through vulnerable application inputs.

The web vulnerabilities were textbook, but the combination mattered — each finding opened a new angle on the same application.

---

## Day 2: Attacking Linux Containers

I shifted focus to Linux servers hosting Apache Tomcat, Struts, and CGI services.

- **Open Source Data Exposure:** I pulled sensitive data from WHOIS records, DNS TXT entries, and SSL certificate details that led to further access points.
- **CVE Exploitation:** I used Metasploit to exploit Apache Tomcat's CVE-2017-12617 and Shellshock (CVE-2014-6271) in CGI scripts, capturing flags from both.
- **Privilege Escalation:** After gaining initial access via Nmap service enumeration, I escalated through misconfigured SUID binaries to root.

---

## Day 3: Windows Network and Domain Controller Penetration

The final day targeted a Windows 10 PC and a Domain Controller — the most complex environment.

- **Credential Harvesting:** I found cleartext credentials in an exposed GitHub repository and captured additional passwords from network traffic.
- **Windows Service Exploitation:** I exploited SLMail to access email services and leveraged misconfigurations in scheduled tasks and service permissions.
- **Domain Controller Attack:** I chained the harvested credentials into a DCSync attack, extracting the Domain Administrator's NTLM hash.

---

## Conclusion

Day 3 was the hardest. Moving from the initial GitHub credential find to the DCSync attack required chaining together techniques across different parts of the Windows environment, and each step had to work cleanly or the chain broke. If I ran this again, I'd spend more time on Day 1 automating payload generation — I wrote too many XSS payloads by hand.

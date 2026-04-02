---
title: "Web Application Penetration Testing Project"
date: 2024-04-04
slug: "web-application-penetration-testing-project"
type: post
categories: ["projects"]
---

## Project Overview

Over three days, I conducted penetration tests on demo web applications and competed in a Capture the Flag event structured as a scavenger hunt. The goal was to identify common security vulnerabilities, exploit them, and develop mitigation strategies.

## Tools and Techniques Used

- **Burp Suite:** Mapped the application's attack surface and automated attacks.
- **OWASP ZAP:** Ran automated scans and used it as a man-in-the-middle proxy to inspect and manipulate traffic.
- **Manual Testing:** Wrote specialized scripts and performed manual SQL injections to find issues automated tools missed.
- **Linux Command Line:** Navigated file systems, examined permissions, and processed output with `ls`, `grep`, and `cat`.
- **John the Ripper:** Cracked passwords from a shadow file.
- **Bash Scripting:** Debugged and executed scripts to automate tasks and manipulate data.
- **Network Tools:** Used `netstat` and custom scripts to analyze network traffic and extract data from logs.

## Process and Key Findings

1. **Reconnaissance and Scanning:** I started by mapping the application's architecture and attack surface with automated scans, then followed up with manual testing to dig deeper.
2. **Exploitation:** I found SQL injection vulnerabilities that exposed database contents, XSS vulnerabilities that could execute scripts in a user's session, and CSRF vulnerabilities that could force forged requests.
3. **Flag Discovery and Password Recovery:** I used `ls -Ra` and `cat` to uncover hidden files and passwords, cracked user passwords with John the Ripper and a wordlist, and analyzed log files to extract unique IP addresses.
4. **Reporting and Mitigation:** I documented all findings with descriptions, exploitation steps, and screenshots. Recommendations included implementing prepared statements and parameterized queries, sanitizing user inputs, enforcing SameSite cookies, and adding anti-CSRF tokens to forms.

## Outcome

The pen test produced a full vulnerability report with actionable fixes. The CTF pushed me to chain techniques together under time pressure — cracking a hacker's account password, using log analysis to unlock encrypted files, and hunting through file permissions for hidden flags. The trickiest part was the log analysis step, where IP addresses buried in log entries turned out to be zip file passwords.

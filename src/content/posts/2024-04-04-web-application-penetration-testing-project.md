---
title: "Web Application Penetration Testing Project"
date: 2024-04-04
slug: "web-application-penetration-testing-project"
type: post
categories: ["projects"]
---

## Project Overview

Over three days, I conducted penetration tests on demo web applications and competed in a related Capture the Flag event. The CTF was built into the same engagement — after testing the web apps, the scavenger hunt challenged me to chain together the techniques I'd used (password cracking, log analysis, file permission hunting) under time pressure. The goal across both was to identify common security vulnerabilities, exploit them, and recommend fixes.

## Process and Key Findings

1. **Reconnaissance and Scanning:** I used Burp Suite to map the application's attack surface and OWASP ZAP as a man-in-the-middle proxy to inspect and manipulate traffic. I followed up with manual testing — writing specialized scripts and performing manual SQL injections to find issues the automated scans missed.
2. **Exploitation:** I found SQL injection vulnerabilities that exposed database contents, XSS vulnerabilities that could execute scripts in a user's session, and CSRF vulnerabilities that could force forged requests.
3. **Flag Discovery and Password Recovery:** I used `ls -Ra` and `cat` to uncover hidden files and passwords, cracked user passwords with John the Ripper and a wordlist, and analyzed log files with `grep` and custom scripts to extract unique IP addresses.
4. **Reporting and Mitigation:** I documented all findings with descriptions, exploitation steps, and screenshots. I recommended prepared statements and parameterized queries, input sanitization, SameSite cookies, and anti-CSRF tokens on forms.

## Outcome

What set this engagement apart was the CTF component — it forced me to apply each finding offensively, not just document it. Cracking a hacker's account password, using log analysis to unlock encrypted files, and hunting through file permissions for hidden flags all reinforced the vulnerabilities I'd reported. The log analysis step was the hardest because nothing flagged the IP addresses as significant — I had to recognize that addresses buried in log entries were doubling as zip file passwords, a connection that no scanner would catch.

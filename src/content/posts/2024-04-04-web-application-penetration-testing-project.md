---
title: "Web Application Penetration Testing Project"
date: 2024-04-04
slug: "web-application-penetration-testing-project"
type: post
categories: ["projects"]
---

## **Project Overview:**

In a series three days, I focused on identifying and exploiting vulnerabilities in web applications to enhance their security posture. These projects included a Web Application Penetration Testing exercise and a thrilling Capture the Flag (CTF) event, both designed to test and refine my skills in penetration testing, vulnerability analysis, and problem-solving.

## **Objectives**:

The primary goal was to conduct thorough penetration tests on demo web applications, identify common security vulnerabilities, and develop mitigation strategies. The CTF event added a competitive edge by structuring the challenge as a scavenger hunt, where each flag represented a unique security challenge within a simulated environment.

## **Tools and Techniques Used:**

- **Burp Suite:** Employed for mapping out the application’s attack surface and automating attacks.
- **OWASP ZAP (Zed Attack Proxy):** Used for automated scanning and as a man-in-the-middle proxy to inspect and manipulate traffic.
- **Manual Testing Techniques:** Included the use of specialized scripts and manual SQL injections to uncover deeper security issues not always caught by automated tools.
- **Linux Command Line:** Extensively used for navigating file systems, examining file permissions, and manipulating output with commands like ls, grep, and cat.
- **John the Ripper:** Employed for cracking passwords stored in a shadow file, showcasing the ability to recover credentials from compromised systems.
- **Bash Scripting:** Debugged and executed bash scripts to automate tasks and manipulate data, demonstrating proficiency in scripting for security tasks.
- **Network Tools:** Utilized netstat and custom scripts to analyze network traffic and extract valuable data from logs.

## **Process and Key Findings:**

1. **Reconnaissance and Scanning:** Began by understanding the application’s architecture and mapping out the attack surface. Used automated tools to perform initial scans, followed by manual testing to dig deeper into potential vulnerabilities.
2. **Exploitation:** Attempted to exploit discovered vulnerabilities to understand their potential impact. Identified SQL injection vulnerabilities allowing access to database contents, uncovered XSS vulnerabilities that could be used to execute malicious scripts in the context of a user’s session, and detected CSRF vulnerabilities that could trick a user into submitting a forged request.
3. **Flag Discovery and Password Recovery:** Used commands like `ls -Ra` and `cat` to uncover hidden files and passwords, leveraged a list of passwords and the john tool to crack user passwords, and analyzed log files to determine unique IP addresses.
4. **Reporting and Mitigation:** Documented all findings with detailed descriptions of the vulnerabilities, the steps taken to exploit them, and screenshots where applicable. Proposed technical and procedural changes to mitigate the risks, including implementing prepared statements and parameterized queries, sanitizing all user inputs, enforcing SameSite cookies, and including anti-CSRF tokens in forms.

- Identified SQL injection vulnerabilities allowing access to database contents.
- Detected CSRF vulnerabilities that could trick a user into submitting a forged request.

## **Challenges and Solutions:**

- **User Impersonation and Privilege Escalation:** Identified a famous hacker’s user account, cracked the password using john, and accessed the account.
- **Log Analysis and Cryptographic Discovery:** Analyzed log files to determine unique IP addresses which served as passwords to unlock zipped files.
- **Directory and File Permissions Analysis:** Searched through user profiles to find files with specific permissions that contained flags.

## **Outcome:**

These projects demonstrated my ability to systematically identify and exploit web application vulnerabilities, providing me with the opportunity to practice drafting practical, actionable recommendations for security improvements. The CTF event was particularly instrumental in enhancing my practical cybersecurity skills, testing my ability to think critically under pressure and apply cybersecurity concepts in a hands-on environment.

## **Conclusion:**

Through this project, I have developed a keen eye for security in web applications, a crucial skill in today’s digital landscape where web applications are frequent targets of cyber attacks. This experience is a testament to my capabilities in navigating complex security environments and ensuring robust defense mechanisms are in place.

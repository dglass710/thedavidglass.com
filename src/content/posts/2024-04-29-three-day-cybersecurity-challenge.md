---
title: "Three-Day Cybersecurity Challenge"
date: 2024-04-29
slug: "three-day-cybersecurity-challenge"
type: post
categories: ["projects"]
---

## **Project Overview:**

In an intensive three-day cybersecurity challenge, I engaged in a series of complex penetration tests across various platforms including web applications, Linux containers, and Windows systems. This multi-faceted challenge was designed to test and enhance my capabilities in identifying and exploiting vulnerabilities across different environments.

---

## **Day 1: Web Application Vulnerability Exploitation**

I started with a web application known for its complex vulnerabilities. My goal was to exploit as many security flaws as possible, ranging from reflected and stored XSS to sensitive data exposure and command injection.

- **XSS Reflected:** Utilized creative payloads to bypass input sanitization, for instance, breaking the word "script" into parts to avoid detection and execute malicious scripts.
- **Local File Inclusion (LFI):** Exploited weak file handling in forms to execute server-side scripts, allowing deeper access to the server's filesystem.
- **SQL Injection:** Successfully exploited the login form by injecting SQL queries that bypass authentication mechanisms.
- **Sensitive Data Exposure:** Identified sensitive data leaked through HTTP response headers and HTML content, which provided further attack vectors.
- **Command Injection:** Demonstrated command injection to execute server-side commands through vulnerable application inputs.

## **Significant Flags Captured:**

- Reflected XSS on Welcome.php and Memory-Planner.php.
- Stored XSS in comments.php.
- Local file inclusion in multiple parameters of Memory-Planner.php.
- SQL injection and sensitive data exposure in Login.php.
- Directory traversal on Disclaimer.php.

---

## **Day 2: Attacking Linux Containers**

On the second day, I shifted my focus to attacking Linux servers hosting various services. I employed different techniques such as open source intelligence, network scanning, and exploiting well-known vulnerabilities like Shellshock and Apache Struts.

- **Open Source Data Exposure:** Retrieved sensitive data from publicly accessible records which led to further access points.
- **Exploitation of CVEs:** Used Metasploit to exploit vulnerabilities like Apache Tomcat's CVE-2017-12617 and Shellshock in CGI scripts.
- **Service Enumeration and Exploitation:** Identified critical services through port scanning and exploited them to gain unauthorized access and escalate privileges.

## **Significant Flags Captured:**

- Flags from WHOIS data, DNS TXT records, and SSL/TLS certificates.
- Multiple flags from exploiting Apache Struts and Tomcat vulnerabilities.
- Gained crucial information through service exploitation and network scanning.

---

## **Day 3: Windows Network and Domain Controller Penetration**

The final day was dedicated to penetrating a Windows network, including a Windows 10 PC and a Domain Controller. This involved cracking hashes, exploiting misconfigured services, and using lateral movement techniques.

- **Credential Harvesting and Cracking:** Retrieved and cracked passwords from exposed GitHub repositories and network traffic.
- **Exploitation of Windows Services:** Used known exploits for services like SLMail and took advantage of misconfigurations in scheduled tasks and service permissions.
- **Domain Controller Attack:** Utilized techniques like DCSync to extract credentials from the Domain Controller, a key to accessing broader network resources.

## **Significant Flags Captured:**

- Accessed sensitive files through credentials obtained from GitHub.
- Exploited SLMail vulnerabilities to access email services.
- Performed DCSync attack to capture the NTLM hash of the Domain Administrator.

---

## **Conclusion:**

This three-day cybersecurity challenge was a comprehensive exercise that pushed the limits of my technical knowledge and practical skills. It underscored the importance of a proactive approach in cybersecurity, emphasizing continuous learning and adaptation to new threats. Through this challenge, I not only improved my offensive security techniques but also prepared myself for real-world cybersecurity challenges, ensuring robust defense mechanisms are effectively in place.

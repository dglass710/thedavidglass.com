---
title: "Web Application Pentest with Capture the Flag"
date: 2024-04-04
slug: "web-application-penetration-testing-project"
type: post
description: "Pentest plus CTF — every finding had to be exploited offensively, not just documented."
categories: ["projects"]
---

## Overview

A three-day engagement combining penetration testing of demo web applications with a Capture the Flag event built on top of the same vulnerabilities. The CTF forced me to apply each finding offensively under time pressure — cracking passwords, chaining log analysis into file access, hunting through permissions for hidden data.

## Reconnaissance and Exploitation

I used Burp Suite to map the application's attack surface and OWASP ZAP as a MITM proxy to inspect and manipulate traffic. Manual testing — writing scripts and performing SQL injections by hand — caught issues the automated scans missed.

I found SQL injection through the login form, reflected and stored XSS in multiple endpoints, and CSRF vulnerabilities in form submissions. The SQLi exposed database contents directly; the XSS and CSRF opened session-level attack paths.

## The CTF Component

The scavenger hunt was where the findings mattered. I used `ls -Ra` and `cat` to uncover hidden files and passwords across the filesystem, cracked user passwords with John the Ripper against a targeted wordlist, and analyzed application logs with `grep` and custom scripts to extract unique IP addresses.

The log analysis was the hardest step. Nothing flagged the IP addresses as significant — I had to recognize that addresses buried in log entries were doubling as zip file passwords. That connection was purely manual; no scanner would catch it. The zip files contained credentials for the next stage, so missing this link would have stalled the entire chain.

## Outcome

What set this engagement apart was the feedback loop between testing and exploitation. Documenting a vulnerability is one thing; actually weaponizing it under time pressure reveals whether you understood it. The CTF made every finding actionable — each reported vulnerability became a tool for the next step.

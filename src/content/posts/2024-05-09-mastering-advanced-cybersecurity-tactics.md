---
title: "Mastering Advanced Cybersecurity Tactics"
date: 2024-05-09
slug: "mastering-advanced-cybersecurity-tactics"
type: post
categories: ["Bootcamp Blog"]
---

## Lateral Movement and Pivoting

This month covered what happens after an attacker gets initial access. We practiced lateral movement — using a compromised host to reach other machines on the network. In one lab, I pivoted from a web server with a known vulnerability to an internal database server that wasn't directly exposed. Process migration let us move our session from an unstable process to a more persistent one, making detection harder.

## Web Application Exploitation

We also targeted web applications directly: SQL injection, cross-site scripting, and directory traversal against intentionally vulnerable apps. The SQL injection exercises connected well with my development background — I've written parameterized queries for years, but seeing what happens when you don't was a useful reminder of why that discipline matters.

## SIEM and Splunk

The second half of the month pivoted to defense. We set up Splunk as a SIEM, ingested log data from multiple sources, and wrote correlation rules to detect suspicious patterns. I built alerts for brute-force login attempts and unusual outbound traffic. The dashboard work — mapping attack origins geographically, tracking alert volume over time — turned raw log data into something actionable.

## Takeaway

The hardest part of this month was context-switching between offensive and defensive mindsets in the same week. But that's also what made it valuable — writing an exploit on Tuesday and then building a detection rule for it on Thursday connects the two sides in a way that studying them separately doesn't.

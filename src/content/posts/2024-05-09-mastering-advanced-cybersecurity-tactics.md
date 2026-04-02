---
title: "Lateral Movement, Web Exploits, and Building Detections in Splunk"
date: 2024-05-09
slug: "lateral-movement-web-exploits-splunk-detections"
type: post
categories: ["Bootcamp Blog"]
---

## Lateral Movement and Pivoting

This month I focused on what happens after initial access. I practiced lateral movement — using a compromised host to reach other machines on the network. In one lab, I pivoted from a web server with a known vulnerability to an internal database server that wasn't directly exposed. Process migration let me move my session from an unstable process to a more persistent one, making detection harder.

## Web Application Exploitation

I also targeted web applications directly: SQL injection, cross-site scripting, and directory traversal against intentionally vulnerable apps. The SQL injection exercises tied directly to my development background — I've written parameterized queries for years, but watching unparameterized queries get exploited in real time showed exactly what happens when you don't parameterize.

## SIEM and Splunk

Halfway through, I switched to defense. I set up Splunk as a SIEM, ingested log data from multiple sources, and wrote correlation rules to detect suspicious patterns. I built alerts for brute-force login attempts and unusual outbound traffic. The dashboard work — mapping attack origins geographically, tracking alert volume over time — gave me a single view that surfaced which hosts were under active attack. It also made it easy to spot how fast alerts were escalating.

## Offense Informs Defense

The hardest part of this month was context-switching between offensive and defensive mindsets in the same week. Writing an exploit on Tuesday and then building a detection rule for it on Thursday connects the two sides — I recognized my own exploit signature in the Splunk logs and watched the detection rule I'd written fire against traffic I'd just generated.

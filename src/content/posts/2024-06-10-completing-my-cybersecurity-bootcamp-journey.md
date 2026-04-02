---
title: "Completing My Cybersecurity Bootcamp Journey"
date: 2024-06-10
slug: "completing-my-cybersecurity-bootcamp-journey"
type: post
categories: ["Bootcamp Blog"]
---

## Advanced Log Analysis

The final month started with deeper Splunk work — writing complex SPL queries to correlate events across multiple log sources, building dashboards that surface real patterns instead of noise, and tuning alert thresholds to reduce false positives. The difference between a useful SIEM and a firehose of alerts comes down to how well you understand what normal looks like.

## Final Project: Have I Been Pwned? Offline

My capstone project was "Have I Been Pwned? Offline" — a Dockerized tool that checks passwords against known breach databases without sending anything over the network. I built it with Python and Bash, using the Have I Been Pwned dataset stored locally. The tool hashes the input, searches the local database, and reports whether the password has appeared in a breach. Docker made it portable — anyone can pull the image and run it without installing dependencies. The project combined several things I'd learned across the program: scripting, containerization, and thinking about privacy as a design constraint.

## Looking Back

Six months ago I could build software but had a shallow understanding of how to attack or defend it. The progression — from Linux fundamentals to packet analysis to Metasploit to building my own security tool — filled that gap. The most valuable thing wasn't any single tool or technique. It was learning to look at systems the way an adversary does: what's exposed, what's misconfigured, what's forgotten.

I'm now focused on roles that combine development and security. The code I write is different because of this program — not just more secure, but designed with threat models in mind from the start.

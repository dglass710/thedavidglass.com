---
title: "Splunk Tuning, a Capstone Build, and What Changed"
date: 2024-06-10
slug: "completing-my-cybersecurity-bootcamp-journey"
type: post
categories: ["Bootcamp Blog"]
---

## Advanced Log Analysis

The final month started with deeper Splunk work — writing SPL queries to correlate Windows Event Logs with firewall data, building dashboards, and tuning alert thresholds. One exercise had me chasing a spike in failed login events that turned out to be a misconfigured service account retrying every thirty seconds. Filtering that out cut our alert volume in half and made actual brute-force attempts visible.

## Final Project: Have I Been Pwned? Offline

My capstone project was "Have I Been Pwned? Offline" — a Dockerized tool that checks passwords against known breach databases without sending anything over the network. I built it with Python and Bash, using the Have I Been Pwned dataset stored locally. Docker made it portable — anyone can pull the image and run it without installing dependencies. The hardest part was keeping the container image small while bundling a multi-gigabyte hash database; I ended up using a multi-stage build and mounting the dataset as a volume instead of baking it into the image.

## Looking Back

Six months ago I could build software but couldn't tell you what a reverse shell looked like in a packet capture or why a misconfigured S3 bucket matters more than a missing input check. The progression — from Linux fundamentals to packet analysis to Metasploit to building my own security tool — filled that gap.

I'm now focused on roles that combine development and security. The code I write is different because of this program — for example, I now default to hashing on the client side and never send raw credentials over the wire, something I wouldn't have thought twice about before.

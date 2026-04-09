---
title: "From Greeter to Security Lead: Building Custom AI Agents"
date: 2026-04-08
slug: "custom-agent-toolkit"
type: post
description: "Six agents, six lessons. From a hello-world greeter to a multi-agent security scanning team."
categories: ["projects"]
---

## Overview

I built a curriculum that teaches Claude Code custom agents through six progressive lessons, each introducing one concept with a working agent to test against intentionally vulnerable code. The final lesson coordinates multiple specialized agents into a security scanning team — a lead agent that delegates to a vulnerability scanner and a code reviewer, then synthesizes their findings.

## How Agents Work

A Claude Code agent is a markdown file with YAML frontmatter. The frontmatter controls the agent's model, available tools, effort level, and which other agents it can spawn. The markdown body is the system prompt — the agent's instructions, role definition, and output format.

This is the entire interface. No framework, no SDK, no API wrappers. The configuration surface is small, which makes the design decisions more deliberate.

## The Progression

**Lesson 1 — Agent Basics:** A greeter agent. Minimal frontmatter, simple prompt. Demonstrates the file structure and invocation.

**Lesson 2 — Tool Restrictions:** A read-only code reader. The `tools` field whitelists only `Read`, `Grep`, and `Glob` — the agent physically cannot edit files, run commands, or access the network, even if instructed to. Least privilege enforced at the system level, not the prompt level.

**Lesson 3 — Model Routing:** A quick summarizer running on Haiku instead of Opus. Simple tasks don't need expensive models. This is a real production pattern — route cheap work to cheap models and save the heavy reasoning for deep analysis.

**Lesson 4 — Structured Output:** A vulnerability scanner with an exact output template. Instead of parsing arbitrary LLM output, the prompt defines the format: finding number, file path, severity, OWASP category, evidence, and remediation. The agent follows the template consistently without needing output validation or correction loops.

**Lesson 5 — Full-Featured Agent:** A security reviewer combining everything — Opus model, high effort, full tool access, structured output, and detailed analysis instructions. The production-grade version.

**Lesson 6 — Multi-Agent Coordination:** A security lead that spawns the vulnerability scanner and the security reviewer as specialists. The lead coordinates, deduplicates overlapping findings, reconciles severity disagreements, and produces a unified report. The `spawn-agents` field prevents infinite delegation chains.

## The Hard Part

Multi-agent coordination without duplication. Both the scanner and the reviewer find some of the same vulnerabilities. The lead agent needs to recognize duplicates, merge findings, and when the two specialists disagree on severity, pick the higher rating. The instructions explicitly tell the lead not to re-scan the code itself — its value is in synthesis, not redundant analysis.

The `spawn-agents` allowlist solves the infinite delegation problem. Without it, a lead could spawn agents that spawn agents recursively, burning context and cost. The whitelist enforces a flat hierarchy: the lead can spawn specific specialists, and those specialists can't spawn anyone.

## Test Targets

I included three intentionally vulnerable files for testing: a Flask route with SQL injection, a Python module with hardcoded API keys and database credentials, and an Express.js endpoint with reflected and stored XSS. Each file targets a different vulnerability category so the agents have realistic material to scan.

## What I'd Change

I'd add a lesson on persistent memory — agents that remember findings across sessions and track whether previously reported vulnerabilities have been fixed. I'd also build a more complex test target with chained vulnerabilities (like an auth bypass that enables SQL injection) to test whether the multi-agent system can identify attack chains, not just individual findings.

Source code: [github.com/dglass710/Custom_Agent_Toolkit](https://github.com/dglass710/Custom_Agent_Toolkit)

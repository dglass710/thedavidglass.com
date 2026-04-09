---
title: "Nine Vulnerabilities, Six Scanners, One Pipeline"
date: 2026-04-08
slug: "devsecops-security-pipeline"
type: post
description: "Six security scanners, sequential gates. Vulnerable code never reaches the container."
categories: ["projects"]
---

## Overview

I built a GitHub Actions pipeline that integrates six security scanners — TruffleHog, Semgrep, Trivy (twice), and Checkov — into a CI/CD workflow with hard gates. If any scan fails, the Docker build is skipped. Vulnerable code never gets containerized.

To demonstrate how each tool works, I started with a Flask application containing nine intentional vulnerabilities: hardcoded secrets, SQL injection, server-side template injection, outdated dependencies with known CVEs, and a Dockerfile running as root with no healthcheck. Then I fixed every finding and documented the remediation.

## The Architecture

The pipeline runs seven jobs in a specific order:

1. **TruffleHog** — scans for hardcoded secrets using pattern matching and entropy analysis
2. **Semgrep** — static analysis with taint tracking (follows untrusted input from source to sink)
3. **Trivy (filesystem)** — checks `requirements.txt` for dependencies with known CVEs

These three run in parallel. All must pass before the next stage:

4. **Docker Build** — builds the image and saves it as an artifact
5. **Trivy (image)** — scans the built container for OS-level vulnerabilities
6. **Checkov** — validates Dockerfile against infrastructure best practices
7. **Summary** — reports overall pass/fail

The `needs:` dependency in GitHub Actions enforces the gate. If any upstream job fails, downstream jobs are automatically skipped.

## What the Scanners Found

**TruffleHog** caught two hardcoded secrets — an API key and a database password — using both regex patterns and Shannon entropy analysis. I replaced them with `os.environ.get()` calls.

**Semgrep** found eleven issues. Four SQL injections where `request.get_json()` flowed through `.format()` into `conn.execute()` — Semgrep traced the taint across multiple lines. Three template injection vulnerabilities where user input went directly into `render_template_string()`. Two instances of debug mode enabled. I fixed the SQL injection with parameterized queries, replaced template rendering with `jsonify()`, and moved debug mode to an environment variable.

**Trivy** found three HIGH CVEs in the pinned dependencies — Flask 2.2.0 had a session cookie disclosure bug, and Werkzeug 2.2.0 had both a DoS vulnerability and a remote code execution flaw in the debugger. Upgrading to Flask 3.1.3 and Werkzeug 3.1.8 cleared all three.

**Checkov** flagged the Dockerfile for running as root and missing a healthcheck. I added a non-root user and a healthcheck using Python's `urllib` instead of `curl` (which isn't available in `python:slim`).

## Defense in Depth

Multiple scanners intentionally overlap. Running as root was caught by both Semgrep and Checkov from different angles. If one scanner has a false negative, another catches it.

The two-stage Trivy scanning matters because dependency security and container security are different problems. Your Python packages might be clean while the base OS image ships with a vulnerable `glibc`. I upgraded from `python:3.9-slim` to `python:3.12-slim` to fix inherited OS-level vulnerabilities that the dependency scan alone wouldn't catch.

Semgrep also flagged `host="0.0.0.0"` as exposing the server publicly. Inside a Docker container, binding to all interfaces is required — it's not a real finding. I suppressed it with an inline `# nosemgrep` comment and a justification, which is the right way to handle a false positive: documented, intentional, and auditable.

## What I'd Change

I'd add severity-based routing — CRITICAL findings should block the pipeline, HIGH should require manual approval, and MEDIUM should be warnings only. Right now everything fails hard, which is correct for a demo but noisy in a real workflow. I'd also add SARIF output from each scanner and aggregate results into a single GitHub Security tab view.

Source code: [github.com/dglass710/security-pipeline](https://github.com/dglass710/security-pipeline)

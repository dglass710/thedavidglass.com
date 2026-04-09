---
title: "Building a Resume Tailoring Engine"
date: 2025-03-15
slug: "resume-tailoring-engine"
type: post
description: "40+ applications. 5 minutes each instead of 30. AI curates my own content — doesn't write a word."
categories: ["projects"]
---

## Overview

I've used this tool to generate tailored resumes for over 40 job applications. What used to take 25-30 minutes of manual editing per application now takes under 5 — paste the job posting, review the AI's selections, adjust if needed, and export.

The tool treats resume content as structured data. I maintain a single database of my qualifications — objectives, projects, competencies, experience — and a two-stage AI pipeline selects and orders the most relevant items for each role. The AI doesn't write anything. It curates.

## The Problem

Every job posting emphasizes different skills. A security engineer role cares about penetration testing; a systems administrator role cares about infrastructure and automation. The underlying experience is the same — the presentation should differ.

Manually editing a resume for each application is tedious and error-prone. Generative AI produces content that isn't mine. I wanted curation, not composition: write all the content once, accurately, and let the tool handle selection and ordering.

## Architecture

The app has four components:

- **GUI (ResumeBuilder.py)** — Tkinter interface with drag-and-drop reordering for every section. Persists state to JSON in the user's Documents folder, keeping data separate from the application bundle.

- **Document engine (generator.py)** — Outputs Harvard-template formatted .docx files using python-docx. Automatically detects and hyperlinks emails, phone numbers, and URLs.

- **Selection logic (ai_selector.py)** — Calls OpenRouter's API in two stages: first to select the most relevant items, then to reorder them by relevance. Shared between the GUI and CLI.

- **CLI tool (resume_cli.py)** — Generates tailored resumes from the command line without opening the GUI.

## AI Selection Pipeline

The AI's role is narrow.

**Stage 1 — Selection:** The model chooses one objective (from several pre-written options), two to four technical projects, and all relevant core competencies. The prompt provides numbered lists and expects a JSON response with index arrays. I validate that responses don't select too many or too few items.

**Stage 2 — Reordering:** The selected items go back to the model with instructions to reorder by relevance to the job posting. The model returns permuted index arrays. Validation confirms every original index is present — no additions, no deletions, just reordering. If reordering fails after three attempts, the original order is preserved.

Both stages default to Claude via OpenRouter.

## Data Management

The app uses three levels of stored data:

1. **default_data.json** — A template bundled with the app, used for resets
2. **data.json** — The user's working data, persisted between sessions
3. **role_specific_data/*.json** — Pre-configured variants optimized for specific roles (cybersecurity analyst, systems administrator, security engineer)

When running as a bundled executable (via PyInstaller), the app reads from and writes to a persistent folder in the user's Documents directory. This keeps user data intact across application updates.

## Cross-Platform Build

A single build script detects the operating system and invokes PyInstaller with the appropriate configuration — creating a .app bundle on macOS or a .exe on Windows. The script handles icon embedding, data file bundling, and post-build cleanup. On macOS, it optionally installs the built application to /Applications.

## What I'd Change

I'd swap Tkinter for Electron or Tauri — drag-and-drop required custom event binding that would be trivial in a modern framework. The AI selection could also benefit from few-shot examples in the prompt. The selection pipeline works reliably with Claude, but some smaller models on OpenRouter return inconsistent JSON formatting despite the structured output instructions.

Source code: [github.com/dglass710/resume-generator](https://github.com/dglass710/resume-generator)

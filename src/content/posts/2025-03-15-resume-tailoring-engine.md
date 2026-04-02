---
title: "Building a Resume Tailoring Engine"
date: 2025-03-15
slug: "resume-tailoring-engine"
type: post
categories: ["projects"]
---

## Overview

I built a desktop application that treats resume content as structured data. Instead of rewriting a resume for every job application, I maintain a single database of my real qualifications — objectives, projects, competencies, experience — and the tool selects and orders the most relevant items for each role. The AI doesn't write anything. It curates.

The project includes a full Tkinter GUI, a command-line interface, cross-platform builds, and a two-stage AI selection pipeline that calls OpenRouter's API.

## The Problem

Every job posting emphasizes different skills. A security engineer role cares about penetration testing and vulnerability assessment. A systems administrator role cares about infrastructure and automation. The underlying experience is the same — the presentation should differ.

Manually editing a resume for each application is tedious and error-prone. Copy-pasting from ChatGPT produces generic content that doesn't reflect real experience. I wanted something in between: a system where I write all the content once, accurately, and the tool handles the selection and ordering.

## Architecture

The application has four main components:

- **ResumeBuilder.py** — An 1,800-line Tkinter GUI with drag-and-drop reordering for every section. Users can edit personal info, objectives, education, experience, projects, certifications, and competencies through structured editors. The GUI persists state to a JSON file in the user's Documents folder, keeping data separate from the application bundle.

- **generator.py** — A document engine using python-docx that outputs Harvard-template formatted .docx files. It automatically detects and hyperlinks emails, phone numbers, and URLs in the content. Formatting is consistent: Times New Roman 11pt, 0.5-inch margins, two-column date/title layouts for experience entries.

- **ai_selector.py** — The selection logic, shared between the GUI and CLI. It calls OpenRouter's API in two stages: first to select which items are most relevant to a given job posting, then to reorder the selected items by relevance. Temperature is set to 0.2 for deterministic results. The module includes retry logic with three attempts, JSON validation, and markdown fence stripping for model responses that wrap JSON in code blocks.

- **resume_cli.py** — A command-line tool for generating tailored resumes without opening the GUI. Takes an output path, job posting text, and optional model override.

## AI Selection Pipeline

The AI's role is narrow and well-defined. Given a job posting, it receives the full list of my pre-written content and returns indices — nothing more.

**Stage 1 — Selection:** The model chooses one objective (from several pre-written options), two to four technical projects, and all relevant core competencies. The prompt provides numbered lists and expects a JSON response with index arrays. Validation rejects responses that select too many or too few items.

**Stage 2 — Reordering:** The selected items are sent back to the model with instructions to reorder by relevance to the job posting. The model returns permuted index arrays. Validation confirms every original index is present — no additions, no deletions, just reordering. If reordering fails after three attempts, the original order is preserved.

Both stages use the same model (defaulting to Claude) via OpenRouter, which means I can swap models by changing a single configuration value.

## Data Management

Resume content is stored as JSON with a specific schema. Each section has a title, content array, and UI metadata (window dimensions, font size). Content types vary by section — string arrays for competencies and certifications, nested arrays for education, dictionaries with subtitle/date/details for professional experience.

The application maintains three data layers:

1. **default_data.json** — A clean template bundled with the app, used for resets
2. **data.json** — The user's working data, persisted between sessions
3. **role_specific_data/*.json** — Pre-configured variants optimized for specific roles (cybersecurity analyst, systems administrator, security engineer)

When running as a bundled executable (via PyInstaller), the app reads from and writes to a persistent folder in the user's Documents directory. This keeps user data intact across application updates.

## Cross-Platform Build

A single build script detects the operating system and invokes PyInstaller with the appropriate configuration — creating a .app bundle on macOS or a .exe on Windows. The script handles icon embedding, data file bundling, and post-build cleanup. On macOS, it optionally installs the built application to /Applications and generates SHA-256 checksums for verification.

## What I'd Improve

The GUI works but Tkinter shows its age — particularly with drag-and-drop, which required custom event binding for reorder operations that would be trivial in a modern framework. If I rebuilt this, I'd use Electron or Tauri for the frontend, keeping the Python backend for document generation and AI integration.

The AI selection could also benefit from few-shot examples in the prompt. Currently it works well with Claude but I've seen inconsistent JSON formatting from some smaller models on OpenRouter, despite the structured output instructions.

Source code: [github.com/dglass710/resume-generator](https://github.com/dglass710/resume-generator)

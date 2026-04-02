---
title: "Have I Been Pwned? Offline Project"
date: 2024-06-10
slug: "have-i-been-pwned-offline-project"
type: post
categories: ["projects"]
---

## Project Overview

For my final project in the Northwestern Cybersecurity Bootcamp, I built an offline version of the "Have I Been Pwned?" service. The tool lets users check if their passwords have been compromised without an internet connection.

## Project Components

### Docker Integration

- **Purpose:** Containerize the application for portability.
- **Details:** The Dockerfile defines the environment, copies project files, and installs dependencies. Anyone with Docker installed can run the tool.

### Database Management

- **Purpose:** Efficiently query a large dataset of compromised passwords.
- **Details:** An SQLite database stores SHA-1 hashes of compromised passwords. A Python script (`txt_to_db.py`) processes the raw data into 4096 tables for fast lookups.

### Automated Updates

- **Purpose:** Keep the database current with the latest breached passwords.
- **Details:** The `Update` script automates the full pipeline:
  - Downloads the latest compromised password list from Have I Been Pwned
  - Converts raw data into an SQLite database organized across 4096 tables
  - Builds a new Docker image using the latest `bitnami/minideb` base image (which includes recent security patches)
  - Pushes the updated image to Docker Hub

### User Interaction

- **Purpose:** Provide a simple interface for password checking.
- **Details:** Users interact via a command-line interface inside the Docker container. They enter passwords and get immediate feedback on whether those passwords appear in known breaches.

## Security and Efficiency

The core security advantage is that users enter their passwords inside a container with no network connection. The container can be deleted after use. This trades disk space (downloading the full database) for the privacy risk of typing passwords into yet another website. Docker isolation also prevents interference with the host system.

## Outcome

The tool works end-to-end: download, build, query, update. The trickiest design decision was splitting the database into 4096 tables — I used the first three hex characters of each SHA-1 hash as the table name, which kept query times fast even with hundreds of millions of entries.

## Conclusion

The project is open source and documented on my [GitHub repository](https://github.com/dglass710/pwned). The automated update pipeline was the most useful piece to build — it means the tool stays current without manual work.

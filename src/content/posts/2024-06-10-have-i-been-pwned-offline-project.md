---
title: "Have I Been Pwned? Offline Project"
date: 2024-06-10
slug: "have-i-been-pwned-offline-project"
type: post
categories: ["projects"]
---

## Project Overview

For my final project in the Northwestern Cybersecurity Bootcamp, I built an offline version of the "Have I Been Pwned?" service. The tool lets users check if their passwords have been compromised without an internet connection.

## Privacy Model

The core tradeoff is disk space for privacy. Instead of typing passwords into a website, users enter them inside a container with no network connection. The container can be deleted after use. Docker isolation also prevents interference with the host system.

## How It Works

I containerized the app with Docker so anyone can run it without configuring dependencies. The Dockerfile defines the environment, copies project files, and installs everything needed.

The password data lives in an SQLite database. A Python script (`txt_to_db.py`) processes the raw Have I Been Pwned dataset into the database, splitting hashes across tables by their first three hex characters for fast lookups.

The CLI inside the container takes a password, hashes it, and reports whether it appears in known breaches.

I wrote an `Update` script that automates the full pipeline: downloading the latest compromised password list, converting it into SQLite, building a fresh Docker image on the latest `bitnami/minideb` base (which includes recent security patches), and pushing the updated image to Docker Hub.

## Outcome

The trickiest design decision was splitting the database into 4096 tables — I used the first three hex characters of each SHA-1 hash as the table name, which kept query times under 100ms even across 900+ million entries in a ~30 GB database. The project proved that offline breach checking is practical for personal use: a single machine can store and query the full HIBP dataset with no cloud dependencies, and the automated update pipeline keeps it current without manual work.

## Conclusion

The biggest insight from this project was that trading disk space for privacy is a worthwhile bargain — downloading a multi-gigabyte password database is a small price for never sending your passwords over a network. The project is open source on [GitHub](https://github.com/dglass710/pwned).

---
title: "Have I Been Pwned? Offline Project"
date: 2024-06-10
slug: "have-i-been-pwned-offline-project"
type: post
description: "900M+ passwords in a 30 GB SQLite database. Sub-100ms queries. Zero network exposure."
categories: ["projects"]
---

## Overview

I built an offline version of the Have I Been Pwned service — a Dockerized tool that checks passwords against 900+ million known breach entries without ever touching a network. Queries run under 100ms against a ~30 GB SQLite database.

## How It Works

The app runs inside a Docker container with no network connection. Users enter a password, the tool hashes it with SHA-1, and reports whether it appears in known breaches. The container can be deleted after use. Docker isolation prevents interference with the host system.

A Python script (`txt_to_db.py`) processes the raw HIBP dataset into SQLite. An `Update` script automates the full pipeline: downloading the latest compromised password list, converting it into the database, building a Docker image on the latest `bitnami/minideb` base, and pushing the updated image to Docker Hub.

## The Sharding Decision

The hardest design choice was how to index 900+ million SHA-1 hashes for fast lookup. A single table was too slow. I split the database into 4096 tables, using the first three hex characters of each hash as the table name. This turned every lookup into a two-step process: compute the table name from the hash prefix, then query a table small enough for SQLite to scan efficiently.

The tradeoff was straightforward — more tables means more complexity in the schema but dramatically faster reads. At 4096 tables, each one holds roughly 220,000 entries. Query times dropped below 100ms.

## Privacy Model

The core tradeoff is disk space for privacy. Instead of typing passwords into a website, users enter them inside a network-isolated container. A multi-gigabyte database is a reasonable price for never sending credentials over a wire.

## What I'd Change

I'd revisit SQLite at this scale. It works, but the 30 GB database file pushes its practical limits — vacuum operations are slow, and the initial import takes hours. A key-value store like RocksDB or LevelDB might handle the hash-prefix sharding more naturally. I'd also add batch checking so users could test multiple passwords in one session.

The project is open source on [GitHub](https://github.com/dglass710/pwned).

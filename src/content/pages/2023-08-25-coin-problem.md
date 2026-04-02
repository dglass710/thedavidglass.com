---
title: "Coin Problem"
date: 2023-08-25
slug: "coin-problem"
type: page
---

## The Frobenius Coin Problem Database

The Frobenius Coin Problem asks: given a set of coin denominations, what is the largest amount that cannot be represented? That value is the Frobenius number. This research focuses on Frobenius symmetry — the distribution of attainable and unattainable values between 0 and the Frobenius number. For two denominations, symmetry always holds. For three, it’s predictable. For four, no general solution exists. The database contains data on roughly 60 million four-denomination sets (all denominations <= 200), classifying each as symmetric or not.

## About This Resource

The ZIP archive (`resources.zip`) includes an SQLite3 database (`frob.db`) and a Python script (`query.py`) for querying it. The database interfaces directly with Python for efficient data access.

## Docker Access

If you’d rather skip local setup, the database is available via a Docker container on [Google Cloud Console](https://console.cloud.google.com/). You can interact with the database directly through Cloud Shell — no downloads required.

## How to Access the Database

Run this command in the Google Cloud Console’s Cloud Shell:

```
docker run -it --rm dglass710/frobenius-quad-database
```

![](/images/Screenshot-2024-01-24-at-4.00.09 PM.png)

The Cloud Shell Button is located on the top right of the screen

## Understanding the Command

- `docker run`: Runs a Docker container.
- `-it`: Interactive mode with a terminal.
- `--rm`: Removes the container on exit — no residual data on your system.
- `dglass710/frobenius-quad-database`: The Docker image containing the database.

This launches an interactive session with the database. No additional setup needed.

## Google Cloud Console’s Cloud Shell

[Google Cloud Console’s](https://console.cloud.google.com/) Cloud Shell is a browser-based command-line environment with Docker pre-installed. It provides persistent storage and pre-authenticated access to your resources — no local setup required.

## Explore and Contribute

Explore the database and contribute to the research. If you find patterns or have insights into four-denomination Frobenius symmetry, they’re welcome.

---
title: "Scavenger Hunt: A Cybersecurity Adventure"
date: 2024-01-31
slug: "cybersecu"
type: post
categories: ["projects"]
---

## Project Overview

A CTF-style scavenger hunt with eight flags hidden across a Linux system. Each task built on the previous one — credentials found in one step unlocked the next user account, creating a chain from unprivileged user to root.

## Discovering Hidden Files

The first task was finding hidden files containing user passwords. I listed all files in the home directory and uncovered `~/Desktop/.flag_1` and `~/Desktop/.pass_list.txt`. The first flag came with a message: find seven more.

## Cracking a Hacker's Password

I needed to identify a famous hacker's user account and crack the password. The account belonged to Kevin Mitnick. I ran John the Ripper against the password list and cracked it (`trustno1`). Logging in revealed the second flag.

## Analyzing Log Files for Clues

The third task involved a log file and a zip file in Mitnick's account. I inspected `/var/log/mitnick.log` and noticed unique IP addresses at the start of each line. Using those addresses as the zip file password unlocked credentials for the babbage user.

## Identifying Files with Specific Permissions

In babbage's home directory, I needed to find files with specific read and execute permissions. I searched for files with permissions `-r-------x` and found that the file named `stallman` contained the password (`computer`) for the stallman user.

## Debugging a Bash Script

The fifth challenge was a faulty bash script in stallman's home directory. The script `flag5.sh` had syntax errors. I fixed them and ran the script to reveal the next flag.

## Inspecting Custom Aliases

For the sixth flag, I checked the sysadmin user's `.bashrc` file and found a custom alias that contained the flag. Running it produced flag six.

## Gaining Root Access

I discovered that sysadmin could run `less` with root privileges via sudo. I dropped to a root shell from within `less`, changed the root password, and logged in as root. Flag seven.

## Gathering and Cracking All Flags

The final task: gather all previously found flags, format them as username:password pairs, and crack them with John the Ripper. I searched the full system for flag files, compiled them, and cracked the passwords to reveal the eighth and final flag.

## Outcome

The most satisfying step was the log analysis — figuring out that IP addresses were the zip password required a mental leap that no tool could automate. The chain from one user account to the next made this feel like a realistic privilege escalation scenario rather than a set of isolated puzzles.

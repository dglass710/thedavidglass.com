---
title: "CTF Walkthrough: User to Root in Eight Flags"
date: 2024-01-31
slug: "linux-ctf-privilege-escalation"
type: post
description: "Eight flags. Unprivileged user to root. Each account unlocked the next."
categories: ["projects"]
---

## Overview

A privilege escalation chain across a Linux system — eight flags, eight user accounts, unprivileged user to root. Each flag's credentials unlocked the next account, and each account required a different technique to crack.

## Discovering Hidden Files

The first task was finding hidden files containing user passwords. I listed all files in the home directory and uncovered `~/Desktop/.flag_1` and `~/Desktop/.pass_list.txt`. The first flag came with a message: find seven more.

## Cracking a Hacker's Password

I needed to identify a famous hacker's user account and crack the password. The account belonged to Kevin Mitnick. I ran John the Ripper against `/etc/shadow` using the password list as a wordlist and cracked it (`trustno1`). Logging in revealed the second flag.

## Analyzing Log Files for Clues

The third task involved a log file and a zip file in Mitnick's account. I inspected `/var/log/mitnick.log` and noticed unique IP addresses at the start of each line. Using those addresses as the zip file password unlocked credentials for the babbage user.

## Identifying Files with Specific Permissions

In babbage's home directory, I needed to find files with specific read and execute permissions. I searched for files with permissions `-r-------x` and found that the file named `stallman` contained the password (`computer`) for the stallman user.

## Script Debugging and Shell Config Inspection

Flags five and six tested filesystem attention. The fifth was a faulty bash script (`flag5.sh`) in stallman's home directory with missing quotes around variable expansions and a broken conditional test — fixing the syntax and running it produced the flag. The sixth was hidden in the sysadmin user's `.bashrc` as a custom alias that echoed the flag when invoked.

## Gaining Root Access

I discovered that sysadmin could run `less` with root privileges via sudo. I dropped to a root shell from within `less`, changed the root password, and logged in as root. Flag seven.

## Gathering and Cracking All Flags

The final task: gather all previously found flags, format them as username:password pairs, and crack them with John the Ripper. I ran `find / -name "flag*"` to locate every flag file on the system, compiled them into a single list of MD5-hashed passwords, and ran John against them to reveal the eighth and final flag.

## Outcome

The hardest step was the `less`-to-root escalation — it required knowing that interactive pagers can spawn shells, something I had read about but never tried. If I did this again I would script the enumeration earlier; I manually checked permissions and configs for the first several accounts before writing a reusable checklist, which cost time I could have saved.

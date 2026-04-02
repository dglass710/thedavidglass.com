---
title: "CTF Walkthrough: User to Root in Eight Flags"
date: 2024-01-31
slug: "linux-ctf-privilege-escalation"
type: post
categories: ["projects"]
---

## Project Overview

A CTF-style scavenger hunt with eight flags hidden across a Linux system. Each task built on the previous one — credentials from one step unlocked the next user account, chaining from unprivileged user to root.

## Discovering Hidden Files

The first task was finding hidden files containing user passwords. I listed all files in the home directory and uncovered `~/Desktop/.flag_1` and `~/Desktop/.pass_list.txt`. The first flag came with a message: find seven more.

## Cracking a Hacker's Password

I needed to identify a famous hacker's user account and crack the password. The account belonged to Kevin Mitnick. I ran John the Ripper against `/etc/shadow` using the password list as a wordlist and cracked it (`trustno1`). Logging in revealed the second flag.

## Analyzing Log Files for Clues

The third task involved a log file and a zip file in Mitnick's account. I inspected `/var/log/mitnick.log` and noticed unique IP addresses at the start of each line. Using those addresses as the zip file password unlocked credentials for the babbage user.

## Identifying Files with Specific Permissions

In babbage's home directory, I needed to find files with specific read and execute permissions. I searched for files with permissions `-r-------x` and found that the file named `stallman` contained the password (`computer`) for the stallman user.

## Filesystem Attention to Detail

Flags five and six tested attention to detail on the filesystem. The fifth was a faulty bash script (`flag5.sh`) in stallman's home directory with missing quotes around variable expansions and a broken conditional test — fixing the syntax and running it produced the flag. The sixth was hidden in the sysadmin user's `.bashrc` as a custom alias that echoed the flag when invoked, the kind of thing you only catch by inspecting shell configuration files on every account you access.

## Gaining Root Access

I discovered that sysadmin could run `less` with root privileges via sudo. I dropped to a root shell from within `less`, changed the root password, and logged in as root. Flag seven.

## Gathering and Cracking All Flags

The final task: gather all previously found flags, format them as username:password pairs, and crack them with John the Ripper. I ran `find / -name "flag*"` to locate every flag file on the system, compiled them into a single list of MD5-hashed passwords, and ran John against them to reveal the eighth and final flag.

## Outcome

Every flag required a different skill — file discovery, password cracking, log analysis, permission enumeration, script debugging, shell config inspection, and sudo abuse — but the privilege escalation chain is what tied them together. The hardest step was the `less`-to-root escalation because it required knowing that interactive pagers can spawn shells, something I had read about but never exploited hands-on. If I did this again I would script the enumeration earlier; I manually checked permissions and configs for the first several accounts before writing a reusable checklist, which cost time I could have saved.

---
title: "Scavenger Hunt: A Cybersecurity Adventure"
date: 2024-01-31
slug: "cybersecu"
type: post
categories: ["projects"]
---

## Project Overview:

Engaging in the scavenger hunt was an exhilarating experience that tested my cybersecurity problem-solving skills and penetration testing abilities. Each task was a puzzle that required ingenuity, patience, and a keen eye for detail, reflecting real-world scenarios that security professionals encounter.

## Discovering Hidden Files:

The first task was to find hidden files containing user passwords before they were hacked. By listing all files in the home directory, I uncovered `~/Desktop/.flag_1` and `~/Desktop/.pass_list.txt`. This initial discovery set the stage for the challenges to come and provided the first flag with the encouraging message to find seven more.

## **Cracking a Hacker's Password**:

Next, I needed to identify a famous hacker's user account and crack the password. Recognizing the hacker as Kevin Mitnick, I used John the Ripper and a password list to successfully crack his password (`trustno1`). Logging into Mitnick's account and finding the second flag was a thrilling moment, affirming my ability to use password-cracking tools effectively.

## **Analyzing Log Files for Clues**:

The third task involved finding a log file and a zip file related to the hacker. By inspecting the `/var/log/mitnick.log`, I noticed unique IP addresses at the beginning of each line. Using these addresses as the password for the zip file, I unlocked additional information, leading to credentials for the babbage user. This step underscored the importance of log analysis in uncovering hidden data.

## **Identifying Files with Specific Permissions**:

Switching to the babbage home directory, I needed to find files with specific read and execute permissions. I identified files with permissions `-r-------x` and discovered that the file named `stallman` contained the password (`computer`) for the stallman user. This task highlighted the value of understanding file permissions in security investigations.

## **Debugging a Bash Script**:

The fifth challenge was to find, debug, and run a faulty bash script. Located in stallman's home directory, the script `flag5.sh` had syntax errors that needed fixing. Correcting these errors and successfully running the script to reveal the next flag was a satisfying achievement, showcasing my scripting skills.

## **Inspecting Custom Aliases**:

For the sixth flag, I inspected the custom aliases of the sysadmin user. The `.bashrc` file in sysadmin's home directory contained an alias for the flag. Running this alias provided the sixth flag, demonstrating how system configurations can hide critical information.

## **Gaining Root Access**:

The seventh task required gaining a root shell by exploiting sudo permissions. I discovered that sysadmin could run `less` with root privileges. By dropping to a root shell from within `less`, I changed the root password and logged in as root, revealing the seventh flag. This step was a powerful reminder of the importance of managing sudo permissions carefully.

## **Gathering and Cracking All Flags**:

The final task was to gather all the previously found flags, format them as username:password pairs, and crack the final passwords using John the Ripper. This culmination of efforts involved searching for all flags on the system, compiling them into a file, and cracking the passwords to reveal the final flag. Completing this challenge was immensely rewarding, as it demonstrated my ability to piece together information from various sources and use advanced tools to solve complex problems.

## **Outcome**:

Participating in the scavenger hunt was an intensive and fulfilling exercise that significantly enhanced my practical skills in penetration testing, debugging, and log analysis. Each task provided valuable insights into real-world cybersecurity challenges, preparing me for future endeavors in the field. This experience reinforced my passion for cybersecurity and my commitment to continuous learning and improvement.

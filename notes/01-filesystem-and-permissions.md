# Linux Filesystem & Permissions Basics

## Overview
Linux uses a single unified filesystem tree, rooted at `/`, unlike Windows'
separate drive letters (C:\, D:\). Every file, folder, and even external
drives get mounted somewhere inside this one tree.

## The Filesystem Tree

| | | | |
bin etc home var usr
| |
username bin, lib...

## Key Directories

| Path | Purpose |
|---|---|
| `/home/username` | Personal user files |
| `/etc` | System configuration files |
| `/var/log` | System and application logs — critical for debugging |
| `/tmp` | Temporary files, cleared on reboot |
| `/usr/bin`, `/bin` | Installed program binaries |
| `/opt` | Third-party software installs |

## Core Navigation Commands

| Command | Purpose |
|---|---|
| `pwd` | Print current working directory |
| `ls` | List directory contents |
| `ls -la` | List all files (including hidden) in long format |
| `cd <path>` | Change directory |
| `cd ~` | Go to home directory |
| `cd ..` | Go up one directory level |

## Understanding `ls -la` Output

Example:

drwxr-xr-x 6 terminal staff 192 Jun 28 00:50 Documents

| Field | Meaning |
|---|---|
| `d` | File type: `d` = directory, `-` = file, `l` = symbolic link |
| `rwxr-xr-x` | Permissions (owner / group / others) |
| `6` | Link count |
| `terminal` | Owner |
| `staff` | Group |
| `192` | Size in bytes |
| `Jun 28 00:50` | Last modified timestamp |
| `Documents` | File/folder name |

## Special Entries
- `.` — refers to the current directory
- `..` — refers to the parent directory

## Permissions Fundamentals

Every file/folder has 3 permission types, for 3 categories of users:

**Permission types:**
- `r` (read) — view contents
- `w` (write) — modify contents
- `x` (execute) — run as program (file) OR enter the directory (folder)

**User categories:**
- Owner (the user who created the file)
- Group (a defined set of users)
- Others (everyone else on the system)

**Important distinction:** `x` means something different for files vs directories.
- On a **file**: `x` allows running it as a program/script.
- On a **directory**: `x` allows entering/accessing it — without it, you cannot `cd` into the folder even if you have read access.

## Real-World Relevance
- `/var/log` is the first place DevOps engineers check when debugging production issues.
- Understanding this tree structure applies identically across Linux servers, EC2 instances, and Docker containers.
- Permission misconfigurations are a leading cause of CI/CD pipeline failures (e.g., a script missing execute permission).

## Interview Questions
1. What is the difference between `/` and `/root`?
2. What does the `d` at the start of a permission string indicate, and what would a `-` mean instead?
3. What's the functional difference between `x` on a file versus `x` on a directory?
4. Why does every directory contain `.` and `..` entries?

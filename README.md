# Lets-Learn-Linux-1

## ParoCyber DevSecOps Bootcamp — Lab 01/Assignment

**Author:** Samuel Osadare  
**Date:** June 2026

A comprehensive hands-on lab focused on mastering the Linux command-line interface (CLI) from a DevSecOps perspective. This repository contains the completed tasks, terminal workflows, and security concepts covered during the assignment.

## Lab Overview
This lab covers core Linux administration and security practices required to safely navigate, manage, and audit production server environments. The objectives focus on understanding system architecture, file permissions, data streams, and vulnerability assessment (Kernel CVE tracking).

---

## Completed Tasks & Core Concepts

### Section 1: Orient Yourself
- [x] **System Orientation:** Executed `pwd`, `whoami`, and `hostnamectl` to establish environment context, verify active user privileges, and extract OS/kernel specifications (`Ubuntu 24.04 LTS`, `Kernel 6.8.0-35-generic`).
- [x] **Filesystem Hierarchy Standard (FHS) Audit:** Explored the root directory (`/`) and mapped the security implications of five core system directories (`/root`, `/home`, `/etc`, `/tmp`, `/bin`).
- [x] **Hidden Artifacts Discovery:** Utilized `ls -la` to expose hidden configuration files (`.bashrc`, `.profile`) and deconstructed Linux naming conventions.

### Section 2: Build the Environment
- [x] **Automated Directory Trees:** Leveraged advanced brace expansion with `mkdir -p` to efficiently build out nested project architectures (`~/projects/cyphercore/{configs,logs,reports}`).
- [x] **Bulk File Initialization:** Used `touch` to create empty baselines and analyzed how it modifies timestamps (`atime`/`mtime`) safely without altering data contents.
- [x] **Permission Deconstruction:** Decoded 10-character permission strings (e.g., `drwxr-xr-x`) to audit owner, group, and world access rights using human-readable formats (`ls -lh`).
- [x] **Structural Verification:** Utilized the `tree` utility versus recursive `ls -R` listings to visually audit the system layout for configuration drift.

### Section 3: Write and Read Files
- [x] **Stream Appending & Safe Redirects:** Simulated application logging using the append operator (`>>`) and analyzed the operational risks of accidental truncation via standard redirection (`>`).
- [x] **Log Stream Triage:** Used `cat` and `tac` to read file streams, highlighting why reverse-chronological reading (`tac`) is essential for rapid incident response on active live servers.
- [x] **Forensic Footprint Minimization:** Implemented multi-line string injections using **Heredocs (`cat << 'EOF'`)** to write templates directly through the shell, bypassing interactive text editors to keep disk artifacts clean.
- [x] **Memory-Efficient Pagination:** Compared `more` vs. `less` pagers to navigate multi-gigabyte files safely without triggering system RAM exhaustion.

### Section 4: Move and Clean Up
- [x] **Data Management Mechanics:** Executed `cp` and `mv` commands, detailing how the OS manipulates underlying disk storage blocks versus metadata records.
- [x] **Recursive Directory Destruction:** Evaluated the safe usage of `rmdir` on empty directories vs. the destructive risks of the force-recursive flag (`rm -rf`).

### Section 5: Links and Inodes
- [x] **Hard Link Persistence:** Created and verified hard links sharing identical inode pointers, demonstrating data survival and link decrement mechanics even after the primary pointer is removed.
- [x] **Symbolic Link Pipelines:** Built target-pointing symlinks and engineered broken/dangling link errors to simulate typical runtime failures found in automated CI/CD pipelines.

### Section 6: Data Streams and Redirection
- [x] **Stream Partitioning (STDOUT & STDERR):** Managed separate communication channels by filtering valid data outputs (`1>`) and system error logs (`2>`) independently, as well as combining them (`2>&1`).
- [x] **Dynamic Variable Expansion:** Configured unquoted vs. quoted heredoc blocks to safely control environmental shell variable expansions during automated report provisioning.

---

## Key DevSecOps Takeaways
* **Attack Surface Analysis:** Knowing how to quickly trace OS, kernel releases, and file system states is vital for tracking high-severity CVEs (e.g., Dirty Pipe, Dirty COW).
* **Forensic Awareness:** Prioritizing stream manipulation tools over interactive text editors keeps server changes clean and minimizes trace files left on volatile production storage.
* **Resilient Automation:** Standardizing operations around the Filesystem Hierarchy Standard (FHS) ensures automation tools (like Ansible or Logstash) execute predictably without crashing disk limits.

* Author: Samuel Osadare

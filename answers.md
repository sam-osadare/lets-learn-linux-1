# LetsLearnLinux1 - Episode 1

## Assignment Overview
This repository contains my Linux assignment/lab-1 completed as part of the ParoCyber DevSecOps Bootcamp.

## Tasks Completed

**Q1: Print your current directory, your username, and full OS and kernel details — three separate commands.**

![image](https://github.com/user-attachments/assets/855df528-e487-4722-a6d0-a6835d66b72e)

### What does each command tell you? Why does a DevSecOps engineer run these three things first when SSHing into an unknown server — especially regarding kernel CVEs?

#### Commands:
- **pwd:** It displays my exact/current path or directory.
- **whoami:** It displays the current user. This helps to ascertain the current user’s privilege level.
- **hostnamectl:** It provides the details of the operating system – OS, Kernel, and architecture.

#### DevSecOps Engineer:
- **pwd:** It helps identify the right directory before committing any system modifications. This prevents wrong modifications of sensitive files like `/etc` or root.
- **whoami:** To determine the access/privilege level of the current user. This way, I can determine if I need root access or not before committing any changes to the system.
- **hostnamectl:** By displaying the system’s OS, Kernel, and architecture information, I can easily, as a DevSecOps Engineer, identify any malicious system modifications and attack surface or known vulnerabilities.

---

**Q2: List the contents of the root directory — the very top of the Linux filesystem. Pick five directories from the output and explain each one's purpose. What is the Filesystem Hierarchy Standard (FHS) and why does it exist? What breaks on a shared production server without it?**

<img width="940" height="274" alt="image" src="https://github.com/user-attachments/assets/27c0c2d7-47ce-4f4d-9f16-cfd12d5d5869" />

**Command:** `ls -a /`

**Five directories and their purposes:**
* **/root** – it is the home directory for the root user. It is the highest directory in the Filesystem Hierarchy Standard (FHS).
* **/home** – it is the user’s personal directory. It represents the individual user’s directory in the system.
* **/etc** – this directory contains all the configuration files, such as shadow, passwd, and firewall settings or rules.
* **/tmp** – it is a directory for temporary files like `ssh-XXXXXX/`, `apt-extract-dummy`, `exploit.sh`, etc.
* **/bin** – it is a directory that contains executable programs/commands (standard users and admins) that are essential for the system booting and running in a single-user mode.

---

### What is the Filesystem Hierarchy Standard (FHS) and why does it exist?

**Filesystem Hierarchy Standard (FHS)**: This is a formal standard by the Linux Foundation that describes the layout and directories in Linux and Unix-like operating systems. It ensures that a uniformity is maintained for the structure and format of directories/files and where each of the files must reside in the system. 

FHS was developed and adopted as the standard for Linux to eradicate the menace of dumping files anywhere as deemed necessary by the creators before the mid-1990s. 

#### FHS addresses the three benefits:
* **Predictability**: With FHS, hardcoding file paths into their respective applications, like pointing a service to `/etc` for a setting/file, by the developers is made possible, while assuring that it will work on all Linux distributions.
* **Interoperability**: FHS protects the native operating system by addressing any breaking or overwriting concerns and problems during the third-party software installation on any compliant system.
* **Physical and Logical Isolation**: FHS aims to strengthen the effective demarcation, isolation, and categorization of the filesystem. It helps with the separation of files from one another, such as the user’s file in `/home` from system binaries in `/usr`, and the mounting of directories on the hard drives or partitions for optimised different workloads.

---

### What breaks on a shared production server without it?

A production server will in no doubt break if one the followings occur;

* **Disk Exhaustion and Storage Management problem**: When FHS is ignored in setting up storage and disk in an operational environment, and everything sits on the `/root` partition, a sudden spike in application logging or a user uploading massive files to a random directory can instantly fill the entire disk. Consequently, when the root partition hits 100% utilization, the kernel cannot write critical system files, causing the entire production server to crash and experience a Denial of Service (DoS).
* **Automated Patching and Configuration Management**: Automated tools are built to interact with folders (e.g., `/usr/bin`) in the system during security patching. But when the server has a faulty FHS and the appropriate folder is not accessible, the patching automation will fail, leading to the server being vulnerable to critical CVEs because automated security updates are silently missing their targets.

---

**Q3: List your home directory twice – once the default way, then again revealing hidden files with full details (permissions, ownership, size)**

**Since I am currently in my home directory, the following commands are used directly:**
* To list in a default way – no permissions, hidden files, etc included: `ls`
* To list with permissions, hidden files included: `ls -al`

![image](https://github.com/user-attachments/assets/ef416b6b-d0ed-4c30-9925-fb4e65d9a422)

#### What is different between the two outputs? What makes a file hidden in Linux — is it a special type? Name two real hidden files found in a home directory and explain what they do.

**Difference between the two outputs:**
* `ls`: displays only the visible files and directories in the home directory.
* `ls -al`: while `ls -l` displays all files, directories, and other details, `-a` includes all the hidden ones – special entries **`.`** (current directory) and **`..`** (parent directory), as well as `.profile`, `.ssh`, `.local` as shown in the picture. 

The command also shows detailed information such as:
* File or directory name
* File permissions
* Number of links
* Owner
* Group owner
* File size
* Last modification date and time

**What makes a file hidden in Linux — is it a special type:**
A file is hidden in Linux when its name begins with a dot (**`.`**). Hidden files are not a special file type. They are ordinary files or directories that follow this naming convention.

**Name two real hidden files found in a home directory and explain what they do:**
* `.bashrc` – Stores Bash shell configuration settings, aliases, and environment variables that are loaded when a shell session starts.
* `.profile` – Contains user-specific startup commands and environment settings that are executed when the user logs in.

![image](https://github.com/user-attachments/assets/86819dfa-eb25-4912-a133-ae56feea38b3)

**Q4: Create the entire structure above in a single command — including all nested subdirectories. Then display the full tree to verify.**

![image](https://github.com/user-attachments/assets/7243624b-5774-4ae0-9182-82fd2f0d2390)

### Which flag did you use, and what does it do?

I used the `-p` flag with the `mkdir` command.
The **-p** flag tells mkdir to create parent directories as needed. If intermediate directories do not already exist, they are created automatically. It also prevents errors if a directory already exists.

**Example:** `mkdir -p ~/projects/cyphercore/logs/access`
- This creates all missing directories in the path at once.

---

### What is brace expansion and how did it help here?

Brace expansion is a Bash feature that generates multiple strings from a single pattern enclosed in curly braces {}.

**For example:**
`echo {a,b,c}`
**produces:**
`a b c`

In this task, brace expansion allowed me to create several directories with one command: `mkdir -p ~/projects/cyphercore/{configs,reports,logs/{access,errors,archive}}`

Instead of typing separate commands for each directory, Bash expanded the pattern into all the required paths automatically.

---

### Could you have done this without it — and would you want to?

Yes. I could have created the directories individually, for example:
`mkdir -p ~/projects/cyphercore/configs`
`mkdir -p ~/projects/cyphercore/reports`
`mkdir -p ~/projects/cyphercore/logs/access`
`mkdir -p ~/projects/cyphercore/logs/errors`
`mkdir -p ~/projects/cyphercore/logs/archive`
However, using brace expansion is much faster, requires less typing, and reduces the risk of errors. Creating multiple related directories is generally the preferred approach.

**Q5: Task: Create these five empty files in a single command: configs/app.conf, configs/db.conf, logs/access/access.log, logs/errors/error.log, reports/weekly_report.txt**

**Command:** `touch ~/projects/cyphercore/configs/{app.conf, db.conf} ~/projects/cyphercore/logs/{access/access.log, errors/error.log} ~/projects/cyphercore/reports/weekly_report.txt`

![image](https://github.com/user-attachments/assets/45eb009a-a426-4215-a676-6a1ffdd85d65)

### What does this command actually do beyond creating files?

The `touch` command creates empty files if they do not already exist. However, its primary purpose is to update a file's timestamps.
When `touch` is run on a file, it updates the file's access time (atime) and modification time (mtime) to the current date and time.

### What happens if you run it on an existing file?

If the file already exists, `touch` does not delete or modify its contents. Instead, it updates the file's timestamps.

**Commands – for example:**

	ls -l app.conf
	touch app.conf
	ls -l app.conf

**Or:** 
	`ls -l ~/projects/cyphercore/configs/app.conf`
	`touch ~/projects/cyphercore/configs/app.conf`
	`ls -l ~/projects/cyphercore/configs/app.conf`

The file size and permissions remain the same, but the modification date and time change to the current time.

### Command: check ls -l before and after — what changes?

![image](https://github.com/user-attachments/assets/1e012297-7ea8-49ed-9292-814abe4a04b7)

**Before:**
`-rw-rw-r-- 1 sirban1 sirban1 0 Jun 10 09:53 app.conf`

**After running touch app.conf:**
`-rw-rw-r-- 1 sirban1 sirban1 0 Jun 10 10:05 app.conf`

**The following remain unchanged:**
* File name
* File size
* Permissions
* Owner and group

The modification timestamp changes to the current time.

### Why does this behaviour matter in automation scripts?

This behaviour is useful because scripts can safely ensure that required files exist without overwriting existing data.

For example, a deployment script can run: `touch app.conf`
If the file does not exist, it is created. If it already exists, its contents are preserved.

Updating timestamps can also be used to:
* Mark when a task was last run
* Trigger build systems that rely on file modification times
* Create marker files that indicate a process has completed

Because `touch` is non-destructive, it is commonly used in automation and system administration scripts.

---

**Q6: Task: List ~/projects/cyphercore/configs/ in long format with human-readable file sizes.**

**Command to run:** `ls -lh ~/projects/cyphercore/configs`
* `-l` = long format
* `-h` = human-readable file size

![image](https://github.com/user-attachments/assets/e9d3ea49-d3f9-4865-b2e1-20dbfd227ac0)

### What do the file sizes tell you about the files you just created?

The file sizes are shown as `0`, which means the files are empty. This is expected because they were created using the `touch` command, which creates files without adding any content.

### Break down the permission string on one file — explain every character group.

**Example permission string:** `-rw-rw-r--`

**Breakdown:**
* **`-`** → The file type. A dash (`-`) indicates a regular file. A directory would be shown as `d`.
* **`rw-`** → Permissions for the owner:
  * `r` = read
  * `w` = write
  * `-` = no execute permission
* **`rw-`** → Permissions for the group:
  * `r` = read
  * `w` = write permission
  * `-` = no execute permission
* **`r--`** → Permissions for others:
  * `r` = read
  * `-` = no write permission
  * `-` = no execute permission

Therefore, the owner can read and modify the file, while group members and other users can only read it.

### What does the -h flag add?

The **-h** flag makes file sizes human-readable. 

**Without -h**, sizes are displayed in bytes: `ls -l`
*Example:*
1024
1048576

**With -h**: `ls -lh`
*Example:*
1.0K
1.0M
This makes file sizes easier for humans to understand, especially when working with large files and directories.

**Q7: Task: Display the full tree of ~/projects/cyphercore again**. 

![image](https://github.com/user-attachments/assets/7045794b-8431-409a-8c7c-026c5ab498fd)

### How is tree different from ls -R?

Both commands display directory contents recursively, but they do it differently:
* `tree` shows the directory structure in a visual hierarchical format, using lines and branches to represent folder relationships clearly.
* `ls -R` prints a text-based recursive listing, showing each directory followed by its contents in sequence, but without a visual structure.

**In short:**
* `tree` is visually easier to understand
* `ls -R` is more raw and minimal
  
**Example difference:**

**tree output:**

![image](https://github.com/user-attachments/assets/1bf1deed-7411-4fea-9ee2-d6f56ec87a77)

**ls -R output:**

![image](https://github.com/user-attachments/assets/27ed3687-574b-4c50-9868-a63e9562a677)

### When would a DevSecOps engineer run tree on a live server?

A DevSecOps engineer would use `tree` on a live server to quickly understand and audit the file system structure. They might be looking for:
* Unexpected or unauthorized directories or files
* Misplaced configuration files
* Hidden or suspicious files in sensitive directories
* Proper placement of logs, configs, and secrets
* Verification of deployment structure after automation or CI/CD runs

It helps with security reviews and incident response as it gives a fast visual overview of what exists on the system without opening files or deeply inspecting each directory individually.

---

9:42 AM. Abena's phone buzzes.  
Abena: "Staging just threw errors. I need you to simulate what their application logs look like so we can practice triaging them."  
She hands Kofi a notepad with three log lines:  

	2025-06-02 08:14:33 INFO  Application started successfully
	2025-06-02 08:14:55 WARN  High memory usage detected: 87%
	2025-06-02 08:15:10 ERROR Database connection timeout — retrying (attempt 1/3)

Abena: "Put those into the access log. One at a time. Do not overwrite the file."

---

**Q8: Task: Write each of the three log lines into logs/access/access.log using echo, appending each one. Read the file back to confirm all three lines exist.**

**Each command is to be run separately:**

*	`echo "2025-06-02 08:14:33 INFO Application started successfully" >> ~/projects/cyphercore/logs/access/access.log`
*	`echo "2025-06-02 08:14:55 WARN High memory usage detected: 87%" >> ~/projects/cyphercore/logs/access/access.log`
*	`echo "2025-06-02 08:15:10 ERROR Database connection timeout — retrying (attempt 1/3)" >> ~/projects/cyphercore/logs/access/access.log`

**Command for reading back to confirm the operation:**
`cat ~/projects/cyphercore/logs/access/access.log`

<img width="940" height="251" alt="image" src="https://github.com/user-attachments/assets/f0290c37-e26b-4f1d-87bb-eb4b6171be5e" />

**What is the difference between `>` and `>>`?**

    > redirects output to a file and overwrites the file completely
    >> redirects output to a file and appends to the existing content

**So:**
    `>` = destructive overwrite
    `>>` = safe append

**Prove it:**
**access.log** file (~/projects/cyphercore/logs/access/access.log) contains information/data in the image below:

Now overwrite it – access.log by adding `>` to the command:

<img width="940" height="150" alt="image" src="https://github.com/user-attachments/assets/5d8c83b2-5326-4a9e-8b65-37109b9a5901" />

**Now overwrite it – access.log by adding `>` the command:**

`echo "Testing > function/power" > ~/projects/cyphercore/logs/access/access.log`

**Confirm the output of the operation using cat command:**

`cat ~/projects/cyphercore/logs/access/access.log`

<img width="940" height="77" alt="image" src="https://github.com/user-attachments/assets/43f26cad-94a4-4221-bf32-1ab29dfc3c88" />

Only the newly added text – “Testing > function/power” remains in the access.log  file. All previous log entries are gone.

**Restore the file:**

To restore, you must re-add the missing log lines manually or from backup:

`echo "2025-06-02 08:14:33 INFO  Application started successfully" >> ~/projects/cyphercore/logs/access/access.log`

`echo "2025-06-02 08:14:55 WARN  High memory usage detected: 87%" >> ~/projects/cyphercore/logs/access/access.log`

`echo "2025-06-02 08:15:10 ERROR Database connection timeout — retrying (attempt 1/3)" >> ~/projects/cyphercore/logs/access/access.log`

Output command: `cat ~/projects/cyphercore/logs/access/access.log`

<img width="940" height="214" alt="image" src="https://github.com/user-attachments/assets/bf919fdb-ce22-482b-a060-e719636ed1af" />

**Why is confusing `>` and `>>` dangerous in production log rotation scripts?**

In production systems, logs are critical for debugging and auditing. Accidentally using `>` instead of `>>` can:
	* Erase active logs containing important incident data
	* Remove evidence needed for security investigations
	* Break monitoring and alerting systems that depend on log history
    * Cause loss of compliance/audit records
In log rotation scripts, this mistake is especially dangerous because logs are often automatically managed. One wrong redirect operator can wipe out real-time system history without any warning.

---

**Q9: Task: Display the access log using two different reading commands — one from top to bottom, one from bottom to top.**

* **Command for “from the top”:** `cat ~/projects/cyphercore/logs/access/access.log`
* **Command for “from the bottom”:** `tac ~/projects/cyphercore/logs/access/access.log`

![image](https://github.com/user-attachments/assets/2a2ffe15-dbd4-466b-9f7e-c9b226af71b9)

### What is the difference between the two reading commands?
* `cat` prints the file from top to bottom (oldest to newest lines).
* `tac` prints the file from bottom to top (newest to oldest lines).

They show the same data but in the opposite order.

### In a real incident (80,000 lines), which do you reach for first?
In a real incident, I would reach for the bottom of the file first using tools like `tac` or `tail`, because:
* The most recent errors are at the bottom
* Incidents are usually caused by the latest changes or events
* It reduces time spent scanning irrelevant historical logs

Typically, start with:
`tail -n 50 logfile`

### What third command shows only the last 10 lines?
The command is:
`tail -n 10 logfile` *(Or simply `tail logfile`, which defaults to 10 lines.)*

### Why this matters in debugging
Logs grow over time, so reading everything is inefficient. Tools like `tail` and `tac` help engineers quickly focus on the most recent and most relevant system events during troubleshooting.

---

9:42 AM. Abena: "Now the error log — four lines. Don't use echo four times. Write it the way you'd write it in a script."  
* She gives Kofi the entries:  

2025-06-02 08:15:10 ERROR    Database connection timeout — retrying (attempt 1/3)
2025-06-02 08:15:13 ERROR    Database connection timeout — retrying (attempt 2/3)
2025-06-02 08:15:16 CRITICAL Database connection failed — all retries exhausted
2025-06-02 08:15:16 CRITICAL Triggering failover to secondary DB at 10.0.0.52

---

# Q10: Task: Use heredoc notation to append all four lines into logs/errors/error.log in one operation. Read it back and 📸 screenshot.

**Command run operation:** 
`cat <<  'EOF'  > ~/projects/cyphercore/logs/errors/error.log
 2025-06-02 08:15:10 ERROR    Database connection timeout — retrying (attempt 1/3)
 2025-06-02 08:15:13 ERROR    Database connection timeout — retrying (attempt 2/3)
 2025-06-02 08:15:16 CRITICAL Database connection failed — all retries exhausted
 2025-06-02 08:15:16 CRITICAL Triggering failover to secondary DB at 10.0.0.52
 EOF`

💡 Hint: A heredoc starts with << and a delimiter word (e.g., EOF). Everything between the opening and closing delimiters is written as-is. The closing delimiter must be alone on its line.

<img width="940" height="304" alt="image" src="https://github.com/user-attachments/assets/3ef6bdc2-9fc9-44c0-9fd0-62757aa00854" />

**Read it back**: `tac ~/projects/cyphercore/logs/errors/error.log`

<img width="940" height="125" alt="image" src="https://github.com/user-attachments/assets/9f62e506-006a-4b0b-b140-9de8707ae204" />

**What is a heredoc, and what does it solve over multiple echo commands?**

A heredoc ("here document") is a shell feature that allows multiple lines of text to be provided as input to a command.
Example:
      `cat <<  'EOF'  > ~/projects/cyphercore/logs/errors/error.log
      2025-06-02 08:15:10 ERROR    Database connection timeout — retrying (attempt 1/3)
      2025-06-02 08:15:13 ERROR    Database connection timeout — retrying (attempt 2/3)
      2025-06-02 08:15:16 CRITICAL Database connection failed — all retries exhausted
      2025-06-02 08:15:16 CRITICAL Triggering failover to secondary DB at 10.0.0.52
      EOF`
**Compared to multiple echo commands, a heredoc:**
      *	`Is` easier to read and maintain
      *	Reduces repetitive commands
      *	Preserves formatting exactly
      •	`Is`is commonly used in scripts to generate configuration files, logs, and templates

**What is the difference between 'EOF' (quoted) and EOF (unquoted)?**
When the delimiter is **unquoted**:
      `cat << EOF
      Database name: $DBNAME
      EOF`
Shell variables are expanded before being written.
Output:
Database name: cyphercore

<img width="940" height="180" alt="image" src="https://github.com/user-attachments/assets/358540ed-0f9c-48e2-b956-ac5e24f270e8" />

When the delimiter is quoted:
      `cat << 'EOF'
      Database name: $DBNAME
      EOF`
Variable expansion is disabled.
Output:
Database name: $DBNAME

<img width="940" height="152" alt="image" src="https://github.com/user-attachments/assets/403c581a-6fb2-4ac9-907f-11af3d591343" />

**Observation**
    *	EOF → variables, command substitutions, and escape sequences may be interpreted.
    *	'EOF' → content is treated literally.
Quoted delimiters are safer when exact text must be preserved.

<img width="940" height="561" alt="image" src="https://github.com/user-attachments/assets/ba7b0f89-bb50-4ee2-8ecb-4bbcbefa269a" />

**Why might someone on a compromised server prefer heredoc over opening nano or vim?**

A heredoc can create or modify files directly from the shell without launching an interactive editor.
**Example:**
      `cat << 'EOF' > ~/projects/cyphercore/configs/app.conf
      setting=value
      EOF`
**Advantages:**
      *	Faster during incident response
      *	Works over unstable SSH connections
      *	Can be pasted directly into scripts or automation
      
**What forensic traces does each method leave?**
**Heredoc:**
Typically leaves:
    •	Shell history entries (unless history is disabled)
    •	File modification timestamps
    •	Audit logs if command auditing is enabled
**nano/vim:**
Typically leaves:
    *	Shell history showing the editor launch
    *	File modification timestamps
    *	Swap files, backup files, or recovery files (depending on editor settings)
    *	Additional audit records showing editor execution
From a forensic perspective, interactive editors often leave more artifacts than a simple heredoc command, although both may still be visible through shell history and system auditing.

---

# Q11: Task: Open the error log using two different pager commands. Navigate and quit each one.

**more command:** press the Enter key to navigate line by line; Space key to navigate page by page; PgDn/PgUp to navigate page down/page up. q key to quit.

![image](https://github.com/user-attachments/assets/5124cf8b-ffea-4bd6-9075-b54c86624684)

**less command:**

![image](https://github.com/user-attachments/assets/a77c9d2a-0b00-43a8-bc24-da460e7e6d61)

### Key differences between more and less

Both are pager programs used to read files one screen at a time.

**more:**
* Older and simpler pager
* Primarily moves forward through a file
* Limited navigation features
* Suitable for small files

**less:**
* More powerful and flexible
* Can move forward and backward
* Supports searching
* Better suited for large log files
* Commonly used by system administrators and DevSecOps engineers

**A useful way to remember it is:** "less is more" because less provides more functionality than more.

### Which loads the entire file into memory first?

Generally:
* *more* is less efficient with very large files and has fewer optimizations.
* *less* is designed to read files incrementally and does not need to load the entire file into memory before displaying content.

For very large logs, less is the preferred tool.

### On a server with 512 MB RAM and a 3 GB log, which do you use?

I would use the **less** command because it can efficiently handle large files without requiring the entire file to be loaded into memory.

For troubleshooting recent events, I might first use: **tail -n 50 [filename]**, and then use the *less* command for deeper investigation.

### Three useful keyboard shortcuts for less:

* **Search:** `/search_term`, e.g., `/CRITICAL` – press enter to search forward through the file.
* **Jump to the end:** press `G` (capital G). This moves directly to the last line of the file.
* **Quit:** press `q`. This exits less and returns to the shell prompt.

---

# Q12: Tasks: Copy the error log to the archive folder as error_2025-06-02.log

**Commands:**
mkdir -p ~/projects/cyphercore/logs/archive && \
cp ~/projects/cyphercore/logs/errors/error.log \
~/projects/cyphercore/logs/archive/error_2025-06-02.log

**Confirm operations with commands:**
`tree ~/projects/cyphercore/logs or ls -l ~/projects/cyphercore/logs/archive`
`cat ~/projects/cyphercore/logs/archive/error_2025-06-02.log`

<img width="940" height="846" alt="image" src="https://github.com/user-attachments/assets/ae3b84cf-23fc-49b4-a9b0-38b8edab83e8" />

<img width="940" height="121" alt="image" src="https://github.com/user-attachments/assets/4714f082-e95b-475f-8533-0531c485c0a0" />

**Rename the access log to access_2025-06-02.log**
**Command:**
   `mv ~/projects/cyphercore/logs/access/access.log \
   ~/projects/cyphercore/logs/access/access_2025-06-02.log`

Confirm operation: `tree ~/projects/cyphercore` or `ls -l ~/projects/cyphercore/logs/access`

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/2cbcdf4a-5ba0-41e5-b36b-9e09fbb768e7" />

**What is the fundamental difference between the two commands used?**

The two commands were: `cp` and `mv`
**cp (copy):** creates a new file with the same contents as the original.
After copying:
    *	Original file remains unchanged.
    *	New file exists as a separate file.
**mv (move/rename):** changes a file's location or name.
After moving/renaming:
    *	The original filename is removed.
    *	The file exists only under the new name.

### After renaming, does the original filename still exist?**

No. The original name no longer exists as the filename changed from access.log to access_2025-06-02.log
**How can you verify this?**
Command to use: `ls -l ~/projects/cyphercore/logs/access`
**If the rename succeeded:**
    *	access_2025-06-02.log appears.
    *	access.log does not appear.

<img width="940" height="193" alt="image" src="https://github.com/user-attachments/assets/53487d17-7bd7-4a76-9337-37e2f9749118" />

**Does copying a large file duplicate all data on disk immediately?**

Usually, yes. A traditional cp operation creates a second copy of the file's data on disk. If a 10 GB file is copied, approximately another 10 GB of storage is required.
However, some modern file systems support optimizations such as Copy-on-Write (CoW), Reflinks, and Snapshots. On such systems, the data may not be physically duplicated immediately. Instead, the filesystem initially shares the data blocks and only creates separate copies when changes occur.

**Why does this matter?**
For large log files:
    *	mv is usually very fast because it often only updates directory metadata.
    *	cp may take significantly longer because file contents must be duplicated.
This is an important consideration when handling large logs, backups, and archives on production systems.

---

**Q13: Task: Create an empty directory reports/temp_drafts**

Command: `mkdir ~/projects/cyphercore/reports/temp_drafts`
Confirm operation: `ls -l ~/projects/cyphercore/reports`

<img width="940" height="535" alt="image" src="https://github.com/user-attachments/assets/1ea2706b-be0c-4f33-91db-799a756c7d93" />

<img width="940" height="161" alt="image" src="https://github.com/user-attachments/assets/45af5ade-1118-4aab-b1af-4db69b117f61" />

**Remove it using the command designed for empty directories**
Command: `rmdir ~/projects/cyphercore/reports/temp_drafts`
Confirm operation: `tree ~/projects/cyphercore`

<img width="940" height="195" alt="image" src="https://github.com/user-attachments/assets/db6d305f-8a0a-476e-a593-88602176e572" />>

**Attempt to remove the entire logs/ directory using that same command**
Command: `rmdir ~/projects/cyphercore/logs`
The command failed to remove the directory.

<img width="940" height="69" alt="image" src="https://github.com/user-attachments/assets/f3a018b7-8ff5-4a35-90db-7833ff568b0c" />

**What happened when you tried to remove logs/? Why?**

The command failed and displayed an error stating that the directory was not empty.

<img width="940" height="69" alt="image" src="https://github.com/user-attachments/assets/6bc931fb-f5b2-4809-a0a7-cb97ace536f8" />

This happened because rmdir can only remove empty directories. The logs directory contained subdirectories and log files, so the system prevented it from being removed.

**What is the difference between rmdir and rm -r?**

Command **rmdir**:
    *	Removes only empty directories.
    *	Refuses to remove directories that contain files or subdirectories.
    *	Safer because it prevents accidental deletion of data.
Command **rm -r**: rm -r ~/projects/cyphercore/logs
    *	Removes directories recursively.
    *	Deletes the entire logs directory and everything inside it, including files and subdirectories.

**What makes rm -rf dangerous?**
The command **rm -rf** combines:
    *	`-r` = recursive deletion
    *	`-f` = force deletion without confirmation

This means Linux will attempt to delete everything specified without asking for confirmation and without stopping for many common warnings.
If the wrong path is entered, important files, applications, logs, or even an entire system can be deleted.

**What rule should every engineer follow before running rm -rf?**
Always verify the exact path before pressing Enter.
A common safety practice is:
    * Run pwd to confirm your current directory.
    * Run ls to verify what will be affected.
    * Carefully review the command.
    * Only then execute rm -rf.
Many engineers follow the rule: "Read the command twice, run it once."
This helps prevent accidental deletion of critical data on production systems.

---

**Q14: Task: Display the full tree of ~/projects/cyphercore.**

**Command:** 
`tree ~/projects/cyphercore`

<img width="940" height="436" alt="image" src="https://github.com/user-attachments/assets/82f7075d-b59d-4956-a602-fbc7ab23e146" />

### Does the structure match Abena's original sticky note?
Yes, the core directory structure matches Abena's original design. Throughout the exercises, files were added inside these directories, such as configuration files, log files, archived logs, and reports. The overall directory layout remains consistent with the original structure while now containing realistic project data.

### Why does a clean, predictable directory structure matter for automation scripts?
Automation scripts often rely on fixed paths. For example, `backup_script.sh` may expect logs to exist in `logs/access/` and `logs/errors/`. If files are moved or stored in unexpected locations, the script may fail or miss important data. A predictable structure makes scripts easier to write, maintain, and troubleshoot.

### Why does it matter for log agents?
Log collection tools such as Logstash, Fluent Bit, Fluentd, and other monitoring agents are typically configured to watch specific directories.  

**Example:** `/var/log/application/*.log`

If logs are scattered across different locations, the agent may not collect them, causing gaps in monitoring and alerting.
A consistent structure ensures logs are discovered and processed correctly.

### Why does it matter for security monitoring tools?

Security monitoring and SIEM platforms depend on known file locations to detect suspicious activity. 

**Examples include:**
* Monitoring authentication logs
* Detecting unauthorized file changes
* Tracking application errors
* Investigating security incidents

**When files follow a standard structure:**
* Security rules are easier to configure
* Incident investigations are faster
* Compliance audits are simpler
* Unauthorized changes are easier to identify

A clean and predictable directory structure improves reliability, automation, security visibility, and operational efficiency across the entire system.

---

# Q15: Task: Write three lines into configs/db.conf: 
   
	DB_HOST=10.0.0.10
    DB_PORT=5432
    DB_NAME=cyphercore_prod

**Command:** 
    `cat << ‘EOF’ > ~/projects/cyphercore/configs/db.conf ¬\
    >DB_HOST=10.0.0.10
    >DB_PORT=5432
    >DB_NAME=cyphercore_prod
	EOF`

<img width="940" height="288" alt="image" src="https://github.com/user-attachments/assets/3fa47105-cf8c-420d-b75f-0674d471bd91" />

**Create a hard link to it in the same directory named db_hardlink.conf**

**Command:** 
    ln ~/projects/cyphercore/configs/db.conf ~/projects/cyphercore/configs/db_hardlink.conf

<img width="940" height="232" alt="image" src="https://github.com/user-attachments/assets/0046c5e5-5b46-49b7-a910-b9ab61174862" />

**List the configs directory showing inode numbers alongside full file details**

<img width="940" height="232" alt="image" src="https://github.com/user-attachments/assets/cda3e38b-9f12-4a39-8629-6e612d5a3343" />

💡 Hint: The flag that shows inode numbers with ls -l is -i.

**What do you notice about the inode numbers of both files?**

Both db.conf and db_hardlink.conf have the same inode number – 396061. This shows that they are hard links pointing to the same underlying file data on disk.

**What does the link count (third column) represent?** 
The link count shows how many directory entries (hard links) point to the same inode. Since two filenames are referring to the same inode, the link count is `2`.

**What is an inode in your own words?**
An inode is a data structure used by a filesystem to store information about a file, such as its permissions, ownership, size, timestamps, and disk block locations. Filenames are directory entries that point to an inode, and multiple filenames can point to the same inode through hard links.

---

**Q16: Task: Delete the original db.conf. Then read the hard link file.**

<img width="940" height="101" alt="image" src="https://github.com/user-attachments/assets/2f17a1e4-f83b-44c4-8d25-cd73b425f169" />

<img width="940" height="231" alt="image" src="https://github.com/user-attachments/assets/fbdef063-5294-4175-a224-4b09c72f72a8" />

<img width="940" height="101" alt="image" src="https://github.com/user-attachments/assets/57e53d99-7913-47b9-902c-75e9f37f2f72" />

**Is the data still there? Explain why inodes and link counts are used**. 

Yes. The data remains accessible via the hard link even after the original `db.conf` file was deleted.
A hard link and the original filename both point to the same inode. The inode contains the file's metadata and pointers to the actual data blocks on disk.
When `db.conf` was deleted, only one directory entry was removed. The inode and data blocks remained because the hard link still referenced the same inode.

**Check ls -li again — what changed in the link count?** 

Before deleting `db.conf`, the inode link count was 2 because two directory entries were pointing to the same inode.
After deleting db.conf, running `ls -li` showed:
    * The inode number remained the same.
    * The link count decreased from 2 to 1.
This indicates that one reference was removed, but the inode is still in use.

<img width="940" height="140" alt="image" src="https://github.com/user-attachments/assets/e62de9b9-40f7-4854-832a-4cf20cd0e4da" />

**What would need to happen for the data to be permanently deleted?**
The data would only be deleted when:
    * All hard links to the inode are removed (link count reaches 0), and
    * No running process still has the file open.
At that point, the filesystem can free the inode and release the data blocks

**How could an attacker use hard links to hide data on a compromised system?**
An attacker could create hard links to sensitive files and then delete the original filenames. Administrators might believe the files were removed, but the data would still exist through the hidden hard links. This technique can be used to conceal malicious files, maintain persistence, or make forensic investigations more difficult because the data remains accessible until every hard link is removed.

---

### Q17: Task: Write some content into configs/app.conf

**Command:** 
`echo “application_port=8080" > ~/projects/cyphercore/configs/app.conf`

<img width="940" height="126" alt="image" src="https://github.com/user-attachments/assets/0d51f9e8-d280-4f31-8ba0-7877e149a8ff" />

**Create a symbolic link to it inside ~/projects/cyphercore/ named app_config_link**

**Command:** 
`ln -s ~/projects/cyphercore/configs/app.conf ~/projects/cyphercore/app_config_link`

<img width="940" height="65" alt="image" src="https://github.com/user-attachments/assets/5714ae0c-0dc0-4f29-9de1-a182b5a37ad2" />

### List ~/projects/cyphercore/ showing full details including link targets

**Command:** 
`ls -l ~/projects/cyphercore`

<img width="940" height="171" alt="image" src="https://github.com/user-attachments/assets/cbda4390-8cce-4527-90ef-cd18aab56ba2" />

### Delete the original app.conf and try to read the symlink

**Command:** 
`rm ~/projects/cyphercore/configs/app.conf`

<img width="940" height="187" alt="image" src="https://github.com/user-attachments/assets/bd31d82b-257d-458c-94c5-28c0da03221d" />

**Reading the symlink:**

**Command:** 
`cat ~/projects/cyphercore/app_config_link`

<img width="940" height="82" alt="image" src="https://github.com/user-attachments/assets/3837d608-c0a3-425d-a67a-08b24a6eb8a2" />

**What character in the listing identifies a symbolic link?** 

<img width="940" height="187" alt="image" src="https://github.com/user-attachments/assets/f595ed52-3844-4b94-b6fb-65116ab7a282" />

The character `l` at the beginning of the permissions field identifies a symbolic link. 

**For example:**
_lrwxrwxrwx 1 sirosaubun sirosaubun..._
The `l` indicates that the file is a symlink rather than a regular file or directory.

**What happened when you read the broken link — what is this called?**

After deleting the original app.conf file, attempting to read the symbolic link failed with a "No such file or directory" error because the symlink still pointed to the original path, but the target file no longer existed.

<img width="940" height="82" alt="image" src="https://github.com/user-attachments/assets/29712b58-0143-474d-a615-8e767a1d1911" />

This is called a dangling symlink or broken symlink.

**Why does a dangling symlink in a deployment pipeline cause failures that are hard to debug at 2 AM?**

A deployment pipeline may rely on symlinks to point to configuration files, application releases, or shared resources. If the target file is removed, renamed, or moved, the symlink remains but points to a non-existent location. Applications then fail when trying to access the missing resource.
These failures can be difficult to debug because the symlink itself still exists and may appear correct at first glance. The actual problem is hidden in the missing target, causing unexpected errors that may only appear after deployment or during runtime.

### Q18: Fill in this table from your experiments. 

No guessing:

**Property**	                              **Hard Link**    **Soft Link**
Shares inode with original?	                      Yes	             -
Works across different file systems?	           -	            Yes
Survives deletion of the original?	              Yes	             -
Can link to a directory?	                       -	            Yes
Shows as l in ls -la?	                           -	            Yes
Detectable by matching inodes in ls -li?	      Yes	             -

**After the table: one real-server use case for a hard link and one for a soft link in a DevSecOps context.**

**Hard link:**
Uses a hard link to maintain multiple references to log files in different directories without duplicating disk space. This can allow automated archiving while keeping the original accessible for monitoring tools. 

**Soft link:**
Uses a soft link to point to a shared configuration file across different servers or containers in a DevSecOps pipeline. 
For example, symlinking _/etc/app/config.yaml_ to the latest version in _/opt/deploy/releases/current/config.yaml_ allows rolling updates without modifying scripts that reference the symlink.

---

**Q19: Task: Run a single ls targeting two paths: one that exists (logs/) and one that does not**

First, create files – ~/projects/cyphercore/reports/stdout_output.txt & ~/projects/cyphercore/reports/stderr_output.txt 

<img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/38bf5f27-64a3-4919-8b25-798a595f962d" />

**Command:**

`ls ~/projects/cyphercore/logs/ ~/projects/cyphercore/fakefile_path/ > ~/projects/cyphercore/reports/stdout_output.txt 2> ~/projects/cyphercore/reports/stderr_output.txt`
The outcome of the operation will produce an error message because of the non-existent file – ~/projects/cyphercore/fakefile 

<img width="940" height="168" alt="image" src="https://github.com/user-attachments/assets/c36c6a09-fd8b-4aea-8833-d82f3235f644" />

**Redirect normal output to reports/stdout_output.txt**

**Command:**

`ls ~/projects/cyphercore/logs/ > ~/projects/cyphercore/reports/stdout_output.txt`

<img width="940" height="102" alt="image" src="https://github.com/user-attachments/assets/c108f297-938c-4528-b5b4-7d3fd9505b7e" />

**Redirect error output to reports/stderr_output.txt**

**Command:** 

`ls ~/projects/cyphercore/fakefile_path/ 2> ~/projects/cyphercore/reports/stderr_output.txt`

<img width="940" height="97" alt="image" src="https://github.com/user-attachments/assets/8d13c28e-3204-4160-afa6-4da4a84668ac" />

**Read both files back**

For the reports/stdout_output.txt 

**Command to read back:** `tac ~/projects/cyphercore/reports/stdout_output.txt`

For the reports/stderr_output.txt

**Command to read back:** `tac ~/projects/cyphercore/reports/stderr_output.txt`

💡 Hint: > is shorthand for 1>. Think of 2>&1 as "send stream 2 to wherever stream 1 is going."

**Which content landed where and why?**

The successful output from ls logs/ was written to reports/stdout_output.txt because standard output (stream 1) was redirected with: > reports/stdout_output.txt
The error message generated by attempting to list the nonexistent path was written
to reports/stderr_output.txt because standard error (stream 2) was redirected with: 2> reports/stderr_output.txt

**Name all three streams, their numbers, and what each carries.**

**Stream**	        **Number**	      **Purpose**
Standard Input	     	0	      		Data read by a command
Standard Output	        1	      		Normal command output
Standard Error	        2	      		Error and diagnostic messages

**What does 2> mean specifically?**

**2>** redirects stream 2 (standard error) to a file.
**Example:** `ls parocyber_dir 2> errors.txt`
This sends only the error message to errors.txt while leaving standard output unchanged.

**Run the same command with 2>&1 | cat instead. Explain what 2>&1 does and where you'd use it in a real deployment script.**

**Command:** `ls ~/projects/cyphercore/logs/ ~/projects/cyphercore/fakefile_path/ 2>&1 | cat`

<img width="940" height="208" alt="image" src="https://github.com/user-attachments/assets/99c0d824-1446-429c-a58d-12772deaafb6" />

**What does “2>&1” do?**

**2>&1** tells the shell to:

**>** redirects standard error (2) to the same destination currently used by standard output (1).
After this redirection, both normal output and error output flow through the pipe to ‘cat’.

**Where would you use it in a real deployment script?**
This is commonly used when:
    * Capturing complete logs from a deployment
	* Sending both output and errors into a logging system
    * Piping everything through tools such as ‘tee’, ‘grep’, or log processors

**Example:** `./deploy.sh 2>&1 | tee deploy.log`

This records both normal messages and errors in a single deployment log while still displaying them on the terminal.

---

Q20: Task: Write the following report into reports/weekly_report.txt using a heredoc — in one operation:

==========================================================
WEEKLY SECURITY REPORT — CypherCore Systems
Week Ending: 2025-06-06
Prepared By: Osad
===========================================================

SUMMARY:
No critical incidents recorded this week.
One WARN: memory usage spike at 08:14 on 2025-06-02.
One CRITICAL: DB connection failure at 08:15 on 2025-06-02.
Failover to secondary DB executed successfully.
				
RECOMMENDED ACTION:
Review DB connection pool settings before the next deployment.
=============================================================

**Run command to write the text:** `cat > ~/projects/cyphercore/reports/weekly_report.txt \ or cat << EOF > ~/projects/cyphercore/reports/weekly_report.txt \
      ============================================
      WEEKLY SECURITY REPORT — CypherCore Systems
      Week Ending: 2025-06-06
      Prepared By: Osad……………..
      EOF`
	  
**Command to confirm the operation:** `cat ~/projects/cyphercore/reports/weekly_report.txt`

<img width="940" height="697" alt="image" src="https://github.com/user-attachments/assets/31e24456-d1f5-4dfb-b83a-4d6a907c6510" />

**Did you quote the delimiter or not — and why does that choice matter?**

No, I did not quote the delimiter (`EOF`).

**Example:**
	`cat <<EOF
	Hello $USER
	EOF`

When the delimiter is unquoted, shell expansions occur inside the heredoc:
	*	Variable expansion (`$VAR`)
	*	Command substitution (`$(command)`)
    *	Arithmetic expansion (`$((...))`)

If the delimiter is quoted, the contents are treated literally, and no expansion occurs.

For this report, quoting did not matter because the text contained no variables or commands that needed expansion.

**Set a variable ANALYST="Your Name". Write two small heredocs — one quoted, one unquoted — both referencing $ANALYST. What is the difference and why?

**Command:** `ANALYST=Osad`

**Unquoted heredoc:**
	`cat <<EOF
	Analyst: $ANALYST
	EOF`

<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/cb7316e2-a103-45b1-8146-a716aeefec77" />

**Quoted heredoc:**
	`cat <<'EOF'
	Analyst: $ANALYST
	EOF`

<img width="940" height="124" alt="image" src="https://github.com/user-attachments/assets/1af64f48-21b2-4da7-9145-0b54e977b641" />

**Unquoted:** 

It outputs the variable set up initially – ANALYST=Osad

<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/6fa142e3-69af-4916-8e3a-e0d568517841" />

**Quoted:** 

It disregards the extension of the variable(ANALYST=Osad) by outputting the exact test – Analyst=$ANALYST

<img width="940" height="124" alt="image" src="https://github.com/user-attachments/assets/e930fe4f-45b6-426d-9117-e33fb15f27bd" />

**What is the difference and why?**
    *	The unquoted heredoc expands variables before writing output.
    *	The quoted heredoc suppresses expansion and preserves the text exactly as written.
**Because of that:**    
	*	<<EOF → $ANALYST becomes Osad
   
<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/8878106d-699b-4a5f-bb1f-1a59117322d3" />

    *	<<'EOF' → $ANALYST remains literal text

<img width="940" height="124" alt="image" src="https://github.com/user-attachments/assets/24b4a7d7-1779-49d4-ab3f-d84d0dde03d7" />

**When do you use a quoted heredoc vs unquoted in a real script? Give a concrete example of each.**

**Unquoted heredoc:** 

It is used when you want variables substituted into generated files.

**Example:**
    `HOSTNAME="web01"
    cat > app.conf <<EOF
    server_name=$HOSTNAME
    environment=production
    EOF`
  
<img width="940" height="222" alt="image" src="https://github.com/user-attachments/assets/8fdf06af-2aae-4c7f-9a9a-2d82855e8dba" />

Confirm the operation by running the command: `cat /home/sirosaubun/app.conf`

**Result:**

<img width="940" height="119" alt="image" src="https://github.com/user-attachments/assets/902b947e-4255-469a-9aa5-126a83c58d9d" />

**Quoted heredoc:** 

It is used when generating content that contains shell variables or commands that must remain untouched.

**Example:**
    `cat > backup_template.sh <<'EOF'
	#!/bin/bash
	echo "Backing up $HOME"
	EOF`

<img width="940" height="174" alt="image" src="https://github.com/user-attachments/assets/a9d01d49-58c4-48e0-84c7-260ee15d18fb" />

**Command to confirm operation:** 
	`cat /home/sirosaubun/backup_template.sh

**Result:**

<img width="940" height="91" alt="image" src="https://github.com/user-attachments/assets/dfc77b64-09fa-4178-b39f-7188821720f9" />

The variable is preserved for execution later rather than expanded when the file is created.

---

**Q21: Task: Display the final tree of ~/projects/cyphercore.** 

**Command:**
	`tree ~/projects/cyphercore` (alternatively: `find ~/projects/cyphercore | sort`)

<img width="940" height="517" alt="image" src="https://github.com/user-attachments/assets/314a051a-307c-4f36-a1cc-4a772e6c7a6e" />

**Look at everything you built today — the structure, logs, archives, links, report. How does each connect to what a real DevSecOps engineer does daily?**

Looking at everything built today, I can see the importance of the File Hierarchy System – FHS in structuring and connecting files in the system. And how each part connects to real DevSecOps work. The directory structure represents how engineers organize applications, configurations, logs, reports, and deployment assets so that systems remain maintainable and easy to troubleshoot. The logs provide operational visibility and help identify failures or security events. Archives are used for backups, retention, and disaster recovery. Symbolic links help manage shared resources and versioned deployments efficiently. Reports document incidents, findings, and operational status for teams and stakeholders.

**Pick two commands from today that changed how you think about Linux and explain why they matter beyond this bootcamp.**

## Two commands that changed how I think about Linux are:

1. **Redirection (>, 2>, and 2>&1)**

Before this Lab, I saw command output as something that appeared on the screen. Learning about standard input, standard output, and standard error showed me that Linux treats data streams as separate channels that can be redirected and combined. This matters beyond the bootcamp because production systems generate large amounts of output that must be captured, logged, monitored, and analyzed. Understanding stream redirection is essential for automation, troubleshooting, and building reliable deployment scripts.

2. **Heredocs (<<EOF)**

Heredocs showed me that Linux can generate structured files directly from scripts without opening an editor. This is important because DevSecOps engineers frequently automate the creation of configuration files, deployment manifests, reports, and scripts. Understanding the difference between quoted and unquoted heredocs also taught me how variable expansion works, which is critical when generating templates safely and predictably in automated environments.

These commands demonstrated that Linux is not just a collection of commands but a system designed for automation, reproducibility, and operational efficiency. Those principles are at the core of modern DevSecOps practices.


## Screenshots
Screenshots demonstrating the completed tasks are available in the `screenshots` folder.

## Author
Samuel Osadare
Program: ParoCyber DevSecOps Bootcamp

# Linux Command Line Mastery, Shell Scripting & Systems

## 1. Introduction & Prerequisites
Linux is the operating system powering enterprise servers, cloud instances (AWS EC2), containers (Docker), and edge infrastructure. Mastering the shell environment, process lifecycle, filesystem permissions, stream redirection, and text processing utilities (`grep`, `sed`, `awk`) is essential for any senior engineer.

- **Prerequisites:** Basic terminal window usage.
- **Estimated Study Time:** 16 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Navigate the UNIX Filesystem Hierarchy Standard (FHS: `/etc`, `/var`, `/usr`, `/proc`, `/dev`).
- Master standard streams (`stdin`, `stdout`, `stderr`), pipes (`|`), and redirection operators (`>`, `>>`, `2>&1`).
- Perform text processing using `grep`, `awk`, `sed`, `cut`, `sort`, and `uniq`.
- Manage Linux processes: signals (`SIGTERM`, `SIGKILL`), foreground/background jobs, `ps`, `top`, `htop`, `systemd`.
- Configure POSIX file permissions (`chmod`, `chown`, octal notation, sticky bit, umask).
- Author modular, robust Bash automation scripts with error checking (`set -euo pipefail`).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] UNIX Filesystem Hierarchy (`/bin`, `/etc`, `/home`, `/var/log`, `/proc`, `/tmp`)
- [ ] Standard I/O Streams (`stdin` 0, `stdout` 1, `stderr` 2)
- [ ] Redirection Operators (`<`, `>`, `>>`, `2>`, `&>`, `|`)
- [ ] File Permissions & Ownership (`chmod 755`, `chown user:group`, octal representation)
- [ ] Process Inspection & Control (`ps aux`, `top`, `kill`, `killall`, `pkill`)
- [ ] System Resource Monitoring (`free -m`, `df -h`, `du -sh`, `uptime`, `vmstat`)
- [ ] Advanced Text Processing (`grep -rnE`, `awk '{print $1}'`, `sed 's/find/replace/g'`)
- [ ] Networking CLI Tools (`curl -Iv`, `netstat`/`ss -tulpn`, `dig`, `traceroute`, `ping`)
- [ ] SSH & Key Management (`ssh-keygen`, `~/.ssh/authorized_keys`, `scp`, `rsync`)
- [ ] Bash Scripting Robustness (`#!/usr/bin/env bash`, `set -euo pipefail`, trap handling)

---

## 4. Free Documentation Resources
- **Linux Journey:** [https://linuxjourney.com/](https://linuxjourney.com/)
- **The Linux Command Line (Free Book by William Shotts):** [https://linuxcommand.org/tlcl.php](https://linuxcommand.org/tlcl.php)
- **OverTheWire Bandit Wargame:** [https://overthewire.org/wargames/bandit/](https://overthewire.org/wargames/bandit/)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** *Linux for Beginners Full Course*
- **NetworkChuck:** *Linux for Hackers // Ep. 1-10*
- **TechWorld with Nana:** *Linux Basics for DevOps*

---

## 6. Important Concepts Explained

### Standard Streams & Pipeline Architecture
Every UNIX process is initialized with three file descriptors:
- `0`: Standard Input (`stdin`)
- `1`: Standard Output (`stdout`)
- `2`: Standard Error (`stderr`)

Pipes (`|`) connect the `stdout` of the upstream process directly to the `stdin` of the downstream process without writing intermediate files to disk.

```
       Command 1                Pipe (|)               Command 2
 ┌───────────────────┐    ─────────────────►    ┌───────────────────┐
 │ stdout (fd 1)     │                          │ stdin (fd 0)      │
 └───────────────────┘                          └───────────────────┘
```

---

## 7. Practical Code Examples

### Production-Quality Bash Automation Script
```bash
#!/usr/bin/env bash
# Strict error handling: exit on error, exit on unset vars, fail on pipe errors
set -euo pipefail
IFS=$'
	'

LOG_DIR="/var/log/app"
BACKUP_DIR="/backups/logs"
DATE_STAMP=$(date +"%Y%m%d_%H%M%S")

log() {
  echo "[$(date -u +"%Y-%m-%dT%H:%M:%SZ")] $*"
}

cleanup() {
  log "Cleaning up temporary files..."
  rm -f /tmp/backup_*.tar.gz
}
trap cleanup EXIT ERR

log "Starting log backup routine..."
mkdir -p "${BACKUP_DIR}"

if [[ -d "${LOG_DIR}" ]]; then
  ARCHIVE_FILE="${BACKUP_DIR}/logs_${DATE_STAMP}.tar.gz"
  tar -czf "${ARCHIVE_FILE}" -C "${LOG_DIR}" .
  log "Successfully created archive at ${ARCHIVE_FILE}"
else
  log "Error: Directory ${LOG_DIR} not found!"
  exit 1
fi

# Rotate backups: remove files older than 7 days
find "${BACKUP_DIR}" -type f -name "logs_*.tar.gz" -mtime +7 -delete
log "Backup and rotation completed successfully."
```

---

## 8. Practice Assignments
1. **Assignment 1:** Play through levels 0-10 of OverTheWire Bandit to practice navigation, file permissions, hidden files, and password extraction.
2. **Assignment 2:** Write a shell one-liner that parses an Nginx `access.log` file, extracts the client IP address (column 1), counts occurrences, and prints the top 5 most frequent visitors.

---

## 9. Mini-Projects
- **Automated System Health Monitor Daemon:** A Bash script that runs every 5 minutes (via cron/systemd timer) to check CPU usage, RAM utilization, and disk space, sending an alert if disk usage exceeds 85%.

---

## 10. Interview Questions & Detailed Answers

### Q1: What does `chmod 755` mean in octal notation?
**Answer:** File permissions are divided into three groups: Owner, Group, and Others. The permission values are Read (4), Write (2), and Execute (1).
- `7` (4+2+1) = Read, Write, Execute for the Owner.
- `5` (4+0+1) = Read and Execute for the Group.
- `5` (4+0+1) = Read and Execute for Others.

### Q2: What is the difference between `SIGTERM` (signal 15) and `SIGKILL` (signal 9)?
**Answer:** `SIGTERM` is the polite termination request; the receiving process can catch the signal, flush database connections, complete in-flight requests, remove temporary files, and exit gracefully. `SIGKILL` cannot be caught, blocked, or ignored; the Linux kernel immediately terminates the process and reclaims memory, which can lead to data corruption if writes were in progress.

---

## 11. Revision Notes
- Always start Bash scripts with `set -euo pipefail`.
- Use `grep -rn "pattern" .` to search recursively with line numbers.
- Use `ss -tulpn` to check listening network ports and binding processes.
- Quote shell variables (`"${VAR}"`) to prevent word splitting and glob expansion.

---

## 12. Topic Completion Checklist
- [ ] Mastered standard streams, redirection, and pipes
- [ ] Understand file permission octals and groups
- [ ] Filtered and formatted logs using `awk` and `grep`
- [ ] Created a robust Bash automation script with traps

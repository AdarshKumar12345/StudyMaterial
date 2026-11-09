# Linux – Important Topics & Learning Resources

## 1. Linux Fundamentals
- [ ] Introduction to Linux
- [ ] What is an Operating System?
- [ ] Linux vs Unix vs Windows
- [ ] Linux Architecture
- [ ] Linux Kernel
- [ ] Shell and Terminal
- [ ] Linux Distributions (Ubuntu, Debian, Fedora, Arch)
- [ ] Open Source Concepts
- [ ] Linux Boot Process
- [ ] Kernel Space vs User Space
- [ ] GUI vs CLI
- [ ] Linux Directory Structure
- [ ] Root Directory (`/`)
- [ ] Home Directory (`~`)
- [ ] Absolute vs Relative Paths
- [ ] Linux File Types

**Resources:**
- [Linux Journey](https://linuxjourney.com/)
- [Ubuntu Documentation](https://documentation.ubuntu.com/)
- [The Linux Documentation Project](https://tldp.org/)

## 2. Essential Linux Commands
- [ ] `pwd` – Current Directory
- [ ] `ls` – List Files
- [ ] `cd` – Change Directory
- [ ] `mkdir` – Create Directory
- [ ] `rmdir` – Remove Empty Directory
- [ ] `touch` – Create Files
- [ ] `cp` – Copy Files
- [ ] `mv` – Move or Rename Files
- [ ] `rm` – Remove Files
- [ ] `cat` – Display File Content
- [ ] `less` and `more` – Read Files
- [ ] `head` and `tail`
- [ ] `echo` and `printf`
- [ ] `clear`
- [ ] `history`
- [ ] `man` – Manual Pages
- [ ] `which` and `whereis`
- [ ] `find` and `locate`
- [ ] `file` – Identify File Type
- [ ] `stat` – File Information

**Resources:**
- [Linux Commands – Ubuntu](https://ubuntu.com/tutorials/command-line-for-beginners)
- [Linux Command Library](https://linuxcommandlibrary.com/)

## 3. File System and File Management
- [ ] Linux File System Hierarchy
- [ ] `/bin` and `/sbin`
- [ ] `/etc`
- [ ] `/home`
- [ ] `/root`
- [ ] `/var`
- [ ] `/tmp`
- [ ] `/usr`
- [ ] `/dev`
- [ ] `/proc`
- [ ] `/sys`
- [ ] `/opt`
- [ ] File Extensions
- [ ] Hidden Files and Directories
- [ ] Wildcards (`*`, `?`)
- [ ] File Compression
- [ ] `tar`
- [ ] `gzip` and `gunzip`
- [ ] `zip` and `unzip`
- [ ] Symbolic Links
- [ ] Hard Links
- [ ] Disk Partitions
- [ ] Mounting and Unmounting
- [ ] Filesystem Types (ext4, XFS, Btrfs)

**Resources:**
- [Linux Filesystem Hierarchy](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
- [Linux Journey – Filesystem](https://linuxjourney.com/lesson/filesystem-hierarchy)

## 4. Users and Permissions
- [ ] Linux User Management
- [ ] Root User
- [ ] Regular Users
- [ ] Groups and Group Management
- [ ] `useradd`
- [ ] `usermod`
- [ ] `userdel`
- [ ] `passwd`
- [ ] `groupadd`
- [ ] `groups`
- [ ] `whoami`
- [ ] `id`
- [ ] File Ownership
- [ ] `chown`
- [ ] `chgrp`
- [ ] File Permissions
- [ ] Read, Write and Execute Permissions
- [ ] `chmod`
- [ ] Numeric Permissions (755, 644, 777)
- [ ] Symbolic Permissions
- [ ] SUID, SGID and Sticky Bit
- [ ] `sudo`
- [ ] `/etc/passwd`
- [ ] `/etc/shadow`
- [ ] `/etc/group`
- [ ] Access Control Lists (ACLs)

**Resources:**
- [Linux Journey – Permissions](https://linuxjourney.com/lesson/file-permissions)
- [Ubuntu – User Management](https://documentation.ubuntu.com/server/how-to/security/user-management/)

## 5. Text Processing and Search Commands
- [ ] `grep`
- [ ] `egrep` and `fgrep`
- [ ] `sed`
- [ ] `awk`
- [ ] `cut`
- [ ] `sort`
- [ ] `uniq`
- [ ] `wc`
- [ ] `tr`
- [ ] `diff`
- [ ] `comm`
- [ ] `xargs`
- [ ] `tee`
- [ ] Regular Expressions
- [ ] Searching Files
- [ ] Searching Text Inside Files
- [ ] Pattern Matching
- [ ] Text Filtering
- [ ] Stream Processing

**Resources:**
- [GNU Grep Manual](https://www.gnu.org/software/grep/manual/)
- [GNU Sed Manual](https://www.gnu.org/software/sed/manual/)
- [GNU Awk Manual](https://www.gnu.org/software/gawk/manual/)

## 6. Shell and Bash Scripting
- [ ] Introduction to Shell
- [ ] Bash Shell
- [ ] Shell Types
- [ ] Shell Variables
- [ ] Environment Variables
- [ ] Local Variables
- [ ] Export Command
- [ ] Shell Script Creation
- [ ] Shebang (`#!/bin/bash`)
- [ ] Script Execution
- [ ] Comments
- [ ] User Input (`read`)
- [ ] Command-Line Arguments
- [ ] Positional Parameters (`$1`, `$2`)
- [ ] Special Variables (`$?`, `$$`, `$#`, `$@`)
- [ ] Conditional Statements (`if`, `else`, `elif`)
- [ ] Case Statements
- [ ] Loops (`for`, `while`, `until`)
- [ ] Functions
- [ ] Arrays
- [ ] String Manipulation
- [ ] Arithmetic Operations
- [ ] Exit Status
- [ ] Return Codes
- [ ] Command Substitution
- [ ] Process Substitution
- [ ] Pipes and Redirection
- [ ] Error Handling
- [ ] Debugging Shell Scripts
- [ ] Cron Jobs with Shell Scripts

**Resources:**
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
- [ShellCheck](https://www.shellcheck.net/)
- [Learn Shell](https://www.learnshell.org/)

## 7. Process Management
- [ ] Understanding Linux Processes
- [ ] Process IDs (PID)
- [ ] Parent and Child Processes
- [ ] Foreground and Background Processes
- [ ] Process States
- [ ] `ps`
- [ ] `top`
- [ ] `htop`
- [ ] `kill`
- [ ] `killall`
- [ ] `pkill`
- [ ] `nice`
- [ ] `renice`
- [ ] `jobs`
- [ ] `bg`
- [ ] `fg`
- [ ] `nohup`
- [ ] Signals (SIGTERM, SIGKILL, SIGHUP)
- [ ] Zombie Processes
- [ ] Orphan Processes
- [ ] Daemon Processes
- [ ] Process Scheduling

**Resources:**
- [Linux Journey – Processes](https://linuxjourney.com/lesson/processes)
- [Linux man-pages](https://man7.org/linux/man-pages/)

## 8. Package Management
- [ ] Linux Package Management
- [ ] APT Package Manager
- [ ] `apt update`
- [ ] `apt upgrade`
- [ ] `apt install`
- [ ] `apt remove`
- [ ] `apt purge`
- [ ] DPKG
- [ ] YUM and DNF
- [ ] RPM Package Manager
- [ ] Snap Packages
- [ ] Flatpak
- [ ] Repository Management
- [ ] Package Dependencies
- [ ] Installing Software from Source
- [ ] Updating and Removing Packages

**Resources:**
- [Ubuntu APT Documentation](https://documentation.ubuntu.com/server/how-to/software/package-management/)
- [DNF Documentation](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)

## 9. Networking in Linux
- [ ] Networking Fundamentals
- [ ] IP Address
- [ ] Public vs Private IP
- [ ] IPv4 and IPv6
- [ ] MAC Address
- [ ] DNS
- [ ] DHCP
- [ ] Default Gateway
- [ ] Subnet Mask and CIDR
- [ ] TCP vs UDP
- [ ] Ports and Protocols
- [ ] `ip`
- [ ] `ifconfig`
- [ ] `ping`
- [ ] `traceroute`
- [ ] `ss`
- [ ] `netstat`
- [ ] `curl`
- [ ] `wget`
- [ ] `nslookup`
- [ ] `dig`
- [ ] `hostname`
- [ ] `ssh`
- [ ] `scp`
- [ ] `rsync`
- [ ] Firewall Fundamentals
- [ ] UFW
- [ ] iptables and nftables
- [ ] Network Troubleshooting

**Resources:**
- [Linux Networking Documentation](https://docs.kernel.org/networking/)
- [Ubuntu Networking](https://documentation.ubuntu.com/server/how-to/networking/)
- [Linux Journey – Networking](https://linuxjourney.com/lesson/networking-basics)

## 10. Storage and Disk Management
- [ ] Linux Storage Architecture
- [ ] HDD vs SSD
- [ ] Disk Partitions
- [ ] `lsblk`
- [ ] `blkid`
- [ ] `df`
- [ ] `du`
- [ ] `fdisk`
- [ ] `parted`
- [ ] `mkfs`
- [ ] `mount`
- [ ] `umount`
- [ ] `/etc/fstab`
- [ ] Swap Space
- [ ] Logical Volume Management (LVM)
- [ ] RAID Fundamentals
- [ ] Disk Usage Monitoring
- [ ] Inodes
- [ ] Filesystem Repair

**Resources:**
- [Ubuntu Storage Documentation](https://documentation.ubuntu.com/server/how-to/storage/)
- [Linux man-pages](https://man7.org/linux/man-pages/)

## 11. System Administration
- [ ] Linux System Administration
- [ ] Boot Process
- [ ] BIOS and UEFI Basics
- [ ] GRUB Bootloader
- [ ] systemd
- [ ] systemctl
- [ ] Managing Services
- [ ] Starting and Stopping Services
- [ ] Enabling and Disabling Services
- [ ] Service Configuration
- [ ] Journalctl
- [ ] System Logs
- [ ] `/var/log`
- [ ] Log Rotation
- [ ] Cron and Crontab
- [ ] Scheduled Tasks
- [ ] System Monitoring
- [ ] Resource Monitoring
- [ ] CPU and Memory Usage
- [ ] Kernel Modules
- [ ] Kernel Updates
- [ ] System Shutdown and Reboot

**Resources:**
- [Ubuntu Server Documentation](https://documentation.ubuntu.com/server/)
- [systemd Documentation](https://systemd.io/)
- [Linux Journey – Boot Process](https://linuxjourney.com/lesson/boot-process)

## 12. Linux Security
- [ ] Linux Security Fundamentals
- [ ] User and Group Security
- [ ] File Permission Security
- [ ] SSH Security
- [ ] SSH Key Authentication
- [ ] Firewall Configuration
- [ ] UFW and iptables
- [ ] SELinux Fundamentals
- [ ] AppArmor
- [ ] Password Policies
- [ ] Sudoers Configuration
- [ ] Security Updates
- [ ] Log Monitoring
- [ ] Fail2ban
- [ ] Secure File Transfer
- [ ] Open Ports and Services
- [ ] Principle of Least Privilege
- [ ] File Integrity Monitoring

**Resources:**
- [Ubuntu Security Documentation](https://ubuntu.com/security)
- [SELinux Project](https://selinuxproject.org/)
- [AppArmor Documentation](https://apparmor.net/)

## 13. Linux for DevOps and Cloud Engineering
- [ ] Linux Server Fundamentals
- [ ] SSH into Remote Servers
- [ ] Remote Server Administration
- [ ] Environment Variables
- [ ] Bash Automation
- [ ] Process and Resource Monitoring
- [ ] Service Management
- [ ] Log Management
- [ ] Networking and Firewall
- [ ] Disk and Storage Management
- [ ] Docker Fundamentals
- [ ] Docker Installation on Linux
- [ ] Docker Images and Containers
- [ ] Docker Volumes
- [ ] Docker Networking
- [ ] Docker Compose
- [ ] Container Logs
- [ ] Kubernetes Fundamentals
- [ ] Linux Namespaces
- [ ] Control Groups (cgroups)
- [ ] Virtualization
- [ ] Cloud VM Management
- [ ] Nginx Basics
- [ ] Reverse Proxy
- [ ] Web Server Configuration
- [ ] CI/CD on Linux Servers

**Resources:**
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [DigitalOcean Tutorials](https://www.digitalocean.com/community/tutorials)
- [Linux Foundation Training](https://training.linuxfoundation.org/)

## 14. Linux Internals and Advanced Concepts
- [ ] Linux Kernel Architecture
- [ ] System Calls
- [ ] Kernel vs User Mode
- [ ] Process Scheduling
- [ ] Context Switching
- [ ] Virtual Memory
- [ ] Paging
- [ ] Memory Management
- [ ] File Descriptors
- [ ] Inodes and VFS
- [ ] Pipes and Named Pipes
- [ ] Inter-Process Communication (IPC)
- [ ] Signals
- [ ] Shared Memory
- [ ] Semaphores
- [ ] Mutexes
- [ ] Linux Namespaces
- [ ] Control Groups
- [ ] Kernel Modules
- [ ] Device Drivers
- [ ] `/proc` and `/sys`
- [ ] strace
- [ ] lsof
- [ ] ltrace

**Resources:**
- [Linux Kernel Documentation](https://docs.kernel.org/)
- [The Linux Programming Interface](https://man7.org/tlpi/)
- [Linux Insides](https://github.com/0xAX/linux-insides)

## Best Overall Learning Resources

| Resource | Best For |
|---|---|
| [Linux Journey](https://linuxjourney.com/) | Beginner to intermediate Linux |
| [Linux Command Library](https://linuxcommandlibrary.com/) | Command reference |
| [The Linux Documentation Project](https://tldp.org/) | Linux tutorials |
| [Ubuntu Documentation](https://documentation.ubuntu.com/) | Linux administration |
| [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)
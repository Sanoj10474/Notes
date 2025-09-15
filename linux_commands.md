# 📘 Linux Commands Cheat Sheet

A categorized reference guide for commonly used **Linux commands**.

---

## 🔹 File and Directory Management
- `pwd` → Show current directory path
- `ls` → List files in directory
- `ls -l` → Long listing format
- `ls -a` → Show hidden files
- `cd <dir>` → Change directory
- `cd ..` → Move one level up
- `mkdir <dir>` → Create new directory
- `rmdir <dir>` → Remove empty directory
- `rm -r <dir>` → Remove directory with contents
- `touch <file>` → Create empty file
- `cp <src> <dest>` → Copy file/directory
- `mv <src> <dest>` → Move or rename file/directory
- `rm <file>` → Delete file

---

## 🔹 File Viewing & Editing
- `cat <file>` → View file content
- `less <file>` → View file page by page
- `head <file>` → Show first 10 lines
- `tail <file>` → Show last 10 lines
- `tail -f <file>` → Continuously monitor file (logs)
- `nano <file>` → Open file in nano editor
- `vim <file>` → Open file in Vim editor

---

## 🔹 File Permissions & Ownership
- `ls -l` → Show permissions
- `chmod 755 <file>` → Change permissions
- `chmod +x <file>` → Add execute permission
- `chown user:group <file>` → Change owner
- `chgrp <group> <file>` → Change group

---

## 🔹 Process Management
- `ps` → Show processes
- `ps aux` → Detailed process info
- `top` → Real-time process monitor
- `htop` → Interactive process monitor
- `kill <pid>` → Kill process by PID
- `kill -9 <pid>` → Force kill
- `pkill <name>` → Kill process by name
- `jobs` → Show background jobs
- `fg %1` → Bring job to foreground
- `bg %1` → Run job in background

---

## 🔹 User Management
- `whoami` → Show current user
- `id` → Show user ID and group ID
- `who` → Show logged-in users
- `adduser <user>` → Add new user
- `passwd <user>` → Change password
- `su <user>` → Switch user
- `sudo <cmd>` → Run as superuser

---

## 🔹 Networking
- `ping <host>` → Check connectivity
- `ifconfig` → Show IP addresses (deprecated)
- `ip addr` → Show IP details
- `curl <url>` → Fetch data from URL
- `wget <url>` → Download file
- `scp <src> <user>@<host>:<path>` → Secure copy between hosts
- `ssh <user>@<host>` → Connect to remote host
- `netstat -tulnp` → Show active connections
- `ss -tulnp` → Alternative to netstat

---

## 🔹 Disk Management
- `df -h` → Show disk usage
- `du -sh <dir>` → Show size of directory
- `mount` → Show mounted filesystems
- `umount <path>` → Unmount device
- `lsblk` → List block devices
- `fdisk -l` → Show disk partitions

---

## 🔹 Package Management
### Debian/Ubuntu (APT)
- `apt update` → Update package list
- `apt upgrade` → Upgrade installed packages
- `apt install <pkg>` → Install package
- `apt remove <pkg>` → Remove package
- `apt search <pkg>` → Search package

### RedHat/CentOS (YUM/DNF)
- `yum install <pkg>` → Install package
- `yum remove <pkg>` → Remove package
- `yum update` → Update packages
- `dnf upgrade` → Upgrade packages

---

## 🔹 Search & Find
- `find /path -name <file>` → Find file by name
- `grep "text" <file>` → Search text in file
- `grep -r "text" <dir>` → Search recursively
- `locate <file>` → Find file quickly
- `which <cmd>` → Show command location
- `whereis <cmd>` → Show all command locations

---

## 🔹 Archiving & Compression
- `tar -cvf archive.tar <files>` → Create tar archive
- `tar -xvf archive.tar` → Extract tar archive
- `gzip <file>` → Compress file
- `gunzip <file.gz>` → Decompress file
- `zip archive.zip <files>` → Create zip archive
- `unzip archive.zip` → Extract zip archive

---

## 🔹 System Information
- `uname -a` → Show system info
- `hostname` → Show system hostname
- `uptime` → Show system uptime
- `dmesg | less` → Kernel boot messages
- `free -h` → Show memory usage
- `vmstat` → System performance
- `uptime` → Show how long system is running
- `lscpu` → CPU details
- `lsusb` → USB devices
- `lspci` → PCI devices

---

## 🔹 Disk & File Utilities
- `stat <file>` → Show file details
- `file <file>` → Show file type
- `md5sum <file>` → File checksum
- `sha256sum <file>` → File checksum

---

## 🔹 Shortcuts & Tricks
- `clear` → Clear terminal
- `history` → Show command history
- `!!` → Run last command
- `!<num>` → Run command from history
- `ctrl + c` → Kill current process
- `ctrl + z` → Suspend process
- `ctrl + d` → Logout / end input

---

✅ This Markdown file can be directly saved as **`linux_commands.md`** and used as a quick reference guide.


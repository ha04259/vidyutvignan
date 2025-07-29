# Complete Linux & DevOps Engineering Tutorial

## Table of Contents
1. [Linux Fundamentals for DevOps](#linux-fundamentals-for-devops)
2. [File System and Permissions](#file-system-and-permissions)
3. [Process Management](#process-management)
4. [Package Management](#package-management)
5. [System Services and Systemd](#system-services-and-systemd)
6. [Networking Fundamentals](#networking-fundamentals)
7. [Linux Networking Tools](#linux-networking-tools)
8. [Security and Firewall](#security-and-firewall)
9. [Essential Command Line Tools](#essential-command-line-tools)
10. [Text Processing and Search](#text-processing-and-search)
11. [System Monitoring and Performance](#system-monitoring-and-performance)
12. [Bash Scripting Fundamentals](#bash-scripting-fundamentals)
13. [Advanced Bash Scripting](#advanced-bash-scripting)
14. [Log Management](#log-management)
15. [Automation and Cron Jobs](#automation-and-cron-jobs)
16. [DevOps Best Practices](#devops-best-practices)

---

## Linux Fundamentals for DevOps

### Linux Distribution Overview
Popular distributions for DevOps:
- **Ubuntu/Debian**: User-friendly, extensive package repositories
- **CentOS/RHEL**: Enterprise-focused, stable, long-term support
- **Amazon Linux**: Optimized for AWS environments
- **Alpine Linux**: Minimal, security-focused, container-friendly

### Linux Architecture
```
┌─────────────────────────────────────┐
│         Applications                │
├─────────────────────────────────────┤
│         Shell/GUI                   │
├─────────────────────────────────────┤
│         System Libraries            │
├─────────────────────────────────────┤
│         Kernel                      │
├─────────────────────────────────────┤
│         Hardware                    │
└─────────────────────────────────────┘
```

### Key Concepts
- **Kernel**: Core of the operating system
- **Shell**: Command-line interface (bash, zsh, fish)
- **Process**: Running instance of a program
- **Thread**: Lightweight process within a process
- **Daemon**: Background service process

---

## File System and Permissions

### Linux File System Hierarchy
```
/                    # Root directory
├── bin/            # Essential user binaries
├── boot/           # Boot loader files
├── dev/            # Device files
├── etc/            # System configuration files
├── home/           # User home directories
├── lib/            # Essential shared libraries
├── media/          # Removable media mount points
├── mnt/            # Temporary mount points
├── opt/            # Optional software packages
├── proc/           # Process and kernel information
├── root/           # Root user home directory
├── run/            # Runtime data
├── sbin/           # System binaries
├── srv/            # Service data
├── sys/            # System information
├── tmp/            # Temporary files
├── usr/            # User utilities and applications
└── var/            # Variable data files
```

### File Operations
```bash
# Navigation
pwd                 # Print working directory
cd /path/to/dir    # Change directory
cd ~               # Go to home directory
cd -               # Go to previous directory

# Listing files
ls                 # List files
ls -la             # Long format with hidden files
ls -lh             # Human-readable file sizes
ls -lt             # Sort by modification time

# File manipulation
cp source dest     # Copy file
cp -r dir1 dir2    # Copy directory recursively
mv old new         # Move/rename file
rm file            # Remove file
rm -rf dir         # Remove directory recursively
mkdir dir          # Create directory
mkdir -p a/b/c     # Create nested directories
rmdir dir          # Remove empty directory
```

### File Permissions
```bash
# Permission format: rwxrwxrwx (owner group others)
# r=4, w=2, x=1

# Change permissions
chmod 755 file     # rwxr-xr-x
chmod +x script    # Add execute permission
chmod -w file      # Remove write permission
chmod u+rw file    # User read/write
chmod g-x file     # Remove group execute

# Change ownership
chown user:group file
chown -R user:group directory
chgrp group file   # Change group only

# View permissions
ls -l file
stat file
```

### Advanced File Operations
```bash
# Links
ln file hardlink          # Create hard link
ln -s target symlink      # Create symbolic link
readlink symlink          # Show symlink target

# File attributes
lsattr file              # List attributes
chattr +i file           # Make file immutable
chattr -i file           # Remove immutable attribute

# Find files
find /path -name "*.log"              # Find by name
find /path -type f -size +100M        # Find large files
find /path -mtime -7                  # Modified last 7 days
find /path -user username             # Find by owner
```

---

## Process Management

### Process Information
```bash
# View processes
ps aux                    # All processes
ps -ef                    # Full format
ps -u username           # User processes
pstree                   # Process tree
htop                     # Interactive process viewer
top                      # Real-time process monitor

# Process details
pgrep process_name       # Find process ID by name
pidof process_name       # Get PID of process
lsof -p PID             # Files opened by process
lsof filename           # Processes using file
```

### Process Control
```bash
# Start processes
command &               # Run in background
nohup command &         # Run immune to hangups
screen command          # Run in screen session
tmux                    # Terminal multiplexer

# Control processes
kill PID                # Terminate process
kill -9 PID            # Force kill process
killall process_name    # Kill all instances
pkill -f pattern       # Kill by pattern

# Job control
jobs                    # List background jobs
fg %1                   # Bring job to foreground
bg %1                   # Send job to background
disown %1              # Remove job from shell
```

### System Resources
```bash
# Memory usage
free -h                 # Human-readable memory info
cat /proc/meminfo      # Detailed memory information
vmstat                 # Virtual memory statistics

# CPU information
cat /proc/cpuinfo      # CPU details
lscpu                  # CPU architecture info
uptime                 # System load averages

# Disk usage
df -h                  # Disk space usage
du -sh directory       # Directory size
du -h --max-depth=1    # Subdirectory sizes
iostat                 # I/O statistics
```

---

## Package Management

### APT (Ubuntu/Debian)
```bash
# Update package lists
sudo apt update

# Upgrade packages
sudo apt upgrade
sudo apt full-upgrade

# Install packages
sudo apt install package_name
sudo apt install -y package_name    # Skip confirmation

# Remove packages
sudo apt remove package_name
sudo apt purge package_name         # Remove with config files
sudo apt autoremove                 # Remove unused dependencies

# Search packages
apt search keyword
apt show package_name

# Package files
dpkg -l                            # List installed packages
dpkg -L package_name               # List package files
dpkg -S /path/to/file             # Find package containing file
```

### YUM/DNF (RHEL/CentOS/Fedora)
```bash
# YUM (older systems)
sudo yum update
sudo yum install package_name
sudo yum remove package_name
yum search keyword
yum info package_name

# DNF (newer systems)
sudo dnf update
sudo dnf install package_name
sudo dnf remove package_name
dnf search keyword
dnf info package_name
```

### Package Compilation
```bash
# Install build tools
sudo apt install build-essential   # Ubuntu/Debian
sudo yum groupinstall "Development Tools"  # RHEL/CentOS

# Compile from source
./configure
make
sudo make install

# Create packages
checkinstall           # Create package from compiled source
```

---

## System Services and Systemd

### Systemd Service Management
```bash
# Service control
sudo systemctl start service_name
sudo systemctl stop service_name
sudo systemctl restart service_name
sudo systemctl reload service_name

# Service status
systemctl status service_name
systemctl is-active service_name
systemctl is-enabled service_name

# Enable/disable services
sudo systemctl enable service_name     # Start at boot
sudo systemctl disable service_name    # Don't start at boot

# List services
systemctl list-units --type=service
systemctl list-unit-files --type=service
```

### Creating Custom Services
```bash
# Create service file
sudo vim /etc/systemd/system/myapp.service
```

Example service file:
```ini
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
User=myuser
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/start.sh
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
# Reload systemd and start service
sudo systemctl daemon-reload
sudo systemctl enable myapp.service
sudo systemctl start myapp.service
```

### System Targets (Runlevels)
```bash
# View current target
systemctl get-default

# Change target
sudo systemctl set-default multi-user.target
sudo systemctl isolate graphical.target

# Common targets
# poweroff.target (0)
# rescue.target (1)
# multi-user.target (3)
# graphical.target (5)
# reboot.target (6)
```

---

## Networking Fundamentals

### OSI Model and TCP/IP Stack
```
Application Layer    │ HTTP, HTTPS, FTP, SSH, DNS
Presentation Layer   │ SSL/TLS, Encryption
Session Layer        │ NetBIOS, RPC
Transport Layer      │ TCP, UDP
Network Layer        │ IP, ICMP, OSPF
Data Link Layer      │ Ethernet, WiFi
Physical Layer       │ Cables, Switches, Hubs
```

### IP Addressing
```bash
# IPv4 Classes
Class A: 1.0.0.0    - 126.255.255.255  (/8)
Class B: 128.0.0.0  - 191.255.255.255  (/16)
Class C: 192.0.0.0  - 223.255.255.255  (/24)

# Private IP Ranges
10.0.0.0     - 10.255.255.255   (/8)
172.16.0.0   - 172.31.255.255   (/12)
192.168.0.0  - 192.168.255.255  (/16)

# CIDR Notation
192.168.1.0/24  = 256 IPs (192.168.1.0 - 192.168.1.255)
192.168.1.0/28  = 16 IPs  (192.168.1.0 - 192.168.1.15)
```

### Network Configuration
```bash
# View network interfaces
ip addr show
ip link show
ifconfig

# Configure interface
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up

# Routing
ip route show
sudo ip route add 192.168.2.0/24 via 192.168.1.1
sudo ip route del 192.168.2.0/24

# DNS configuration
cat /etc/resolv.conf
# Edit: nameserver 8.8.8.8

# Network interface configuration files
# Ubuntu/Debian: /etc/netplan/
# RHEL/CentOS: /etc/sysconfig/network-scripts/
```

---

## Linux Networking Tools

### Network Diagnostics
```bash
# Connectivity testing
ping google.com                    # Test connectivity
ping -c 4 google.com              # Send 4 packets
ping6 ipv6.google.com             # IPv6 ping

# Trace route
traceroute google.com
tracepath google.com
mtr google.com                     # Combined ping/traceroute

# Port testing
telnet hostname 80                 # Test TCP port
nc -zv hostname 80                 # Netcat port scan
nc -zv hostname 1-1000            # Scan port range
```

### Network Monitoring
```bash
# Active connections
netstat -tuln                     # Listening ports
netstat -tupln                    # With process names
ss -tuln                          # Modern replacement for netstat
ss -tupln                         # With process names

# Network traffic
iftop                             # Interface traffic
nethogs                          # Per-process bandwidth
tcpdump -i eth0                  # Packet capture
tcpdump -i eth0 port 80          # HTTP packets only
wireshark                        # GUI packet analyzer
```

### Network Services Testing
```bash
# HTTP/HTTPS testing
curl -I http://example.com        # Headers only
curl -v http://example.com        # Verbose output
curl -X POST -d "data" http://api.example.com
wget http://example.com/file.zip

# DNS testing
nslookup google.com
dig google.com
dig @8.8.8.8 google.com          # Specific DNS server
host google.com

# SSL/TLS testing
openssl s_client -connect google.com:443
```

### Firewall Management (iptables)
```bash
# View rules
sudo iptables -L
sudo iptables -L -n               # Numeric output
sudo iptables -L -v               # Verbose

# Basic rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -j DROP    # Drop all other input

# Save rules
sudo iptables-save > /etc/iptables/rules.v4

# UFW (Uncomplicated Firewall)
sudo ufw enable
sudo ufw allow 22
sudo ufw allow 80/tcp
sudo ufw deny from 192.168.1.100
sudo ufw status
```

---

## Security and Firewall

### User and Group Management
```bash
# User management
sudo useradd -m -s /bin/bash username
sudo passwd username
sudo userdel -r username
sudo usermod -aG sudo username     # Add to sudo group

# Group management
sudo groupadd groupname
sudo groupdel groupname
sudo usermod -aG groupname username

# Switch users
su - username
sudo -u username command
sudo -i                           # Root shell
```

### SSH Configuration
```bash
# SSH key generation
ssh-keygen -t ed25519 -C "email@example.com"
ssh-keygen -t rsa -b 4096

# Copy SSH key
ssh-copy-id user@hostname
cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> ~/.ssh/authorized_keys'

# SSH configuration (/etc/ssh/sshd_config)
Port 2222                         # Change default port
PermitRootLogin no               # Disable root login
PasswordAuthentication no        # Key-only authentication
AllowUsers user1 user2           # Allow specific users

# Restart SSH service
sudo systemctl restart sshd
```

### File System Security
```bash
# Set secure permissions
chmod 600 ~/.ssh/id_rsa          # Private key
chmod 644 ~/.ssh/id_rsa.pub      # Public key
chmod 700 ~/.ssh                 # SSH directory

# Find security issues
find / -perm -4000 2>/dev/null   # Find SUID files
find / -perm -2000 2>/dev/null   # Find SGID files
find / -perm -002 2>/dev/null    # World-writable files
```

### System Hardening
```bash
# Disable unused services
sudo systemctl disable telnet
sudo systemctl disable ftp

# Update system regularly
sudo apt update && sudo apt upgrade

# Configure automatic updates
sudo apt install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades

# Monitor system logs
sudo tail -f /var/log/auth.log    # Authentication logs
sudo tail -f /var/log/syslog      # System logs
```

---

## Essential Command Line Tools

### File Viewing and Editing
```bash
# View files
cat file.txt                     # Display entire file
less file.txt                    # Paginated view
head -n 20 file.txt             # First 20 lines
tail -n 20 file.txt             # Last 20 lines
tail -f file.txt                # Follow file changes

# Edit files
nano file.txt                   # Simple editor
vim file.txt                    # Advanced editor
emacs file.txt                  # Emacs editor

# Compare files
diff file1 file2
diff -u file1 file2             # Unified format
cmp file1 file2                 # Binary comparison
```

### Archive and Compression
```bash
# tar archives
tar -czf archive.tar.gz directory/    # Create compressed archive
tar -xzf archive.tar.gz               # Extract compressed archive
tar -tzf archive.tar.gz               # List archive contents

# zip archives
zip -r archive.zip directory/
unzip archive.zip
unzip -l archive.zip              # List contents

# Other compression
gzip file.txt                     # Compress file
gunzip file.txt.gz               # Decompress file
```

### System Information
```bash
# System details
uname -a                         # System information
hostnamectl                      # Host information
lsb_release -a                   # Distribution info
cat /etc/os-release              # OS information

# Hardware information
lscpu                           # CPU information
lsmem                           # Memory information
lsblk                           # Block devices
lsusb                           # USB devices
lspci                           # PCI devices
```

---

## Text Processing and Search

### grep - Pattern Matching
```bash
# Basic usage
grep pattern file.txt
grep -i pattern file.txt         # Case-insensitive
grep -v pattern file.txt         # Invert match
grep -n pattern file.txt         # Show line numbers
grep -c pattern file.txt         # Count matches

# Extended regex
grep -E "pattern1|pattern2" file.txt
grep -E "^start" file.txt        # Lines starting with "start"
grep -E "end$" file.txt          # Lines ending with "end"

# Recursive search
grep -r pattern directory/
grep -r --include="*.log" pattern /var/log/
```

### sed - Stream Editor
```bash
# Substitution
sed 's/old/new/' file.txt        # Replace first occurrence
sed 's/old/new/g' file.txt       # Replace all occurrences
sed 's/old/new/gi' file.txt      # Case-insensitive replacement

# In-place editing
sed -i 's/old/new/g' file.txt

# Delete lines
sed '/pattern/d' file.txt        # Delete matching lines
sed '1d' file.txt                # Delete first line
sed '$d' file.txt                # Delete last line

# Print specific lines
sed -n '10,20p' file.txt         # Print lines 10-20
sed -n '/pattern/p' file.txt     # Print matching lines
```

### awk - Text Processing
```bash
# Basic usage
awk '{print $1}' file.txt        # Print first column
awk '{print $1, $3}' file.txt    # Print columns 1 and 3
awk '{print NF}' file.txt        # Print number of fields

# Field separator
awk -F: '{print $1}' /etc/passwd  # Use colon as separator
awk -F, '{print $2}' file.csv     # CSV processing

# Conditional processing
awk '$3 > 1000 {print $1}' file.txt
awk '/pattern/ {print $0}' file.txt
awk 'NR==10 {print}' file.txt     # Print line 10

# Built-in variables
# NR - Number of records (line number)
# NF - Number of fields
# FS - Field separator
# RS - Record separator
```

### sort and uniq
```bash
# Sorting
sort file.txt                    # Alphabetical sort
sort -n file.txt                 # Numerical sort
sort -r file.txt                 # Reverse sort
sort -k2 file.txt                # Sort by second column
sort -t: -k3 -n /etc/passwd      # Sort passwd by UID

# Remove duplicates
sort file.txt | uniq
sort file.txt | uniq -c          # Count occurrences
sort file.txt | uniq -d          # Show only duplicates
```

### cut and paste
```bash
# Extract columns
cut -d: -f1 /etc/passwd          # First field, colon delimiter
cut -c1-10 file.txt              # Characters 1-10
cut -f1,3 file.txt               # Fields 1 and 3

# Combine files
paste file1.txt file2.txt        # Side by side
paste -d, file1.txt file2.txt    # With comma separator
```

---

## System Monitoring and Performance

### System Load and Performance
```bash
# Load averages
uptime
cat /proc/loadavg
w                                # Who is logged in and load

# CPU usage
top                              # Real-time process monitor
htop                             # Enhanced top
iostat 1                         # I/O statistics every second
vmstat 1                         # Virtual memory stats
sar -u 1 10                      # CPU utilization

# Memory analysis
free -h
cat /proc/meminfo
pmap PID                         # Process memory map
```

### Disk and I/O Monitoring
```bash
# Disk usage
df -h                            # File system usage
du -sh /*                        # Directory sizes
ncdu /                           # Interactive disk usage

# Disk I/O
iostat -x 1                      # Extended I/O stats
iotop                            # I/O by process
lsof +D /path                    # Files open in directory

# Block devices
lsblk                            # List block devices
blkid                            # Block device attributes
fdisk -l                         # Partition tables
```

### Network Monitoring
```bash
# Interface statistics
cat /proc/net/dev
ip -s link                       # Interface stats
ifstat                           # Interface bandwidth

# Connection monitoring
netstat -i                       # Interface statistics
ss -s                           # Socket statistics
```

### Log Analysis
```bash
# System logs
journalctl                       # systemd logs
journalctl -f                    # Follow logs
journalctl -u servicename       # Service-specific logs
journalctl --since "1 hour ago"

# Traditional logs
tail -f /var/log/syslog
tail -f /var/log/messages
grep ERROR /var/log/syslog
```

---

## Bash Scripting Fundamentals

### Script Structure and Basics
```bash
#!/bin/bash
# Shebang line - specifies interpreter

# Comments start with #
echo "Hello, World!"              # Print to stdout

# Variables
name="John"
age=25
echo "Name: $name, Age: $age"

# Command substitution
current_date=$(date)
user_count=`who | wc -l`

# Environment variables
echo "Home directory: $HOME"
echo "Current user: $USER"
echo "Shell: $SHELL"
```

### User Input and Arguments
```bash
#!/bin/bash

# Command line arguments
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"

# User input
echo "Enter your name:"
read name
echo "Hello, $name!"

# Read with prompt
read -p "Enter your age: " age

# Read password (hidden)
read -s -p "Enter password: " password
```

### Variables and Data Types
```bash
#!/bin/bash

# String variables
string1="Hello"
string2='World'
combined="$string1 $string2"

# Integer variables
num1=10
num2=20
sum=$((num1 + num2))

# Arrays
fruits=("apple" "banana" "orange")
echo "First fruit: ${fruits[0]}"
echo "All fruits: ${fruits[@]}"
echo "Array length: ${#fruits[@]}"

# Associative arrays (bash 4+)
declare -A colors
colors[red]="#FF0000"
colors[green]="#00FF00"
echo "Red color: ${colors[red]}"
```

### Conditional Statements
```bash
#!/bin/bash

# If statement
if [ $# -eq 0 ]; then
    echo "No arguments provided"
elif [ $# -eq 1 ]; then
    echo "One argument provided: $1"
else
    echo "Multiple arguments provided"
fi

# Test conditions
file="/etc/passwd"
if [ -f "$file" ]; then
    echo "$file exists and is a regular file"
fi

if [ -d "/tmp" ]; then
    echo "/tmp is a directory"
fi

# Numeric comparisons
num=10
if [ $num -gt 5 ]; then
    echo "$num is greater than 5"
fi

# String comparisons
string1="hello"
string2="world"
if [ "$string1" = "$string2" ]; then
    echo "Strings are equal"
else
    echo "Strings are different"
fi

# Case statement
case $1 in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
esac
```

### Loops
```bash
#!/bin/bash

# For loop with list
for item in apple banana orange; do
    echo "Fruit: $item"
done

# For loop with range
for i in {1..10}; do
    echo "Number: $i"
done

# For loop with command output
for file in $(ls *.txt); do
    echo "Processing: $file"
done

# While loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Until loop
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Reading file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt
```

### Functions
```bash
#!/bin/bash

# Function definition
greet() {
    echo "Hello, $1!"
}

# Function with return value
add_numbers() {
    local num1=$1
    local num2=$2
    local sum=$((num1 + num2))
    echo $sum
}

# Function with local variables
process_file() {
    local filename=$1
    if [ -f "$filename" ]; then
        local line_count=$(wc -l < "$filename")
        echo "File $filename has $line_count lines"
    else
        echo "File $filename not found"
        return 1
    fi
}

# Function calls
greet "World"
result=$(add_numbers 10 20)
echo "Sum: $result"
process_file "/etc/passwd"
```

---

## Advanced Bash Scripting

### Error Handling and Debugging
```bash
#!/bin/bash

# Exit on error
set -e                           # Exit on any error
set -u                          # Exit on undefined variable
set -x                          # Print commands as executed
set -o pipefail                 # Exit on pipe failure

# Custom error handling
error_exit() {
    echo "Error: $1" >&2
    exit 1
}

# Check command success
if ! command -v docker &> /dev/null; then
    error_exit "Docker is not installed"
fi

# Trap signals
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/script.$$
}
trap cleanup EXIT

# Logging function
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1"
}

log "Script started"
```

### File and Directory Operations
```bash
#!/bin/bash

# Check file/directory existence
check_file() {
    local file=$1
    if [ -f "$file" ]; then
        echo "$file is a regular file"
    elif [ -d "$file" ]; then
        echo "$file is a directory"
    elif [ -L "$file" ]; then
        echo "$file is a symbolic link"
    else
        echo "$file does not exist"
    fi
}

# Create backup
backup_file() {
    local file=$1
    local backup="${file}.bak.$(date +%Y%m%d_%H%M%S)"
    cp "$file" "$backup"
    echo "Backup created: $backup"
}

# Find and process files
find /var/log -name "*.log" -mtime +7 | while read -r logfile; do
    echo "Archiving old log: $logfile"
    gzip "$logfile"
done
```

### Text Processing in Scripts
```bash
#!/bin/bash

# Parse configuration file
parse_config() {
    local config_file=$1
    while IFS='=' read -r key value; do
        # Skip comments and empty lines
        [[ $key =~ ^[[:space:]]*# ]] && continue
        [[ -z $key ]] && continue
        
        # Remove leading/trailing whitespace
        key=$(echo "$key" | xargs)
        value=$(echo "$value" | xargs)
        
        echo "Config: $key = $value"
    done < "$config_file"
}

# Process CSV file
process_csv() {
    local csv_file=$1
    local line_num=0
    
    while IFS=',' read -r col1 col2 col3; do
        ((line_num++))
        if [ $line_num -eq 1 ]; then
            echo "Headers: $col1, $col2, $col3"
        else
            echo "Data row $((line_num-1)): $col1 | $col2 | $col3"
        fi
    done < "$csv_file"
}
```

### Network Operations
```bash
#!/bin/bash

# Check network connectivity
check_connectivity() {
    local host=$1
    local port=${2:-80}
    
    if timeout 5 bash -c "</dev/tcp/$host/$port"; then
        echo "Connection to $host:$port successful"
        return 0
    else
        echo "Connection to $host:$port failed"
        return 1
    fi
}

# Download file with retry
download_with_retry() {
    local url=$1
    local output=$2
    local max_attempts=3
    local attempt=1
    
    while [ $attempt -le $max_attempts ]; do
        echo "Download attempt $attempt of $max_attempts"
        if curl -L -o "$output" "$url"; then
            echo "Download successful"
            return 0
        else
            echo "Download failed"
            ((attempt++))
            sleep 5
        fi
    done
    
    return 1
}

# API request
api_request() {
    local endpoint=$1
    local method=${2:-GET}
    local data=${3:-""}
    
    if [ "$method" = "POST" ] && [ -n "$data" ]; then
        curl -X POST -H "Content-Type: application/json" -d "$data" "$endpoint"
    else
        curl -X "$method" "$endpoint"
    fi
}
```

### System Administration Tasks
```bash
#!/bin/bash

# Service management
manage_service() {
    local service=$1
    local action=${2:-status}
    
    case $action in
        start|stop|restart|status)
            echo "Managing service $service: $action"
            systemctl $action $service
            ;;
        enable|disable)
            echo "Setting service $service to $action"
            systemctl $action $service
            ;;
        *)
            echo "Invalid action: $action"
            echo "Usage: manage_service <service> <start|stop|restart|status|enable|disable>"
            return 1
            ;;
    esac
}

# System cleanup
system_cleanup() {
    echo "Starting system cleanup..."
    
    # Clear package cache
    echo "Clearing package cache..."
    apt autoremove -y
    apt autoclean
    
    # Clear log files older than 30 days
    echo "Cleaning old log files..."
    find /var/log -name "*.log" -mtime +30 -delete
    
    # Clear temporary files
    echo "Cleaning temporary files..."
    find /tmp -type f -mtime +7 -delete
    
    # Clear user cache
    echo "Cleaning user cache..."
    find /home -name ".cache" -type d -exec rm -rf {} + 2>/dev/null
    
    echo "System cleanup completed"
}

# Disk space monitoring
check_disk_space() {
    local threshold=${1:-90}
    
    df -h | awk -v threshold=$threshold '
    NR>1 {
        usage = int($5)
        if (usage > threshold) {
            print "WARNING: " $1 " is " usage "% full (mounted on " $6 ")"
        }
    }'
}

# User management
create_user_with_ssh() {
    local username=$1
    local ssh_key=$2
    
    if id "$username" &>/dev/null; then
        echo "User $username already exists"
        return 1
    fi
    
    # Create user
    useradd -m -s /bin/bash "$username"
    
    # Setup SSH key
    local ssh_dir="/home/$username/.ssh"
    mkdir -p "$ssh_dir"
    echo "$ssh_key" > "$ssh_dir/authorized_keys"
    
    # Set permissions
    chown -R "$username:$username" "$ssh_dir"
    chmod 700 "$ssh_dir"
    chmod 600 "$ssh_dir/authorized_keys"
    
    echo "User $username created with SSH key access"
}
```

---

## Log Management

### Log File Locations
```bash
# System logs
/var/log/syslog          # General system messages
/var/log/messages        # General system messages (RHEL/CentOS)
/var/log/auth.log        # Authentication logs
/var/log/secure          # Authentication logs (RHEL/CentOS)
/var/log/kern.log        # Kernel messages
/var/log/cron.log        # Cron job logs

# Service logs
/var/log/apache2/        # Apache web server
/var/log/nginx/          # Nginx web server
/var/log/mysql/          # MySQL database
/var/log/postgresql/     # PostgreSQL database
```

### Log Analysis Commands
```bash
# View logs
tail -f /var/log/syslog              # Follow log in real-time
head -100 /var/log/auth.log          # First 100 lines
less +F /var/log/syslog              # Follow mode in less

# Search logs
grep "ERROR" /var/log/syslog
grep -i "failed" /var/log/auth.log   # Case-insensitive
grep -v "INFO" /var/log/app.log      # Exclude INFO messages

# Time-based filtering
grep "$(date '+%b %d')" /var/log/syslog        # Today's logs
grep "Dec 25" /var/log/syslog                  # Specific date

# Count occurrences
grep -c "ERROR" /var/log/app.log
grep "failed login" /var/log/auth.log | wc -l
```

### Log Rotation with logrotate
```bash
# Main configuration
cat /etc/logrotate.conf

# Service-specific configurations
ls /etc/logrotate.d/

# Custom log rotation configuration
sudo vim /etc/logrotate.d/myapp
```

Example logrotate configuration:
```
/var/log/myapp/*.log {
    daily
    missingok
    rotate 52
    compress
    delaycompress
    notifempty
    create 644 myapp myapp
    postrotate
        systemctl reload myapp
    endscript
}
```

### Centralized Logging with rsyslog
```bash
# Main configuration
sudo vim /etc/rsyslog.conf

# Custom rules
sudo vim /etc/rsyslog.d/50-default.conf
```

Example rsyslog configuration:
```
# Send all logs to remote server
*.* @@logserver.example.com:514

# Local file logging
$ModLoad imfile
$InputFileName /var/log/myapp/app.log
$InputFileTag myapp:
$InputFileStateFile stat-myapp
$InputFileSeverity info
$InputRunFileMonitor

# Filter and forward
:programname, isequal, "myapp" @@logserver:514
& stop
```

### Log Analysis Scripts
```bash
#!/bin/bash

# Analyze access logs
analyze_access_log() {
    local logfile=$1
    
    echo "=== Access Log Analysis ==="
    echo "Total requests: $(wc -l < "$logfile")"
    echo
    
    echo "Top 10 IP addresses:"
    awk '{print $1}' "$logfile" | sort | uniq -c | sort -rn | head -10
    echo
    
    echo "Top 10 requested URLs:"
    awk '{print $7}' "$logfile" | sort | uniq -c | sort -rn | head -10
    echo
    
    echo "HTTP status codes:"
    awk '{print $9}' "$logfile" | sort | uniq -c | sort -rn
    echo
    
    echo "Error requests (4xx, 5xx):"
    awk '$9 ~ /^[45]/ {print $9, $7}' "$logfile" | sort | uniq -c | sort -rn
}

# Monitor log for patterns
monitor_log() {
    local logfile=$1
    local pattern=$2
    
    echo "Monitoring $logfile for pattern: $pattern"
    tail -F "$logfile" | while read line; do
        if echo "$line" | grep -q "$pattern"; then
            echo "[ALERT] $(date): $line"
            # Send notification, email, etc.
        fi
    done
}

# Generate log summary
log_summary() {
    local logfile=$1
    local output="/tmp/log_summary_$(date +%Y%m%d).txt"
    
    {
        echo "Log Summary for $(basename "$logfile")"
        echo "Generated: $(date)"
        echo "=========================="
        echo
        
        echo "Total lines: $(wc -l < "$logfile")"
        echo "File size: $(du -h "$logfile" | cut -f1)"
        echo
        
        echo "Error counts:"
        grep -i error "$logfile" | wc -l
        echo
        
        echo "Warning counts:"
        grep -i warning "$logfile" | wc -l
        echo
        
        echo "Recent errors (last 10):"
        grep -i error "$logfile" | tail -10
        
    } > "$output"
    
    echo "Summary saved to: $output"
}
```

---

## Automation and Cron Jobs

### Cron Syntax and Usage
```bash
# Cron time format
# * * * * * command
# │ │ │ │ │
# │ │ │ │ └─── Day of week (0-7, Sunday = 0 or 7)
# │ │ │ └───── Month (1-12)
# │ │ └─────── Day of month (1-31)
# │ └───────── Hour (0-23)
# └─────────── Minute (0-59)

# Edit crontab
crontab -e                       # Edit current user's crontab
sudo crontab -e                  # Edit root's crontab
crontab -l                       # List current crontab
crontab -r                       # Remove crontab

# System-wide cron directories
/etc/cron.hourly/               # Scripts run hourly
/etc/cron.daily/                # Scripts run daily
/etc/cron.weekly/               # Scripts run weekly
/etc/cron.monthly/              # Scripts run monthly
```

### Common Cron Examples
```bash
# Run every minute
* * * * * /path/to/script.sh

# Run at 2:30 AM daily
30 2 * * * /path/to/backup.sh

# Run every 15 minutes
*/15 * * * * /path/to/monitor.sh

# Run every Sunday at 3 AM
0 3 * * 0 /path/to/weekly-task.sh

# Run on weekdays at 9 AM
0 9 * * 1-5 /path/to/weekday-task.sh

# Run first day of every month
0 0 1 * * /path/to/monthly-report.sh

# Multiple times per day
0 6,12,18 * * * /path/to/task.sh

# Run during business hours
0 9-17 * * 1-5 /path/to/business-task.sh
```

### Cron Best Practices
```bash
#!/bin/bash

# Cron-friendly script template
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
LOG_FILE="/var/log/mycron.log"

# Set PATH explicitly
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Redirect output to log file
exec 1>> "$LOG_FILE"
exec 2>> "$LOG_FILE"

# Log start time
echo "[$(date)] Starting cron job: $0"

# Lock file to prevent concurrent runs
LOCK_FILE="/tmp/$(basename "$0").lock"
exec 200>"$LOCK_FILE"

if ! flock -n 200; then
    echo "[$(date)] Script already running, exiting"
    exit 1
fi

# Cleanup function
cleanup() {
    flock -u 200
    rm -f "$LOCK_FILE"
    echo "[$(date)] Cron job completed: $0"
}
trap cleanup EXIT

# Your script logic here
echo "[$(date)] Executing main task..."
```

### System Automation Scripts
```bash
#!/bin/bash

# Automated backup script
automated_backup() {
    local source_dir=$1
    local backup_dir=$2
    local retention_days=${3:-30}
    
    # Create backup directory if it doesn't exist
    mkdir -p "$backup_dir"
    
    # Create backup with timestamp
    local timestamp=$(date +%Y%m%d_%H%M%S)
    local backup_file="$backup_dir/backup_$timestamp.tar.gz"
    
    echo "Creating backup: $backup_file"
    tar -czf "$backup_file" -C "$(dirname "$source_dir")" "$(basename "$source_dir")"
    
    if [ $? -eq 0 ]; then
        echo "Backup created successfully"
        
        # Remove old backups
        find "$backup_dir" -name "backup_*.tar.gz" -mtime +$retention_days -delete
        echo "Old backups cleaned up (older than $retention_days days)"
    else
        echo "Backup failed"
        return 1
    fi
}

# System health check
system_health_check() {
    local alert_email=${1:-"admin@example.com"}
    local temp_file="/tmp/health_check.$"
    
    {
        echo "System Health Check - $(date)"
        echo "================================"
        echo
        
        # Check disk space
        echo "Disk Usage:"
        df -h | awk '$5 > 80 {print "WARNING: " $0}'
        echo
        
        # Check memory usage
        echo "Memory Usage:"
        free -h
        echo
        
        # Check load average
        echo "Load Average:"
        uptime
        echo
        
        # Check failed services
        echo "Failed Services:"
        systemctl --failed --no-legend
        echo
        
        # Check recent errors in logs
        echo "Recent Errors (last hour):"
        journalctl --since "1 hour ago" --priority=err --no-pager
        
    } > "$temp_file"
    
    # Send email if issues found
    if grep -q "WARNING\|FAILED\|error" "$temp_file"; then
        mail -s "System Health Alert - $(hostname)" "$alert_email" < "$temp_file"
    fi
    
    rm -f "$temp_file"
}

# Database backup automation
db_backup() {
    local db_name=$1
    local backup_dir="/backup/databases"
    local retention_days=7
    
    mkdir -p "$backup_dir"
    
    local timestamp=$(date +%Y%m%d_%H%M%S)
    local backup_file="$backup_dir/${db_name}_$timestamp.sql"
    
    # MySQL backup
    mysqldump -u backup_user -p"$DB_PASSWORD" "$db_name" > "$backup_file"
    
    if [ $? -eq 0 ]; then
        gzip "$backup_file"
        echo "Database backup completed: ${backup_file}.gz"
        
        # Cleanup old backups
        find "$backup_dir" -name "${db_name}_*.sql.gz" -mtime +$retention_days -delete
    else
        echo "Database backup failed"
        return 1
    fi
}
```

### at Command for One-time Jobs
```bash
# Schedule one-time job
at 15:30                         # Run at 3:30 PM today
at 15:30 tomorrow               # Run at 3:30 PM tomorrow
at now + 2 hours                # Run in 2 hours
at midnight                     # Run at midnight

# Example at job
echo "/path/to/script.sh" | at now + 5 minutes

# List scheduled jobs
atq

# Remove scheduled job
atrm job_number

# View job details
at -c job_number
```

---

## DevOps Best Practices

### Infrastructure as Code Principles
```bash
# Configuration management with version control
# Store all configurations in Git repositories
mkdir -p /etc/ansible/playbooks
cd /etc/ansible/playbooks
git init
git add .
git commit -m "Initial configuration"

# Environment-specific configurations
├── environments/
│   ├── development/
│   ├── staging/
│   └── production/
├── roles/
├── playbooks/
└── group_vars/
```

### Monitoring and Alerting Setup
```bash
#!/bin/bash

# System monitoring script
monitor_system() {
    local config_file="/etc/monitoring.conf"
    
    # Load configuration
    source "$config_file"
    
    # Check CPU usage
    cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | awk -F'%' '{print $1}')
    if (( $(echo "$cpu_usage > $CPU_THRESHOLD" | bc -l) )); then
        send_alert "High CPU usage: ${cpu_usage}%"
    fi
    
    # Check memory usage
    mem_usage=$(free | grep Mem | awk '{printf("%.2f", $3/$2 * 100.0)}')
    if (( $(echo "$mem_usage > $MEM_THRESHOLD" | bc -l) )); then
        send_alert "High memory usage: ${mem_usage}%"
    fi
    
    # Check disk usage
    while read disk usage; do
        if [ "${usage%?}" -gt "$DISK_THRESHOLD" ]; then
            send_alert "High disk usage on $disk: $usage"
        fi
    done < <(df -h | awk 'NR>1 {print $1, $5}')
}

# Alert function
send_alert() {
    local message=$1
    local timestamp=$(date)
    
    # Log alert
    echo "[$timestamp] ALERT: $message" >> /var/log/monitoring.log
    
    # Send email notification
    echo "$message" | mail -s "System Alert - $(hostname)" "$ADMIN_EMAIL"
    
    # Send to Slack (if webhook configured)
    if [ -n "$SLACK_WEBHOOK" ]; then
        curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"'"$message"'"}' \
            "$SLACK_WEBHOOK"
    fi
}
```

### Security Hardening Scripts
```bash
#!/bin/bash

# System hardening script
harden_system() {
    echo "Starting system hardening..."
    
    # Update system
    apt update && apt upgrade -y
    
    # Install security updates automatically
    apt install -y unattended-upgrades
    dpkg-reconfigure -plow unattended-upgrades
    
    # Configure SSH security
    cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
    sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
    sed -i 's/#Port 22/Port 2222/' /etc/ssh/sshd_config
    systemctl restart sshd
    
    # Configure firewall
    ufw --force enable
    ufw default deny incoming
    ufw default allow outgoing
    ufw allow 2222/tcp  # SSH on custom port
    ufw allow 80/tcp    # HTTP
    ufw allow 443/tcp   # HTTPS
    
    # Install and configure fail2ban
    apt install -y fail2ban
    cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
    
    # Set up log monitoring
    cat > /etc/fail2ban/jail.local << EOF
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
EOF
    
    systemctl enable fail2ban
    systemctl start fail2ban
    
    # Remove unnecessary packages
    apt autoremove -y
    
    # Set file permissions
    chmod 644 /etc/passwd
    chmod 600 /etc/shadow
    chmod 644 /etc/group
    
    echo "System hardening completed"
}

# Audit system security
security_audit() {
    echo "=== Security Audit Report ==="
    echo "Generated: $(date)"
    echo
    
    # Check for users with empty passwords
    echo "Users with empty passwords:"
    awk -F: '$2 == "" {print $1}' /etc/shadow
    echo
    
    # Check for SUID files
    echo "SUID files:"
    find / -perm -4000 -type f 2>/dev/null
    echo
    
    # Check listening services
    echo "Listening services:"
    netstat -tuln
    echo
    
    # Check failed login attempts
    echo "Recent failed login attempts:"
    grep "Failed password" /var/log/auth.log | tail -10
    echo
    
    # Check sudo usage
    echo "Recent sudo usage:"
    grep "sudo:" /var/log/auth.log | tail -10
}
```

### Deployment Automation
```bash
#!/bin/bash

# Application deployment script
deploy_application() {
    local app_name=$1
    local version=$2
    local environment=${3:-staging}
    
    echo "Deploying $app_name version $version to $environment"
    
    # Pre-deployment checks
    if ! pre_deployment_checks; then
        echo "Pre-deployment checks failed"
        exit 1
    fi
    
    # Create backup
    backup_current_version "$app_name"
    
    # Download and deploy new version
    deploy_new_version "$app_name" "$version"
    
    # Run database migrations if needed
    if [ -f "/opt/$app_name/migrate.sh" ]; then
        echo "Running database migrations..."
        /opt/"$app_name"/migrate.sh
    fi
    
    # Restart services
    systemctl restart "$app_name"
    
    # Post-deployment verification
    if post_deployment_checks "$app_name"; then
        echo "Deployment successful"
        cleanup_old_versions "$app_name"
    else
        echo "Deployment verification failed, rolling back"
        rollback_deployment "$app_name"
        exit 1
    fi
}

pre_deployment_checks() {
    # Check system resources
    if [ $(df / | awk 'NR==2 {print $5}' | sed 's/%//') -gt 90 ]; then
        echo "Insufficient disk space"
        return 1
    fi
    
    # Check if required services are running
    for service in nginx mysql redis; do
        if ! systemctl is-active "$service" &>/dev/null; then
            echo "Service $service is not running"
            return 1
        fi
    done
    
    return 0
}

post_deployment_checks() {
    local app_name=$1
    
    # Wait for application to start
    sleep 10
    
    # Check if application is responding
    if curl -f "http://localhost:8080/health" &>/dev/null; then
        echo "Application health check passed"
        return 0
    else
        echo "Application health check failed"
        return 1
    fi
}

rollback_deployment() {
    local app_name=$1
    local backup_dir="/backup/$app_name"
    local latest_backup=$(ls -t "$backup_dir" | head -1)
    
    echo "Rolling back to $latest_backup"
    
    systemctl stop "$app_name"
    rm -rf "/opt/$app_name"
    tar -xzf "$backup_dir/$latest_backup" -C /opt/
    systemctl start "$app_name"
    
    echo "Rollback completed"
}
```

### Container and Docker Integration
```bash
#!/bin/bash

# Docker management for DevOps
docker_deploy() {
    local image=$1
    local container_name=$2
    local port=${3:-8080}
    
    # Pull latest image
    docker pull "$image"
    
    # Stop and remove existing container
    docker stop "$container_name" 2>/dev/null || true
    docker rm "$container_name" 2>/dev/null || true
    
    # Run new container
    docker run -d \
        --name "$container_name" \
        --restart unless-stopped \
        -p "$port:8080" \
        -v /opt/app/logs:/app/logs \
        -v /opt/app/config:/app/config:ro \
        "$image"
    
    # Health check
    sleep 10
    if docker ps | grep -q "$container_name"; then
        echo "Container $container_name deployed successfully"
    else
        echo "Container deployment failed"
        docker logs "$container_name"
        return 1
    fi
}

# Docker system maintenance
docker_cleanup() {
    echo "Cleaning up Docker system..."
    
    # Remove stopped containers
    docker container prune -f
    
    # Remove unused images
    docker image prune -f
    
    # Remove unused volumes
    docker volume prune -f
    
    # Remove unused networks
    docker network prune -f
    
    # Show space saved
    docker system df
}

# Container monitoring
monitor_containers() {
    echo "=== Container Status ==="
    docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
    echo
    
    echo "=== Container Resource Usage ==="
    docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
    echo
    
    # Check for unhealthy containers
    unhealthy=$(docker ps --filter health=unhealthy --format "{{.Names}}")
    if [ -n "$unhealthy" ]; then
        echo "WARNING: Unhealthy containers detected:"
        echo "$unhealthy"
    fi
}
```

This comprehensive tutorial covers all the essential Linux, networking, command line, and bash scripting topics that a DevOps engineer needs to master. The content is structured to be both a learning resource and a practical reference guide that you can use in real-world scenarios.

The tutorial includes practical examples, best practices, and automation scripts that you can adapt for your specific DevOps needs. Each section builds upon the previous ones, providing a complete foundation for Linux system administration and DevOps practices.
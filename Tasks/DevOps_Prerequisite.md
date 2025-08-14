# DevOps Prerequisites - Complete Course

## Course Overview

This comprehensive course provides all the foundational knowledge and skills required before starting your DevOps journey. Whether you're a developer, system administrator, or someone new to IT, this course will prepare you with the essential prerequisites.

**Duration**: 8-12 weeks (self-paced)  
**Level**: Beginner to Intermediate  
**Prerequisites**: Basic computer literacy

---

## Module 1: Operating Systems Fundamentals (Week 1-2)

### Learning Objectives
- Understand operating system concepts and architecture
- Master Linux command line operations
- Learn file systems, processes, and networking basics
- Practice system administration tasks

### Topics Covered

#### 1.1 Operating System Concepts
- **What is an Operating System?**
  - Kernel, user space, and system calls
  - Process management and memory management
  - File systems and I/O operations
- **Linux vs Windows vs macOS**
  - Understanding different OS families
  - When to use each operating system

#### 1.2 Linux Command Line Mastery
- **Essential Commands**
  ```bash
  # File operations
  ls, cd, pwd, mkdir, rmdir, cp, mv, rm, find, locate
  
  # Text processing
  cat, less, more, head, tail, grep, sed, awk, cut, sort
  
  # Process management
  ps, top, htop, kill, killall, jobs, nohup
  
  # System information
  df, du, free, uname, uptime, who, whoami
  ```

- **File Permissions and Ownership**
  ```bash
  # Understanding permissions
  chmod 755 file.txt
  chmod u+x script.sh
  chown user:group file.txt
  
  # Special permissions
  sudo, su, umask
  ```

- **Text Editors**
  - Vi/Vim basics
  - Nano for beginners
  - Basic text manipulation

#### 1.3 System Administration Basics
- **User Management**
  ```bash
  useradd, usermod, userdel
  groupadd, groupmod, groupdel
  passwd, su, sudo configuration
  ```

- **Package Management**
  ```bash
  # Ubuntu/Debian
  apt update, apt install, apt remove
  
  # CentOS/RHEL
  yum install, yum update, yum remove
  
  # Modern alternatives
  snap, flatpak
  ```

- **Service Management**
  ```bash
  # Systemd
  systemctl start/stop/restart service
  systemctl enable/disable service
  systemctl status service
  journalctl -u service
  ```

### Hands-on Lab 1: Linux System Setup
1. Install Ubuntu Server in a VM
2. Create users and groups
3. Set up SSH access
4. Install and configure basic services (Apache, MySQL)
5. Practice file manipulation and process management

---

## Module 2: Networking Fundamentals (Week 2-3)

### Learning Objectives
- Understand network protocols and models
- Master TCP/IP, DNS, and HTTP concepts
- Learn network troubleshooting
- Understand security basics

### Topics Covered

#### 2.1 Network Fundamentals
- **OSI Model and TCP/IP Stack**
  - 7 layers of OSI model
  - TCP/IP 4-layer model
  - How data flows through networks

- **IP Addressing and Subnetting**
  - IPv4 and IPv6 basics
  - CIDR notation
  - Public vs private IP addresses
  - Subnetting calculations

#### 2.2 Essential Protocols
- **HTTP/HTTPS**
  - Request/response cycle
  - Status codes (200, 404, 500, etc.)
  - Headers and methods (GET, POST, PUT, DELETE)
  - SSL/TLS basics

- **DNS (Domain Name System)**
  - DNS resolution process
  - Record types (A, AAAA, CNAME, MX, TXT)
  - DNS troubleshooting

- **SSH (Secure Shell)**
  - SSH key generation and management
  - SSH config files
  - Port forwarding and tunneling

#### 2.3 Network Tools and Troubleshooting
```bash
# Network diagnostics
ping, traceroute, nslookup, dig
netstat, ss, lsof
curl, wget

# Network configuration
ip addr, ip route
iptables basics
firewall configuration
```

### Hands-on Lab 2: Network Configuration
1. Configure static IP addresses
2. Set up DNS resolution
3. Configure SSH key-based authentication
4. Set up basic firewall rules
5. Test network connectivity and troubleshoot issues

---

## Module 3: Version Control with Git (Week 3-4)

### Learning Objectives
- Master Git version control system
- Understand branching and merging strategies
- Learn collaborative development workflows
- Practice with GitHub/GitLab

### Topics Covered

#### 3.1 Git Fundamentals
- **Git Concepts**
  - Repository, working directory, staging area
  - Commits, branches, and tags
  - Local vs remote repositories

- **Basic Git Commands**
  ```bash
  # Repository setup
  git init, git clone
  
  # Basic workflow
  git add, git commit, git push, git pull
  git status, git log, git diff
  
  # Branch management
  git branch, git checkout, git merge
  git switch, git restore (modern alternatives)
  ```

#### 3.2 Advanced Git Operations
- **Branching Strategies**
  - Feature branches
  - Git Flow
  - GitHub Flow
  - Release branches

- **Merge vs Rebase**
  ```bash
  # Merging
  git merge feature-branch
  
  # Rebasing
  git rebase main
  git rebase -i HEAD~3  # Interactive rebase
  ```

- **Conflict Resolution**
  - Understanding merge conflicts
  - Resolving conflicts manually
  - Using merge tools

#### 3.3 Collaborative Workflows
- **Remote Repositories**
  ```bash
  git remote add origin <url>
  git fetch, git pull --rebase
  git push -u origin branch-name
  ```

- **Pull Requests/Merge Requests**
  - Creating effective PRs
  - Code review process
  - PR templates and guidelines

### Hands-on Lab 3: Git Workflow Project
1. Create a repository with multiple branches
2. Implement a feature branch workflow
3. Practice merge conflict resolution
4. Set up GitHub Actions for basic CI
5. Collaborate with others on a shared project

---

## Module 4: Programming and Scripting (Week 4-5)

### Learning Objectives
- Learn shell scripting for automation
- Understand basic programming concepts
- Practice with Python for DevOps tasks
- Work with APIs and JSON

### Topics Covered

#### 4.1 Shell Scripting
- **Bash Scripting Basics**
  ```bash
  #!/bin/bash
  
  # Variables
  NAME="DevOps"
  echo "Hello $NAME"
  
  # Conditional statements
  if [ -f "/etc/passwd" ]; then
      echo "File exists"
  fi
  
  # Loops
  for file in *.txt; do
      echo "Processing $file"
  done
  
  # Functions
  function backup_files() {
      cp "$1" "$1.backup"
  }
  ```

- **Advanced Scripting**
  - Command line arguments ($1, $2, $@)
  - Exit codes and error handling
  - Regular expressions
  - Process substitution and pipes

#### 4.2 Python for DevOps
- **Python Basics**
  ```python
  # Basic syntax
  import os, sys, json, requests
  
  # File operations
  with open('config.txt', 'r') as f:
      content = f.read()
  
  # Working with APIs
  response = requests.get('https://api.github.com/user')
  data = response.json()
  
  # Error handling
  try:
      result = risky_operation()
  except Exception as e:
      print(f"Error: {e}")
  ```

- **Useful Libraries for DevOps**
  - `os` and `sys` for system operations
  - `subprocess` for running commands
  - `json` and `yaml` for configuration
  - `requests` for HTTP operations
  - `paramiko` for SSH operations

#### 4.3 Working with APIs and Data Formats
- **REST APIs**
  - HTTP methods and status codes
  - Authentication (API keys, OAuth)
  - Rate limiting and error handling

- **Data Formats**
  - JSON parsing and manipulation
  - YAML for configuration files
  - XML basics
  - CSV processing

### Hands-on Lab 4: Automation Scripts
1. Write a system monitoring script in bash
2. Create a Python script to interact with GitHub API
3. Build a log parser and analyzer
4. Automate deployment preparation tasks
5. Create a backup and restore script

---

## Module 5: Containers and Virtualization (Week 5-6)

### Learning Objectives
- Understand virtualization concepts
- Master Docker containerization
- Learn container orchestration basics
- Practice with container registries

### Topics Covered

#### 5.1 Virtualization Concepts
- **Virtual Machines vs Containers**
  - Hypervisors (Type 1 vs Type 2)
  - Resource allocation and isolation
  - Performance considerations
  - When to use VMs vs containers

#### 5.2 Docker Mastery
- **Docker Fundamentals**
  ```dockerfile
  # Dockerfile example
  FROM ubuntu:20.04
  
  RUN apt-get update && apt-get install -y \
      python3 \
      python3-pip
  
  COPY app.py /app/
  WORKDIR /app
  
  EXPOSE 8080
  CMD ["python3", "app.py"]
  ```

- **Docker Commands**
  ```bash
  # Image management
  docker build -t myapp:v1.0 .
  docker images, docker rmi
  
  # Container operations
  docker run -d -p 8080:8080 myapp:v1.0
  docker ps, docker stop, docker rm
  docker exec -it container bash
  
  # Data management
  docker volume create myvolume
  docker run -v myvolume:/data myapp
  ```

#### 5.3 Container Orchestration Basics
- **Docker Compose**
  ```yaml
  version: '3.8'
  services:
    web:
      build: .
      ports:
        - "8080:8080"
      depends_on:
        - db
    db:
      image: mysql:8.0
      environment:
        MYSQL_ROOT_PASSWORD: secret
      volumes:
        - db_data:/var/lib/mysql
  volumes:
    db_data:
  ```

- **Introduction to Kubernetes**
  - Pods, Services, and Deployments
  - kubectl basics
  - YAML manifests

### Hands-on Lab 5: Container Applications
1. Containerize a multi-tier application
2. Create Docker Compose setup for development
3. Build and push images to Docker Hub
4. Deploy containers with environment variables
5. Set up container health checks and logging

---

## Module 6: Infrastructure as Code Basics (Week 6-7)

### Learning Objectives
- Understand IaC principles
- Learn configuration management basics
- Practice with Ansible
- Understand infrastructure provisioning

### Topics Covered

#### 6.1 Infrastructure as Code Concepts
- **IaC Principles**
  - Declarative vs imperative approaches
  - Idempotency and state management
  - Version control for infrastructure
  - Infrastructure testing

#### 6.2 Configuration Management with Ansible
- **Ansible Basics**
  ```yaml
  # Ansible playbook example
  ---
  - hosts: web_servers
    become: yes
    tasks:
      - name: Install nginx
        apt:
          name: nginx
          state: present
      
      - name: Start nginx service
        systemd:
          name: nginx
          state: started
          enabled: yes
  ```

- **Ansible Components**
  - Inventory files
  - Playbooks and roles
  - Variables and templates
  - Modules and collections

#### 6.3 Infrastructure Provisioning
- **Introduction to Terraform**
  ```hcl
  # Terraform example
  resource "aws_instance" "web" {
    ami           = "ami-0c55b159cbfafe1d0"
    instance_type = "t2.micro"
    
    tags = {
      Name = "WebServer"
    }
  }
  ```

- **Cloud Platforms Overview**
  - AWS, Azure, Google Cloud basics
  - Infrastructure services
  - Cost management principles

### Hands-on Lab 6: Infrastructure Automation
1. Create Ansible playbooks for server configuration
2. Set up multi-environment inventory
3. Use Ansible roles for reusable configurations
4. Practice with Terraform for basic provisioning
5. Implement infrastructure testing

---

## Module 7: Monitoring and Logging (Week 7-8)

### Learning Objectives
- Understand monitoring principles
- Learn log management strategies
- Practice with monitoring tools
- Implement alerting systems

### Topics Covered

#### 7.1 Monitoring Fundamentals
- **Types of Monitoring**
  - Infrastructure monitoring
  - Application performance monitoring
  - Log monitoring
  - User experience monitoring

- **Key Metrics**
  - CPU, memory, disk, network
  - Application metrics
  - Business metrics
  - SLA/SLO concepts

#### 7.2 Log Management
- **Log Aggregation**
  ```bash
  # Log rotation and management
  logrotate configuration
  rsyslog setup
  journalctl usage
  ```

- **Log Analysis**
  - Structured vs unstructured logs
  - Log parsing and filtering
  - Log correlation and alerting

#### 7.3 Monitoring Tools
- **Prometheus Basics**
  ```yaml
  # prometheus.yml
  global:
    scrape_interval: 15s
  
  scrape_configs:
    - job_name: 'web-app'
      static_configs:
        - targets: ['localhost:8080']
  ```

- **Grafana Dashboards**
  - Creating visualizations
  - Dashboard design principles
  - Alerting rules

### Hands-on Lab 7: Monitoring Setup
1. Set up Prometheus and Grafana stack
2. Create custom monitoring dashboards
3. Implement log aggregation with rsyslog
4. Configure alerting rules and notifications
5. Monitor containerized applications

---

## Module 8: Security Fundamentals (Week 8)

### Learning Objectives
- Understand security principles
- Learn about common vulnerabilities
- Practice secure configuration
- Implement security monitoring

### Topics Covered

#### 8.1 Security Principles
- **CIA Triad**
  - Confidentiality, Integrity, Availability
  - Authentication vs authorization
  - Principle of least privilege

- **Common Vulnerabilities**
  - OWASP Top 10
  - Security misconfigurations
  - Vulnerable dependencies

#### 8.2 Secure Infrastructure
- **Network Security**
  ```bash
  # Firewall configuration
  iptables -A INPUT -p tcp --dport 22 -j ACCEPT
  iptables -A INPUT -p tcp --dport 80 -j ACCEPT
  iptables -P INPUT DROP
  ```

- **System Hardening**
  - User account security
  - Service minimization
  - File permissions and encryption
  - Security updates and patches

#### 8.3 Secrets Management
- **Best Practices**
  - Never commit secrets to version control
  - Use environment variables carefully
  - Implement secret rotation
  - Use dedicated secret management tools

### Hands-on Lab 8: Security Implementation
1. Harden a Linux server
2. Configure SSL/TLS certificates
3. Set up secure SSH access
4. Implement basic intrusion detection
5. Audit system security configuration

---

## Final Project: Complete DevOps Pipeline Setup

### Project Overview
Build a complete development and deployment pipeline that demonstrates all learned concepts.

### Requirements
1. **Application**: Create a simple web application
2. **Version Control**: Implement Git workflow with feature branches
3. **Infrastructure**: Use IaC to provision servers
4. **Configuration**: Automate server configuration with Ansible
5. **Containerization**: Dockerize the application
6. **Monitoring**: Set up comprehensive monitoring
7. **Security**: Implement security best practices
8. **Documentation**: Create complete documentation

### Deliverables
- Fully functional web application
- Complete IaC templates
- Configuration management playbooks
- Docker containers and compose files
- Monitoring dashboard
- Security compliance report
- Comprehensive documentation

---

## Assessment and Certification

### Knowledge Checks
- Weekly quizzes for each module
- Hands-on lab completion
- Peer code reviews
- Final project presentation

### Practical Skills Assessment
- Command line proficiency test
- Git workflow demonstration
- Infrastructure automation challenge
- Troubleshooting scenarios
- Security audit simulation

---

## Resource Library

### Books
- "The Linux Command Line" by William Shotts
- "Pro Git" by Scott Chacon and Ben Straub
- "Ansible for DevOps" by Jeff Geerling
- "Docker Deep Dive" by Nigel Poulton

### Online Resources
- Linux documentation and man pages
- Docker official documentation
- Ansible documentation
- Git official tutorial
- OWASP security guides

### Practice Environments
- VirtualBox/VMware for local VMs
- Docker Desktop for containerization
- GitHub/GitLab for version control
- Cloud provider free tiers
- Vagrant for reproducible environments

### Tools to Install
```bash
# Essential tools
git, vim, curl, wget, ssh
docker, docker-compose
ansible, terraform
prometheus, grafana
python3, pip
node.js, npm (optional)
```

---

## Next Steps After Completion

### DevOps Specialization Paths
1. **Cloud Platforms**: AWS, Azure, or Google Cloud certification
2. **Container Orchestration**: Kubernetes administration
3. **CI/CD**: Jenkins, GitHub Actions, or GitLab CI
4. **Site Reliability Engineering**: Advanced monitoring and automation
5. **Security**: DevSecOps and security automation

### Recommended Follow-up Courses
- Kubernetes for Container Orchestration
- Advanced CI/CD Pipeline Development
- Cloud Platform Specialization
- Site Reliability Engineering
- DevSecOps and Security Automation

---

## Course Support

### Community Resources
- Course discussion forums
- Study groups and peer support
- Office hours with instructors
- Industry mentorship program

### Additional Help
- 1-on-1 tutoring sessions
- Career guidance and resume review
- Job placement assistance
- Alumni network access

---

*This course is designed to be practical and hands-on. Each module builds upon the previous one, ensuring you have a solid foundation before advancing to more complex DevOps practices.*

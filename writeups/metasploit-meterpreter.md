# Metasploit: Meterpreter

## Room Information

| Platform | Difficulty | Topic |
|----------|------------|-------|
| TryHackMe | Beginner | Post-Exploitation |

**Room Link:** https://tryhackme.com/room/metasploitmeterpreter

---

## Overview

This room focuses on **Meterpreter**, a powerful payload used within the Metasploit Framework. Meterpreter is designed to support penetration testing during the **post-exploitation phase**, allowing attackers to interact with a compromised system in a flexible and stealthy way.

Unlike traditional payloads, Meterpreter runs entirely in **memory**, avoiding writing files to disk. This helps reduce detection by antivirus solutions that primarily monitor files written to storage.

Throughout this lab, I explored how Meterpreter operates, how different Meterpreter payloads work, and how its built-in commands and extensions can be used to gather information, extract credentials, and locate sensitive files on a compromised system.

---

## Skills Demonstrated

- Post-exploitation enumeration
- Credential harvesting
- File system discovery
- Meterpreter command usage
- Privilege and system enumeration
- Identifying sensitive data stored on hosts

---

## Tools Used

- Metasploit Framework
- Meterpreter
- Kiwi (Mimikatz extension)
- SMB exploitation via psexec

---

## Key Concepts

### Meterpreter

Meterpreter is an advanced Metasploit payload that acts as an **in-memory agent** on the compromised host.

Key characteristics:

- Runs entirely in memory
- Avoids writing files to disk
- Uses encrypted communication channels
- Allows advanced system interaction

Once an exploit successfully runs, Meterpreter connects back to the attacker and provides an interactive command environment for system interaction.

### Process Injection

Meterpreter often hides itself inside legitimate processes to remain stealthy.

Example commands used to identify the process ID:

```meterpreter > getpid```

This command returns the **process identifier (PID)** under which Meterpreter is running.

Listing system processes:

```meterpreter > ps```

This helps identify which processes Meterpreter may migrate into.

---

## Meterpreter Payload Types

Meterpreter payloads fall into two categories:

### Single (Inline) Payloads

These payloads contain the full payload in one execution.

Example:

```generic/shell_reverse_tcp```

### Staged Payloads

Staged payloads are delivered in two steps:

1. Stager establishes a connection
2. Stage downloads the full payload

Example:

```windows/x64/meterpreter/reverse_tcp```

---

## Enumerating Meterpreter Payloads

Payloads can be listed using **msfvenom**.

```msfvenom --list payloads | grep meterpreter```

This command filters the list to show only Meterpreter payloads.

Meterpreter payloads exist for many platforms, including:

- Windows
- Linux
- macOS
- Android
- iOS
- Java
- Python
- PHP

---

## Meterpreter Command Categories

Running the `help` command inside a Meterpreter session displays available commands.

```meterpreter > help```

Commands are grouped into categories, including:

- Core commands
- File system commands
- Networking commands
- System commands
- User interface commands
- Credential commands

Each Meterpreter version may include different command sets.

---

## Core Meterpreter Commands

Some commonly used core commands include:

| Command | Purpose |
|---------|---------|
| `help` | Display available commands |
| `background` | Send session to background |
| `exit` | Terminate session |
| `migrate` | Move Meterpreter into another process |
| `sessions` | Switch between sessions |

---

## File System Commands

| Command | Purpose |
|---------|---------|
| `cd` | Change directory |
| `ls` | List directory contents |
| `pwd` | Print working directory |
| `cat` | Display file contents |
| `download` | Download files |
| `upload` | Upload files |

---

## Networking Commands

| Command | Purpose |
|---------|---------|
| `arp` | Display ARP cache |
| `ifconfig` | Show network interfaces |
| `netstat` | Display network connections |
| `portfwd` | Forward local ports |
| `route` | View routing table |

---

## System Commands

| Command | Purpose |
|---------|---------|
| `sysinfo` | Display system information |
| `getuid` | Show current user |
| `getpid` | Show process ID |
| `ps` | List running processes |
| `shell` | Launch system shell |

---

## Post-Exploitation with Meterpreter

Meterpreter provides powerful capabilities during post-exploitation, including:

- gathering system information
- extracting credentials
- searching for sensitive files
- privilege escalation
- lateral movement preparation

Additional functionality can be loaded dynamically using extensions.

---

## Loading Extensions

Meterpreter supports loading extensions to expand functionality.

Example: loading Python support.

```meterpreter > load python```

Running Python code from the session:

```meterpreter > python_execute "print 'TryHackMe Rocks!'"```

### Kiwi Extension

The **Kiwi extension** provides functionality similar to **Mimikatz**for credential extraction.

```meterpreter > load kiwi```

Once loaded, additional commands become available.

Examples include:

- `creds_all`
- `creds_msv`
- `creds_wdigest`
- `dcsync`
- `lsa_dump_secrets`
- `golden_ticket_create`

---

## Post-Exploitation Challenge

The final challenge simulated a compromised system accessed through SMB using the **psexec module**.

Credentials provided:

- **Username:** `ballen`
- **Password:** `Password1`

Using Meterpreter, I enumerated the system and discovered several key pieces of information.

---

## Findings

| Item | Result |
|------|--------|
| Computer Name | `ACME-TEST` |
| Domain | `FLASH` |
| User Share | `speedster` |
| jchambers NTLM Hash | `69596c7aa1e8daee17f8e78870e25a5c` |
| jchambers Password | `Trustno1` |

---

## Sensitive Files Discovered

### secrets.txt

**Location:**  
`c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt`

**Discovered credential:**  
**Twitter Password:** `KDSvbsw3849!`

### realsecret.txt

**Location:**  
`c:\inetpub\wwwroot\realsecret.txt`

**Contents:**  
`The Flash is the fastest man alive`

---

## Key Takeaways

This room demonstrated how powerful Meterpreter can be once initial access has been established.

Key lessons from this lab include:

- Meterpreter operates entirely in memory, improving stealth
- Post-exploitation is critical for extracting valuable information
- Credential harvesting can quickly expand access
- Sensitive information is often stored in unexpected locations
- Extensions like Kiwi dramatically expand Meterpreter’s capabilities

---

## Personal Reflection

This room reinforced how important the post-exploitation phase is during an engagement. Gaining access to a system is only the first step. Meterpreter provides a powerful environment for interacting with compromised machines, extracting credentials, and locating valuable data.

Working through this lab demonstrated how quickly sensitive information can be uncovered once an attacker has established a foothold. It also highlights the importance of proper credential protection, monitoring privileged processes, and restricting access to sensitive files in real-world environments.

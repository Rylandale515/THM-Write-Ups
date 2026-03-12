# Blue

## Room Information

| Platform | Difficulty | Category |
|----------|------------|----------|
| TryHackMe | Beginner | Windows Exploitation |

Room: https://tryhackme.com/room/blue

---

## Overview

The **Blue** room focuses on exploiting a vulnerable Windows machine using the well-known **MS17-010 (EternalBlue)** vulnerability. EternalBlue targets the Windows SMB protocol and enables remote code execution when successfully exploited.

During this lab, reconnaissance was performed to identify open services on the system. Enumeration revealed that the target machine was vulnerable to **MS17-010**. Using the **Metasploit Framework**, the vulnerability was exploited to obtain a remote shell.

Once access was achieved, the shell was upgraded to a **Meterpreter session**, privileges were escalated to **NT AUTHORITY\SYSTEM**, password hashes were extracted from the system, and credentials were cracked. Finally, several flags were located across key areas of the Windows file system.

This lab demonstrates a realistic attack chain including:

- Reconnaissance
- Exploitation
- Shell upgrading
- Privilege escalation
- Credential extraction
- Password cracking
- Post-exploitation enumeration

---

## Skills Demonstrated

- Network reconnaissance with Nmap  
- Identifying SMB vulnerabilities  
- Exploiting EternalBlue (MS17-010)  
- Using Metasploit exploitation modules  
- Upgrading shells to Meterpreter  
- Privilege escalation techniques  
- Dumping and cracking password hashes  
- Post-exploitation enumeration  

---

## Tools Used

- Nmap  
- Metasploit Framework  
- Meterpreter  
- Hashdump  
- Password cracking tools  

---

# Walkthrough

---

# Reconnaissance

The first step was to scan the target system to identify open ports and running services.

```
Nmap Scan
```

The scan revealed **three open ports under port 1000**.

Answer:
3

Further enumeration revealed that the machine was vulnerable to the SMB vulnerability:

MS17-010

This vulnerability allows attackers to execute code remotely through the SMB protocol.

---

# Exploitation

Metasploit was used to exploit the discovered vulnerability.

```Start Metasploit```

The exploit module used was:

exploit/windows/smb/ms17_010_eternalblue

After selecting the module, the options were reviewed.

```show options```

The required parameter that needed to be configured was:

RHOSTS

This value was set to the IP address of the target machine.

For learning purposes, the payload was manually specified.

```set payload windows/x64/shell/reverse_tcp```

Once configured, the exploit was executed.

```exploit```

After successful exploitation, a remote shell was obtained.

The shell session was then backgrounded to continue interacting with Metasploit.

---

# Shell Upgrade

To improve control over the compromised machine, the shell was upgraded to a Meterpreter session.

The following post-exploitation module was used:

post/multi/manage/shell_to_meterpreter

This module converts a basic shell session into a **Meterpreter session**.

The required parameter that needed to be configured was:

SESSION

After setting the correct session ID, the module was executed and the shell was successfully upgraded.

---

# Privilege Escalation

Once a Meterpreter session was established, privileges were verified.

```getsystem```

This confirmed the session had escalated to:

NT AUTHORITY\SYSTEM

A system shell was opened to verify privileges.

```shell```

Running the following command confirmed SYSTEM access.

```whoami```

Result:

NT AUTHORITY\SYSTEM

---

# Process Migration

Even though SYSTEM privileges were obtained, Meterpreter may still be running inside an unstable process.

The list of running processes was reviewed.

```ps```

A stable SYSTEM process was identified and the Meterpreter session was migrated to that process.

```migrate PROCESS_ID```

Process migration helps maintain session stability and reduces the risk of losing access.

---

# Credential Dumping

With SYSTEM privileges obtained, password hashes were extracted.

```hashdump```

This command dumps password hashes stored in the Windows **Security Account Manager (SAM)** database.

The non-default user identified was:

Jon

---

# Password Cracking

The extracted NTLM hash was copied to a file and cracked with a password-cracking tool.

Cracked password:

alqfna22

---

# Flags

## Flag 1

Location: System Root

flag{access_the_machine}

---

## Flag 2

Location: SAM database

flag{sam_database_elevated_access}

---

## Flag 3

Location: Administrator documents

flag{admin_documents_can_be_valuable}

---

# Key Takeaways

This room demonstrates the dangers of unpatched SMB vulnerabilities.

Important lessons include:

- SMB vulnerabilities like EternalBlue can lead to full system compromise
- Metasploit simplifies exploitation workflows significantly
- Meterpreter provides powerful post-exploitation capabilities
- SYSTEM access allows the extraction of credential hashes from the SAM database
- Weak passwords remain a critical security risk

---

# Personal Reflection

This lab provided a clear demonstration of exploiting the **MS17-010 EternalBlue vulnerability** and performing post-exploitation activities on a Windows machine.

The most valuable takeaway from this exercise was understanding the full attack chain beyond initial exploitation. Converting shells to Meterpreter sessions, escalating privileges, extracting password hashes, and cracking credentials are all important skills used during real penetration testing engagements.

From a defensive perspective, this lab highlights the importance of system patching, strong password policies, and proper monitoring of SMB services within enterprise environments.

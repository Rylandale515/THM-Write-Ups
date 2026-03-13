# TryHackMe: Hydra

## Room Information

| Category | Offensive Security Tooling |
|----------|----------------------------|
| Platform | TryHackMe |
| Difficulty | Beginner |
| Tool | Hydra |
| Focus | Credential Brute Forcing |

---

# Overview

Hydra is a fast, flexible network login cracker that brute-forces many different authentication services. It is commonly used during penetration tests to identify weak passwords across network services and web applications.

In this room, I learned how Hydra works, how to configure brute-force attacks against different services, and how to successfully recover credentials from both a web login form and an SSH service.

Hydra supports a wide variety of protocols, including:

Asterisk, FTP, HTTP, HTTPS, IMAP, LDAP, MSSQL, MySQL, RDP, SMB, SMTP, SNMP, SSH, Telnet, VNC, and many more.

This makes Hydra an extremely valuable tool when assessing authentication security across networked services.

---

# Skills Demonstrated

• Password brute forcing  
• Credential auditing  
• Hydra command usage  
• Web form authentication attacks  
• SSH authentication attacks  
• Wordlist-based password attacks  

---

# Key Concepts

## What is Hydra?

Hydra is an automated password brute-force tool used to test login credentials across numerous services.

Rather than manually attempting passwords, Hydra automates the process by:

• Iterating through a password list  
• Attempting authentication repeatedly  
• Running multiple threads simultaneously  

This allows attackers or security testers to rapidly identify weak credentials.

Because of this capability, organizations must ensure they enforce:

• Strong password policies  
• Account lockout mechanisms  
• Rate limiting  
• Multi-factor authentication

---

# Hydra Supported Protocols

Hydra supports brute forcing across a wide variety of authentication services, including:

FTP  
HTTP/HTTPS login forms  
IMAP  
LDAP  
MSSQL  
MySQL  
RDP  
SMB  
SMTP  
SNMP  
SSH  
Telnet  
VNC  

This flexibility allows Hydra to be used during both **network penetration tests and web application assessments**.

---

# Hydra Command Basics

Hydra commands vary depending on the targeted protocol.

General syntax includes:

username specification  
password list specification  
target host  
protocol

Example parameters:

-l : specify username  
-P : specify password list  
-t : number of parallel threads  

---

# Brute Forcing an FTP Service (Example)

Example command format:

hydra -l user -P passlist.txt ftp://MACHINE_IP

Explanation:

-l user specifies the username  
-P passlist.txt supplies the password list  
ftp://MACHINE_IP defines the target service  

Hydra will attempt every password from the list against the provided username.

---

# Brute Forcing SSH Credentials

Hydra can also brute force SSH authentication.

Example syntax:

hydra -l username -P passwords.txt MACHINE_IP -t 4 ssh

Explanation:

-l specifies the username  
-P supplies the password list  
-t sets the number of concurrent threads  
ssh specifies the target service  

Example configuration used during the lab:

hydra -l root -P passwords.txt MACHINE_IP -t 4 ssh

Hydra will attempt all passwords in the list against the SSH service using four parallel threads.

---

# Brute Forcing Web Login Forms

Hydra also supports attacking web authentication forms.

To do this, we must identify:

• Request method (GET or POST)  
• Login endpoint path  
• Parameter names  
• Response indicating failure  

General command syntax:

sudo hydra -l username -P wordlist MACHINE_IP http-post-form "<path>:<parameters>:<failure_string>" -V

Example format:

hydra -l username -P wordlist MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

Explanation:

/ is the login page path  
Username and password are the form fields  
^USER^ and ^PASS^ are replaced dynamically by Hydra  
F=incorrect identifies failed login responses  
-V enables verbose output  

---

# Lab Walkthrough

## Environment Setup

To complete this exercise, both the attacker machine and target machine were deployed through TryHackMe.

The attacker's machine contained Hydra pre-installed.

The target web application was accessible at:

http://MACHINE_IP

---

# Task 1 — Hydra Introduction

This section introduced the Hydra tool and its purpose in credential brute forcing.

Hydra is designed to automate password-guessing attacks against authentication services, significantly reducing the time required to identify weak passwords.

---

# Task 2 — Using Hydra

In this task, Hydra was used to recover credentials from two services:

• A web login form  
• An SSH service  

---

# Attack 1 — Web Form Brute Force

The first objective was to brute force Molly's web application password.

Hydra was configured to attack the login form using a password wordlist.

After running the attack successfully, the recovered flag was:

THM{2673a7dd116de68e85c48ec0b1f2612e}

---

# Attack 2 — SSH Credential Brute Force

Next, Hydra was used to brute force Molly's SSH credentials.

The attack targeted the SSH service running on the machine.

After running Hydra with the appropriate username and password lists, the correct password was identified, and the flag was recovered.

Flag obtained:

THM{c8eeb0468febbadea859baeb33b2541b}

---

# Security Lessons

This room highlights the dangers of weak authentication controls.

Without proper protections, automated tools like Hydra can quickly compromise accounts.

Common defensive controls include:

• Strong password policies  
• Account lockout mechanisms  
• Login rate limiting  
• CAPTCHA protection  
• Multi-factor authentication

---

# Defensive Takeaways

Organizations should implement the following protections:

• Enforce complex passwords  
• Disable default credentials  
• Limit login attempts  
• Implement monitoring and alerting  
• Use MFA for remote access services

These protections significantly reduce the risk of credential brute-force attacks.

---

# Conclusion

This room introduced Hydra and demonstrated how automated password cracking tools can be used to identify weak authentication mechanisms across multiple services.

Understanding how tools like Hydra operate is essential for both offensive and defensive security professionals. Attackers rely on these tools to compromise accounts, while defenders must design systems capable of detecting and preventing such attacks.

Mastering tools like Hydra is an important step in developing practical penetration testing and security assessment skills.

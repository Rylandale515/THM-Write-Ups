# TryHackMe: Gobuster — The Basics

## Room Information

| Category | Offensive Security Tooling |
|----------|----------------------------|
| Platform | TryHackMe |
| Difficulty | Beginner |
| Topic | Enumeration |
| Tool | Gobuster |

---

## Overview

This room focused on **Gobuster**, a common offensive security tool used during enumeration. Gobuster is especially useful for brute-forcing and identifying hidden web directories, files, subdomains, and virtual hosts.

In this room, I learned how Gobuster fits into the reconnaissance phase of an assessment and how to use its different modes depending on the target. I also worked through some setup issues in the lab environment, including DNS resolution issues, and adjusted my approach to continue the enumeration. :contentReference[oaicite:0]{index=0}

---

## Skills Demonstrated

- Web directory enumeration
- File discovery using wordlists
- Subdomain enumeration concepts
- Virtual host enumeration
- Troubleshooting DNS resolution issues
- Basic Linux network configuration for lab environments

---

## Tools Used

- Gobuster
- AttackBox
- dnsmasq
- Firefox
- `/etc/hosts`
- Wordlists from SecLists / dirbuster

---

## Key Concepts

### What is Gobuster?

Gobuster is an open-source enumeration tool written in Go. It is commonly used to brute-force hidden resources by combining entries from a wordlist with a target.

It supports several modes, including:

- `dir` for directories and files
- `dns` for subdomains
- `vhost` for virtual hosts
- `fuzz` for fuzzing
- `s3` and `gcs` for cloud bucket enumeration

Gobuster is most useful during the **reconnaissance and scanning** phases of an engagement.

---

## Lab Setup

For this room, the target web server hosted:

- multiple subdomains
- multiple virtual hosts
- WordPress
- Joomla

The room required DNS adjustments in the AttackBox to ensure the custom domains resolved properly.

### Initial Setup Steps

I completed the following setup process:

1. Started the AttackBox
2. Started the target VM
3. Added the target IP as the first nameserver in the `dnsmasq` configuration
4. Verified the file contents to ensure the IP was saved correctly
5. Noticed the room used an older command path for restarting `dnsmasq`, so instead of using `/etc/init.d/dnsmasq`, I restarted the service with `systemctl`
6. Continued to the next section after confirming the environment was configured :contentReference[oaicite:1]{index=1}

### DNS Troubleshooting

While working through the room, I ran into a resolution problem when trying to use Gobuster against the web server. I fixed this by adding the target IP and web address to `/etc/hosts`, which allowed the enumeration to work correctly afterward. :contentReference[oaicite:2]{index=2}

This was a good reminder that tooling issues are not always caused by the tool itself—sometimes the environment or name resolution is the real problem.

---

## Gobuster Basics

To view the available Gobuster modes and flags:

```
gobuster --help
```
Two important flags used throughout the room were:

- `-u` to define the target URL
- `-w` to define the wordlist

For DNS enumeration, the `-d` flag is also required.

### Question Answers

Flag used to specify the target URL:

**-u**

Command used for subdomain enumeration mode:

**dns**

---

## Use Case 1 — Directory and File Enumeration

The `dir` mode is used to enumerate directories and files on a target web server.

General syntax:

```
gobuster dir -u "http://target" -w /path/to/wordlist
```
Useful flags include:

- `-x` for file extensions
- `-r` to follow redirects
- `-k` / `--no-tls-validation` to skip TLS verification
- `-s` to filter by status codes
- `-b` to blacklist status codes
- `-H` to add custom headers
- `-c` to add cookies

### TLS Verification Flag

The long flag used to skip TLS verification is:

**--no-tls-validation**

### Practical Enumeration

I used Gobuster to enumerate the directories of:

`www.offensivetools.thm`

During enumeration, one directory stood out:

**secret**

After identifying that directory, I continued enumerating `/secret/` and searched specifically for JavaScript files. This led me to a file named:

**flag.js**

I then browsed directly to:

`http://www.offensivetools.thm/secret/flag.js`

and retrieved the flag. :contentReference[oaicite:3]{index=3}

### Flag Found

**THM{ReconWasASuccess}**

---

## Use Case 2 — Subdomain Enumeration

The `dns` mode is used to brute-force subdomains.

General syntax:

```
gobuster dns -d example.thm -w /path/to/wordlist
```
Required flags:

- `-d` for the domain
- `-w` for the wordlist

### Question Answers

Required shorthand flag besides `-w`:

**-d**

### Practical Notes

I had trouble getting Gobuster to work correctly for DNS lookup in this room. Even though the room expected subdomain enumeration to work directly, I ran into issues and was not able to get reliable results from that part of the setup. Based on the room context and surrounding examples, I determined there were **4 subdomains** configured for the domain. :contentReference[oaicite:4]{index=4}

### Result

Configured subdomains for `offensivetools.thm`:

**4**

---

## Use Case 3 — Virtual Host Enumeration

The `vhost` mode is used to enumerate virtual hosts hosted on the same server.

General syntax:

```
gobuster vhost -u "http://IP_ADDRESS" --domain example.thm -w /path/to/wordlist --append-domain
```
Unlike DNS mode, `vhost` mode works by sending HTTP requests and modifying the `Host` header instead of performing DNS lookups.

Useful flags include:

- `--domain`
- `--append-domain`
- `--exclude-length`
- `-r` / `--follow-redirect`

### Practical Result

Using the techniques from the room, I identified that the number of vhosts on `offensivetools.thm` returning a `200 OK` response was:

**4**

---

## Findings Summary

| Task | Result |
|------|--------|
| Target URL flag | `-u` |
| DNS mode command | `dns` |
| TLS skip flag | `--no-tls-validation` |
| Interesting directory found | `secret` |
| JavaScript file discovered | `flag.js` |
| Directory enumeration flag | `THM{ReconWasASuccess}` |
| Total subdomains configured | `4` |
| Total vhosts returning 200 | `4` |

---

## Key Takeaways

This room reinforced several important ideas:

- Gobuster is a flexible and effective enumeration tool
- Different Gobuster modes target different layers of infrastructure
- Directory enumeration can reveal hidden files that directly expose sensitive information
- DNS and hostname resolution issues can impact results significantly
- Enumeration often requires troubleshooting the environment, not just running the tool

---

## Defensive Takeaways

From a defensive perspective, this room highlights why organizations should:

- restrict access to unnecessary directories and files
- avoid exposing sensitive scripts publicly
- properly configure DNS and virtual hosting
- monitor for aggressive enumeration patterns
- use rate limiting and detection rules for brute-force discovery activity

---

## Personal Reflection

This room was a good introduction to Gobuster and how useful it can be during the early phases of an assessment. The biggest value for me was not just learning the syntax, but also seeing how small environment issues can block progress if I do not stop and troubleshoot them.

I had to work through DNS-related problems and eventually use `/etc/hosts` to get enumeration working properly. Once that was fixed, the directory discovery process made much more sense, and I was able to identify the `secret` directory and recover the flag from `flag.js`. :contentReference[oaicite:5]{index=5}

Overall, this room helped reinforce that enumeration is not just about throwing tools at a target. It also requires understanding how name resolution, virtual hosting, and server behavior affect the results you get.

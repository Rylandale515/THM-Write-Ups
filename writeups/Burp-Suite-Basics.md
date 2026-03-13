# TryHackMe: Burp Suite — The Basics

## Room Information

| Category | Web Hacking |
|--------|--------|
| Platform | TryHackMe |
| Difficulty | Beginner |
| Focus | Web Application Testing |
| Tools Used | Burp Suite Community Edition, FoxyProxy |

---

# Overview

Burp Suite is one of the most widely used tools for **web application penetration testing**. It acts as an intercepting proxy between a browser and a web server, allowing a tester to inspect, modify, and replay HTTP/HTTPS traffic.

In this room, I explored the foundational components of Burp Suite, including:

• The Burp Proxy  
• Burp modules and navigation  
• Configuring browsers to use the proxy  
• Intercepting and modifying requests  
• Mapping web applications  
• Performing a simple XSS attack

This room focused primarily on **understanding the Burp Suite interface and workflow**, which is essential before moving into more advanced web exploitation techniques.

---

# Skills Demonstrated

• Web proxy configuration  
• HTTP request interception  
• Web traffic analysis  
• Web application mapping  
• Basic XSS exploitation  
• Burp Suite navigation and configuration

---

# Key Concepts

## What is Burp Suite?

Burp Suite is a **Java-based web security testing framework** developed by PortSwigger. It is commonly used during web application penetration tests to intercept and manipulate HTTP/HTTPS traffic.

Burp allows testers to:

• Capture requests sent from a browser  
• Modify requests before they reach the server  
• Analyze responses from the server  
• Send captured requests to other testing modules

This ability to sit between the browser and server makes it an extremely powerful manual testing tool.

---

# Burp Suite Editions

Burp Suite comes in three primary editions:

### Burp Suite Community Edition

Free version with core testing tools, including:

• Proxy  
• Repeater  
• Intruder (rate-limited)  
• Decoder  
• Comparer  
• Sequencer  

This edition focuses on **manual testing workflows**.

---

### Burp Suite Professional

Paid version used by security professionals. Includes:

• Automated vulnerability scanner  
• Faster Intruder attacks  
• Report generation  
• Burp Collaborator  
• API integrations  
• Extension marketplace support

---

### Burp Suite Enterprise

Enterprise edition is designed for **continuous automated scanning** of web applications.

This version runs on a server and performs scheduled vulnerability scans.

---

# Burp Suite Community Tools

The Community Edition still contains powerful modules for manual testing.

## Proxy

The **Burp Proxy** is the core feature of the framework.

It allows interception and modification of requests between the browser and the target server.

This eA decodertesters to:

• Inspect requests  
• Modify parameters  
• Send requests to other modules

---

## Repeater

Repeater allows testers to:

• Resend captured requests  
• Modify parameters  
• Test payloads repeatedly

This tool is commonly used when testing for vulnerabilities such as:

• SQL Injection  
• Authentication bypass  
• Parameter manipulation

---

## Intruder

Intruder automates request manipulation.

It is commonly used for:

• Brute force attacks  
• Parameter fuzzing  
• Payload testing

The Community Edition has **rate limits** applied.

---

## Decoder

A decoder is used to encode or decode data.

Common uses include:

• URL decoding  
• Base64 decoding  
• Payload encoding

---

## Comparer

Comparer allows comparison of two datasets.

Useful for comparing:

• Server responses  
• Authentication tokens  
• API outputs

---

## Sequencer

Sequencer analyzes the randomness of tokens such as:

• Session cookies  
• CSRF tokens

Weak randomness can lead to session prediction vulnerabilities.

---

# Installing Burp Suite

Burp Suite installation varies by operating system.

## Kali Linux

Burp Suite is pre-installed.

If missing:

``` sudo apt install burpsuite ```

---

## Windows / macOS / Linux

Download from:

https://portswigger.net/burp

Run the installer and follow the default configuration.

---

# Burp Suite Dashboard

The Burp Dashboard contains several components:

### Tasks

Background tasks Burp performs during testing.

Community Edition typically runs a **live passive crawl**.

---

### Event Log

Shows actions performed by Burp Suite, including:

• Proxy start events  
• Connection information  
• Errors

Answer:

**Event log**

---

### Issue Activity

Displays discovered vulnerabilities.

Primarily used in **Burp Professional**.

---

### Advisory

Contains vulnerability explanations and remediation guidance.

---

# Navigation in Burp Suite

Navigation is performed through the **top module bar**.

Modules include:

• Dashboard  
• Target  
• Proxy  
• Intruder  
• Repeater

Each module may contain **sub-tabs** with specific options.

---

## Useful Keyboard Shortcuts

| Shortcut | Module |
|--------|--------|
| Ctrl + Shift + D | Dashboard |
| Ctrl + Shift + T | Target |
| Ctrl + Shift + P | Proxy |
| Ctrl + Shift + I | Intruder |
| Ctrl + Shift + R | Repeater |

Answer:

**Proxy tab**

---

# Burp Suite Settings

Burp Suite includes two configuration levels.

## Global (User) Settings

These settings apply to **all Burp sessions**.

---

## Project Settings

These settings apply only to the **current project session**.

Note: Burp Community cannot save projects.

---

### Key Configuration Categories

• Sessions  
• Suite  
• Hotkeys  

Answers:

Cookie Jar Category:  
**Sessions**

Updates Category:  
**Suite**

Shortcut Configuration Category:  
**Hotkeys**

Client TLS override possible per project:  
**yea**

---

# The Burp Proxy

The **Burp Proxy** intercepts HTTP/HTTPS traffic between the browser and server.

Captured requests can be:

• Forwarded  
• Modified  
• Dropped  
• Sent to other modules

Burp also logs all traffic in:

• HTTP history  
• WebSocket history

---

# Configuring FoxyProxy

To send browser traffic through Burp:

Install **FoxyProxy** in Firefox.

Configure the proxy with:
IP: 127.0.0.1
Port: 8080

You can enable proxy mode in FoxyProxy and make sure Burp Proxy intercept is active.

Once configured, all browser traffic will pass through Burp.

---

# Target Tab

The **Target tab** helps map web applications.

Key components include:

## Site Map

Automatically builds a tree of discovered endpoints.

This helps identify:

• API endpoints  
• Hidden paths  
• Application structure

---

## Issue Definitions

Provides explanations of vulnerabilities detected by Burp scanners.

---

## Scope Settings

Allows defining the testing scope to reduce noise.

---

# Site Mapping Discovery

After browsing the target application, an unusual endpoint was discovered.

Flag obtained:
THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}

---

# Burp Browser

Burp includes a built-in Chromium browser configured for proxy use.

To launch:

Proxy → Open Browser

This avoids manual proxy configuration.

---

# Scoping Targets

To reduce noise, targets can be added to the **scope list**.

Steps:

1. Open Target tab  
2. Right-click the domain  
3. Select **Add to Scope**

Burp can then ignore out-of-scope traffic.

---

# Proxying HTTPS

Burp intercepts encrypted HTTPS traffic using its own certificate authority.

To trust the certificate:

1. Navigate to:
http://burp/cert

2. Download `cacert.der`

3. Import the certificate into Firefox

4. Trust it to identify websites

This allows HTTPS traffic to be intercepted and inspected.

---

# Example Attack — Reflect the XSS

The support form is located at:
http://MACHINE_IP/ticket/

contained a **client-side email filter**.

Client-side validation can often be bypassed using interception tools.

---

## Attack Process

1. Enable Burp Proxy intercept  
2. Submit legitimate form data  
3. Intercept the request  
4. Replace email value with XSS payload  
5. URL encode the payload  
6. Forward the request

Payload used:

``` <script>alert("Successful XSS")</script> ```

After forwarding the request, the browser executed the injected script.

This demonstrated a **reflected XSS vulnerability**.

---

# Security Lessons

Key takeaways from this room:

• Client-side filters are easily bypassed  
• Intercepting proxies are critical for web testing  
• Understanding HTTP requests is essential for exploitation  
• Burp Suite enables full visibility of web traffic

---

# Defensive Considerations

Developers should implement:

• Server-side input validation  
• Output encoding  
• Content Security Policy (CSP)  
• Proper input sanitization

These measures significantly reduce the risk of XSS vulnerabilities.

---

# Conclusion

This room introduced the core functionality of Burp Suite and demonstrated how intercepting proxies can be used to analyze and manipulate web traffic.

Understanding how to intercept and modify HTTP requests is a foundational skill for both:

• Web application penetration testing  
• Security vulnerability analysis

Future modules will build on these fundamentals, using more advanced tools such as **Burp Repeater, Intruder, and automated scanning workflows**.

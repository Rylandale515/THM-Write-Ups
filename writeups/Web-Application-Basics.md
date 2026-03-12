# Web Application Basics

## Room Information

| Platform | Difficulty | Category |
|----------|------------|----------|
| TryHackMe | Beginner | Web Application Fundamentals |

Room: https://tryhackme.com/room/webapplicationbasics

---

## Overview

This room covered the foundational concepts behind how web applications function. It introduced the major components of web applications, the structure of URLs, the format of HTTP requests and responses, common request methods, response status codes, and the role of HTTP headers in both functionality and security.

The room also reinforced how these concepts connect directly to web security. Understanding how browsers and servers communicate is essential before moving into more advanced topics such as web exploitation, authentication flaws, injection attacks, and insecure configurations.

---

## Skills Demonstrated

- Understanding front-end and back-end web application components
- Breaking down and analyzing URLs
- Identifying HTTP request and response structure
- Recognizing common HTTP request methods
- Interpreting HTTP response status codes
- Understanding request and response headers
- Identifying important web security headers
- Performing basic HTTP operations such as GET, POST, and DELETE

---

## Tools Used

- Web browser
- HTTP request emulator
- Static training site provided by TryHackMe

---

# Walkthrough

---

## Introduction

This room introduced the building blocks of web applications and explained how users interact with them through a browser. It also established the importance of understanding HTTP communication for both development and security work.

Key learning objectives included:

- Understanding what a web application is
- Learning the structure of a URL
- Understanding HTTP messages
- Reviewing HTTP methods and response codes
- Learning the purpose of HTTP headers
- Understanding basic security headers

---

## Web Application Overview

The room compared a web application to a planet to explain the difference between what users see and what happens underneath the surface.

### Front End

The front end refers to the visible and interactive part of a web application that users access through a web browser.

Key front-end technologies:

- **HTML**: Defines the structure and content of a webpage
- **CSS**: Controls the visual styling and layout
- **JavaScript**: Adds logic, interactivity, and dynamic behavior

### Back End

The back end includes the systems and services that support the web application behind the scenes.

Important back-end components:

- **Web Server**: Hosts and delivers content to users
- **Database**: Stores, retrieves, and modifies data
- **Infrastructure**: Includes servers, storage, networking, and supporting software
- **WAF (Web Application Firewall)**: Filters malicious requests before they reach the application

### Key Answers

- Component responsible for hosting and delivering content: **web server**
- Tool used to access web applications: **web browser**
- Protective filtering layer: **web application firewall**

---

## Uniform Resource Locator (URL)

A URL is the address used to access resources on the web. Understanding the different parts of a URL is important for security, development, and troubleshooting.

### URL Components

- **Scheme**: The protocol used, such as HTTP or HTTPS
- **User**: Optional authentication information
- **Host/Domain**: The website name or destination server
- **Port**: Directs traffic to the appropriate service
- **Path**: Identifies the specific resource being requested
- **Query String**: Passes additional information to the server
- **Fragment**: Refers to a specific section of a resource

### Security Notes

- **HTTPS** provides encrypted communication between browser and server
- **Typosquatting** refers to registering domains that closely resemble legitimate websites to trick users
- **Query strings** should be handled carefully because attackers can manipulate them
- **Fragments** can also be modified and should not be trusted blindly in client-side logic

### Key Answers

- Encrypted protocol: **HTTPS**
- Misspelled-domain abuse technique: **Typosquatting**
- URL part used to pass additional information: **Query String**

---

## HTTP Messages

HTTP messages are the packets of data exchanged between clients and servers. They form the basis of communication in web applications.

There are two main types:

- **HTTP Requests**: Sent by the client to the server
- **HTTP Responses**: Sent by the server back to the client

### Main Parts of an HTTP Message

- **Start Line**: Identifies the type of message
- **Headers**: Provide extra information and instructions
- **Empty Line**: Separates the headers from the body
- **Body**: Contains the actual data being sent

### Why This Matters

Understanding HTTP messages helps with:

- troubleshooting communication issues
- understanding browser-server interactions
- implementing and testing security controls
- analyzing requests and responses during assessments

### Key Answers

- Message returned by the server: **HTTP response**
- What follows the headers: **Empty Line**

---

## HTTP Request: Request Line and Methods

The request line is the first line of an HTTP request and contains:

- the HTTP method
- the URL path
- the HTTP version

### Common HTTP Methods

- **GET**: Retrieves data
- **POST**: Sends data to create or update something
- **PUT**: Replaces or updates a resource
- **DELETE**: Removes a resource
- **PATCH**: Updates part of a resource
- **HEAD**: Retrieves headers only
- **OPTIONS**: Shows supported methods
- **TRACE**: Often used for debugging
- **CONNECT**: Used for secure connections such as HTTPS tunnels

### Security Notes

- Sensitive information should not be exposed in GET requests
- POST, PUT, PATCH, and DELETE requests should validate input carefully
- OPTIONS and TRACE can expose unnecessary information if not restricted
- URL paths should be validated to prevent unauthorized access and injection risks

### HTTP Versions Covered

- **HTTP/0.9**
- **HTTP/1.0**
- **HTTP/1.1**
- **HTTP/2**
- **HTTP/3**

### Key Answers

- Most widely used HTTP version: **HTTP/1.1**
- Method used to describe communication options for a resource: **OPTIONS**
- Component that identifies the specific endpoint: **URL Path**

---

## HTTP Request: Headers and Body

Request headers provide additional context to the server about the request being made.

### Common Request Headers

- **Host**: Identifies the destination server
- **User-Agent**: Identifies the client software
- **Referer**: Indicates where the request came from
- **Cookie**: Stores and sends previously saved client data
- **Content-Type**: Tells the server what kind of data is being sent

### Request Body Formats

In methods such as POST and PUT, the request body contains the actual data being sent to the server.

Common formats include:

- **application/x-www-form-urlencoded**
- **multipart/form-data**
- **application/json**
- **application/xml**

### Security Notes

- Input in request bodies must always be validated and sanitized
- File uploads require careful handling
- Content type should be checked and enforced properly

### Key Answers

- Header that specifies the web server domain: **Host**
- Default content type for form submissions: **application/x-www-form-urlencoded**
- Part that contains host, agent, and content type details: **Request Headers**

---

## HTTP Response: Status Line and Status Codes

The first line of an HTTP response is called the **Status Line**.

It contains:

- HTTP version
- status code
- reason phrase

### Response Code Categories

- **100–199**: Informational responses
- **200–299**: Successful responses
- **300–399**: Redirection messages
- **400–499**: Client error responses
- **500–599**: Server error responses

### Common Status Codes

- **100 Continue**
- **200 OK**
- **301 Moved Permanently**
- **404 Not Found**
- **500 Internal Server Error**

### Key Answers

- Part containing HTTP version, status code, and explanation: **Status Line**
- Category indicating internal server issues: **Server Error Responses**
- Status code for missing resource: **404**

---

## HTTP Response: Headers and Body

Response headers provide instructions and metadata about the server’s reply.

### Important Response Headers

- **Date**: Shows when the response was generated
- **Content-Type**: Identifies the content format
- **Server**: Identifies the server software
- **Set-Cookie**: Sends cookies to the client
- **Cache-Control**: Controls caching behavior
- **Location**: Used for redirects

### Security Notes

- The **Server** header can reveal useful information to attackers
- Cookies should use the **Secure** flag to ensure they are only sent over HTTPS
- Cookies should use the **HttpOnly** flag to reduce JavaScript access and improve resistance to XSS-related theft
- Response bodies should be sanitized and escaped properly to help prevent injection attacks such as XSS

### Key Answers

- Header that may reveal server software: **Server**
- Cookie flag for HTTPS-only transmission: **Secure**
- Cookie flag to block JavaScript access: **HttpOnly**

---

## Security Headers

The room introduced several important HTTP security headers that strengthen web application security.

### Content-Security-Policy (CSP)

CSP helps reduce the risk of Cross-Site Scripting (XSS) by defining which sources are trusted for content such as scripts and styles.

Important directive covered:

- **script-src**

### Strict-Transport-Security (HSTS)

HSTS forces browsers to use HTTPS when communicating with the site.

Important directive covered:

- **includeSubDomains**

### X-Content-Type-Options

This header prevents browsers from guessing the MIME type of a resource.

Important directive covered:

- **nosniff**

### Referrer-Policy

Controls how much referrer information is shared when moving between pages or sites.

Examples covered:

- **no-referrer**
- **same-origin**
- **strict-origin**
- **strict-origin-when-cross-origin**

### Key Answers

- CSP property for script sources: **script-src**
- HSTS directive for subdomains: **includeSubDomains**
- Directive that stops MIME type sniffing: **nosniff**

---

## Practical Task: Making HTTP Requests

The final practical section used a static site emulator to apply the concepts covered earlier.

Three HTTP actions were completed:

### GET Request

A GET request was made to `/api/users`.

Flag:

**THM{YOU_HAVE_JUST_FOUND_THE_USER_LIST}**

### POST Request

A POST request was made to `/api/user/2` to change Bob’s country from UK to US.

Flag:

**THM{YOU_HAVE_MODIFIED_THE_USER_DATA}**

### DELETE Request

A DELETE request was made to `/api/user/1` to remove the user.

Flag:

**THM{YOU_HAVE_JUST_DELETED_A_USER}**

This practical section tied the room together by showing how request methods directly affect application behavior.

---

## Findings Summary

| Task | Result |
|------|--------|
| Hosting component | `web server` |
| Client access tool | `web browser` |
| Protective filtering layer | `web application firewall` |
| Encrypted protocol | `HTTPS` |
| Fake domain attack technique | `Typosquatting` |
| URL part for extra data | `Query String` |
| Server reply message type | `HTTP response` |
| Separator after headers | `Empty Line` |
| Common HTTP version | `HTTP/1.1` |
| Method that reveals allowed communication options | `OPTIONS` |
| Endpoint identifier | `URL Path` |
| Header identifying destination server | `Host` |
| Default form content type | `application/x-www-form-urlencoded` |
| Response first line | `Status Line` |
| Missing resource code | `404` |
| Header revealing server software | `Server` |
| HTTPS-only cookie flag | `Secure` |
| JavaScript-restricted cookie flag | `HttpOnly` |
| CSP script source directive | `script-src` |
| HSTS subdomain directive | `includeSubDomains` |
| MIME-sniffing prevention directive | `nosniff` |
| GET flag | `THM{YOU_HAVE_JUST_FOUND_THE_USER_LIST}` |
| POST flag | `THM{YOU_HAVE_MODIFIED_THE_USER_DATA}` |
| DELETE flag | `THM{YOU_HAVE_JUST_DELETED_A_USER}` |

---

## Security Takeaways

This room reinforced several important security lessons:

- Web applications rely heavily on properly structured HTTP requests and responses
- URL components such as query strings and fragments can become attack surfaces if not handled carefully
- Request methods must be validated and authorized correctly
- Response headers can leak useful information if not configured carefully
- Security headers provide an important defensive layer against common browser-based attacks
- Cookies should be configured securely with flags such as `Secure` and `HttpOnly`
- Understanding normal web traffic is essential before moving into web exploitation or web application testing

---

## Personal Reflection

This room was a strong foundation for understanding how web applications actually work under the hood. It helped connect together the browser, the server, and the HTTP protocol in a way that made the bigger picture clearer. Instead of just memorizing methods or status codes, this room helped show how each part plays a role in both functionality and security.

The most useful part for me was seeing how these basics directly tie into security. Things like query strings, request bodies, headers, cookies, and status codes seem simple at first, but they all become important once testing or defending web applications. This room gave me a much stronger base for moving into more advanced web application security topics.

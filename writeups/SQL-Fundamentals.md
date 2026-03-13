# TryHackMe: SQL Fundamentals

## Overview

Databases are a foundational component of modern computing and cybersecurity. Nearly every application—whether a web platform, authentication service, SIEM system, or threat detection tool—relies on databases to store and retrieve information.

This room introduces the fundamentals of databases and **Structured Query Language (SQL)**, the primary language for interacting with relational databases. Understanding SQL is critical for both **offensive security** (e.g., SQL injection exploitation) and **defensive security** (e.g., log analysis, threat hunting, and database hardening).

In this lab, I explored how databases are structured, how SQL queries interact with stored data, and how common SQL operations are used to retrieve, manipulate, and analyze information.

---

# Learning Objectives

By completing this room, I learned how to:

- Understand what databases are and why they are important
- Distinguish between relational and non-relational databases
- Understand the structure of tables, rows, and columns
- Understand primary keys and foreign keys
- Use SQL to interact with relational databases
- Perform CRUD operations (Create, Read, Update, Delete)
- Use SQL clauses to filter and sort results
- Apply logical operators in queries
- Use SQL functions to manipulate and analyze data

---

# Task 1 — Introduction

Databases store structured information used by applications, systems, and security tools.

Examples include:

- User authentication data
- Social media content
- Application logs
- Security monitoring data

Understanding databases is essential because many security vulnerabilities—including **SQL Injection attacks**—target database interactions.

---

# Task 2 — Databases 101

## What is a Database?

A **database** is an organized collection of structured data that can be stored, accessed, modified, and analyzed efficiently.

Examples of data stored in databases include:

- User credentials
- Product inventories
- Application logs
- Transaction histories

---

## Types of Databases

### Relational Databases (SQL)

Relational databases store structured data in tables composed of:

- Rows (records)
- Columns (attributes)

Examples include:

- MySQL
- MariaDB
- PostgreSQL
- Oracle Database

These are ideal when data follows a consistent structure.

---

### Non-Relational Databases (NoSQL)

Non-relational databases store data in flexible formats such as:

- Key-value pairs
- Documents
- Collections

These are useful when data varies significantly in structure.

Example technologies include:

- MongoDB
- Cassandra
- Redis

---

## Tables, Rows, and Columns

In relational databases:

- **Tables** store datasets
- **Columns** define attributes
- **Rows** represent individual records

Example table:

| id | name | publication_date |
|----|------|-----------------|

Each row contains a unique record.

---

## Primary Keys

A **primary key** uniquely identifies each record within a table.

Example:

```
book_id
```

Primary keys must be:

- Unique
- Non-null

---

## Foreign Keys

A **foreign key** links one table to another.

Example:

```
Books table → author_id
Authors table → id
```

This allows relationships between datasets.

---

# Task 3 — SQL

## What is SQL?

**Structured Query Language (SQL)** is used to interact with relational databases.

It allows users to:

- Query data
- Insert records
- Update records
- Delete records
- Create and modify database structures

---

## Database Management Systems (DBMS)

A DBMS provides an interface between users and databases.

Examples include:

- MySQL
- MariaDB
- Oracle Database
- MongoDB

In this lab, the database environment was accessed through **MySQL**.

---

# Task 4 — Database and Table Statements

SQL provides commands to create and manage database structures.

---

## Creating a Database

```
CREATE DATABASE thm_bookmarket_db;
```

---

## Listing Databases

```
SHOW DATABASES;
```

---

## Selecting a Database

```
USE thm_bookmarket_db;
```

---

## Creating a Table

```
CREATE TABLE book_inventory (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    publication_date DATE
);
```

This table contains:

- `book_id` → unique identifier
- `book_name` → title of the book
- `publication_date` → date published

---

## Listing Tables

```
SHOW TABLES;
```

---

## Viewing Table Structure

```
DESCRIBE book_inventory;
```

---

## Altering Tables

Tables can be modified using the `ALTER` statement.

Example:

```
ALTER TABLE book_inventory
ADD page_count INT;
```

---

# Task 5 — CRUD Operations

CRUD operations are the core data manipulation operations in databases.

| Operation | SQL Statement |
|-----------|--------------|
| Create | INSERT |
| Read | SELECT |
| Update | UPDATE |
| Delete | DELETE |

---

## Create (INSERT)

```
INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14", "Security architecture guide");
```

---

## Read (SELECT)

Retrieve all records:

```
SELECT * FROM books;
```

Retrieve specific columns:

```
SELECT name, description FROM books;
```

---

## Update (UPDATE)

Modify existing data.

```
UPDATE books
SET description = "Updated description"
WHERE id = 1;
```

---

## Delete (DELETE)

Remove records from a table.

```
DELETE FROM books WHERE id = 1;
```

---

# Task 6 — SQL Clauses

Clauses refine how SQL queries retrieve data.

---

## DISTINCT

Returns only unique values.

```
SELECT DISTINCT name FROM books;
```

---

## GROUP BY

Groups rows sharing a column value.

```
SELECT name, COUNT(*)
FROM books
GROUP BY name;
```

---

## ORDER BY

Sorts results.

Ascending:

```
ORDER BY published_date ASC;
```

Descending:

```
ORDER BY published_date DESC;
```

---

## HAVING

Filters grouped results.

```
SELECT name, COUNT(*)
FROM books
GROUP BY name
HAVING name LIKE "%Hack%";
```

---

# Task 7 — SQL Operators

Operators allow more precise filtering.

---

## LIKE

Searches for patterns.

```
SELECT * FROM books
WHERE description LIKE "%guide%";
```

---

## Logical Operators

### AND

```
WHERE category = "Offensive Security" AND name = "Bug Bounty Bootcamp";
```

### OR

```
WHERE name LIKE "%Android%" OR name LIKE "%iOS%";
```

### NOT

```
WHERE NOT description LIKE "%guide%";
```

---

## BETWEEN

Checks if values fall within a range.

```
WHERE id BETWEEN 2 AND 4;
```

---

## Comparison Operators

| Operator | Meaning |
|--------|--------|
| = | Equal |
| != | Not equal |
| < | Less than |
| > | Greater than |
| <= | Less than or equal |
| >= | Greater than or equal |

Example:

```
SELECT * FROM books
WHERE published_date > "2020-01-01";
```

---

# Task 8 — SQL Functions

Functions help manipulate and analyze database data.

---

# String Functions

## CONCAT()

Combines strings.

```
SELECT CONCAT(name, " is a type of ", category)
FROM books;
```

---

## GROUP_CONCAT()

Combines values from multiple rows.

```
SELECT category, GROUP_CONCAT(name)
FROM books
GROUP BY category;
```

---

## SUBSTRING()

Extracts part of a string.

```
SELECT SUBSTRING(published_date,1,4)
FROM books;
```

---

## LENGTH()

Returns character length.

```
SELECT LENGTH(name)
FROM books;
```

---

# Aggregate Functions

Aggregate functions summarize multiple records.

---

## COUNT()

Counts records.

```
SELECT COUNT(*) FROM books;
```

---

## SUM()

Adds numeric values.

```
SELECT SUM(price) FROM books;
```

---

## MAX()

Returns the highest value.

```
SELECT MAX(published_date) FROM books;
```

---

## MIN()

Returns the lowest value.

```
SELECT MIN(published_date) FROM books;
```

---

# Flags Captured

```
THM{575a947132312f97b30ee5aeebba629b723d30f9}
THM{692aa7eaec2a2a827f4d1a8bed1f90e5e49d2410}
```

---

# Skills Demonstrated

- SQL query development
- Database schema understanding
- Data retrieval and manipulation
- Logical query filtering
- SQL aggregation and analysis
- Database security awareness

---

# Security Relevance

Understanding SQL is essential for cybersecurity because many vulnerabilities involve database interactions, including:

- SQL Injection
- Data exfiltration attacks
- Authentication bypass
- Privilege escalation through database queries

Security professionals frequently analyze database logs and queries when investigating incidents.

---

# Final Thoughts

This room provided a strong foundation in SQL and relational database structure. These skills are essential when working with web applications, analyzing security incidents, or investigating compromised systems.

From a security standpoint, understanding how queries interact with databases enables defenders to detect malicious activity more effectively and penetration testers to identify vulnerabilities such as SQL injection.

Mastering SQL fundamentals is a critical step toward deeper topics such as **web application exploitation, database forensics, and security monitoring**.

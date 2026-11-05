# Source Code Disclosure via Backup Files

This lab exposes its source code through backup files sitting in a hidden directory.

**Goal:** Find and submit the hard-coded database password buried in that leaked source.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

pic 1

---

A `/robots.txt` file is present:

pic 2

It points to a `/backup` endpoint:

pic 3

Reading the file there reveals the database password "td510drfnep124ibalwc0xw32d1cp3or":

pic 4

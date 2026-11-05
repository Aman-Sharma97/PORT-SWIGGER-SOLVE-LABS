# Source Code Disclosure via Backup Files

This lab exposes its source code through backup files sitting in a hidden directory.

**Goal:** Find and submit the hard-coded database password buried in that leaked source.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

<img width="763" height="529" alt="1" src="https://github.com/user-attachments/assets/ab90c04d-dc2e-41d7-8fde-f6db3f295db8" />


---

A `/robots.txt` file is present:

<img width="801" height="99" alt="2" src="https://github.com/user-attachments/assets/ff60e236-9658-4e26-be87-0762b5a49268" />


It points to a `/backup` endpoint:

<img width="790" height="165" alt="3" src="https://github.com/user-attachments/assets/eb61c3fa-e946-4ad4-a4a9-29db56823432" />


Reading the file there reveals the database password "td510drfnep124ibalwc0xw32d1cp3or":

<img width="719" height="563" alt="4" src="https://github.com/user-attachments/assets/f00a572b-eaa3-4f64-ab4f-59cbbd312e1a" />


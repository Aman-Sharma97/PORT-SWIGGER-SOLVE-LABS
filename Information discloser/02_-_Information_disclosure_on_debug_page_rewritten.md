# Information Disclosure via a Debug Page

This lab exposes a debug page that reveals sensitive application details.

**Goal:** Find and submit the `SECRET_KEY` environment variable.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

<img width="762" height="415" alt="1" src="https://github.com/user-attachments/assets/a259ebe9-1a65-42c2-bfbc-2d6cac4751e5" />


---

The debug page is located at `/cgi-bin/phpinfo.php`:

<img width="318" height="144" alt="2" src="https://github.com/user-attachments/assets/c2d0f874-829e-46c2-be88-61283d9bbc60" />


The secret key ("8f4xrr692ckcxycofkaupwwu37cse6io") appears in the "Environment" section:

<img width="663" height="186" alt="3" src="https://github.com/user-attachments/assets/cf3e40a7-011c-4e28-a507-6f5dbef9297f" />


# Information Disclosure via a Debug Page

This lab exposes a debug page that reveals sensitive application details.

**Goal:** Find and submit the `SECRET_KEY` environment variable.

---

**Reference:**
- https://portswigger.net/web-security/information-disclosure/exploiting

pic 1

---

The debug page is located at `/cgi-bin/phpinfo.php`:

pic 2

The secret key ("8f4xrr692ckcxycofkaupwwu37cse6io") appears in the "Environment" section:

pic 3

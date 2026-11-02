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

<img width="857" height="445" alt="1" src="https://github.com/user-attachments/assets/06a43497-c72b-4d39-8c18-d43d08d8589a" />


---

Lab URL generated: https://0a9f008704cb076382ef4c7e0060006d.web-security-academy.net/

A naive brute-force attempt triggers an "IP blocked" message:

<img width="722" height="328" alt="2" src="https://github.com/user-attachments/assets/8469bf86-2212-4e9d-875c-118cdc049433" />


The 8th attempt shows an "incorrect password" error:

<img width="588" height="600" alt="3" src="https://github.com/user-attachments/assets/6a5f9edf-6b20-4b4f-b923-c62930112604" />


To sneak the correct wiener:peter credentials in between failed attempts, I built a password list that repeats the correct password after every password in the candidate list:

<img width="160" height="220" alt="4" src="https://github.com/user-attachments/assets/a6a9b7bd-f6ba-4ec1-83c7-2cfa5eafc7b0" />


And a matching username list:

<img width="126" height="217" alt="5" src="https://github.com/user-attachments/assets/f4acc26e-f0c3-430c-b498-c1ec3e9617c8" />


I used the Pitchfork attack type, which steps through both lists in lockstep, value by value:

<img width="1145" height="501" alt="6" src="https://github.com/user-attachments/assets/cd4acf12-ed24-4c64-92d9-665fe60eb833" />


Launching the attack:

<img width="552" height="465" alt="7" src="https://github.com/user-attachments/assets/9b3f0c54-63a7-4ca5-b83f-ef295fc41956" />


It runs, but doesn't succeed:

<img width="682" height="286" alt="8" src="https://github.com/user-attachments/assets/f38c5934-693a-4916-a33c-e91197992b86" />


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

<img width="549" height="461" alt="9" src="https://github.com/user-attachments/assets/6e616060-3ca9-42bd-af33-00b3a97d41e3" />


Even this didn't work.

Finally, adding a 1000ms delay between requests under Resource Pool settings did the trick:

<img width="455" height="244" alt="10" src="https://github.com/user-attachments/assets/9fab4caa-a616-4013-82d2-f9ff2537ad1a" />


With that in place, the attack succeeds — carlos's password turns out to be "moon":

<img width="776" height="408" alt="11" src="https://github.com/user-attachments/assets/bb9bd7df-c2dd-43c1-9dfd-70f0ee08fc8d" />


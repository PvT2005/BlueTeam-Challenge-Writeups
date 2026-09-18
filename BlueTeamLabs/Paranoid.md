# Paranoid — Blue Team Labs Writeup

---

## Question 1. What account was compromised?

I noticed a lot of failed login attempts coming from IP `192.168.4.155` targeting the `btlo` account. After scrolling through the logs, I spotted one successful login among all those failures. That confirms the compromised account is `btlo`.

<img width="1768" height="1033" alt="Image" src="https://github.com/user-attachments/assets/45c48949-0fed-4125-826f-056a74039ec0" />
<img width="1721" height="882" alt="Image" src="https://github.com/user-attachments/assets/b2bd1c51-d645-49fd-ae0d-c86107ee8bd2" />

---

## Question 2. What attack type was used to gain initial access?

Looking deeper into the logs, there were tons of failed SSH login attempts before the successful one. This pattern is classic `Brute Force`.

---

## Question 3. What is the attacker's IP address?

`192.168.4.155`

---

## Question 4. What tool was used to perform system enumeration?

Once the attacker got into the machine, they probably ran some commands to learn about the system. I checked the command history and found that they ran `linpeas.sh` — a well-known Linux privilege escalation enumeration script.

<img width="1861" height="729" alt="Image" src="https://github.com/user-attachments/assets/876e081e-7836-4172-b38c-3d5dee7a19a2" />

---

## Question 5. What is the name of the binary and pid used to gain root?

The attacker ran `whoami` right at the beginning — probably to check which user they were logged in as. After that, they executed the `linpeas` enumeration script. Then they prepared and compiled a binary called `evil`, ran it, and immediately ran `whoami` again. That second `whoami` check suggests they expected their identity to change. The fact that they could read `/etc/shadow` right after confirms that `evil` successfully escalated their privileges to root.

<img width="1920" height="158" alt="Image" src="https://github.com/user-attachments/assets/e0dcd8b0-b31b-47b8-bba4-617c258194f6" />

The binary name is `evil` and the PID used to gain root is `829992`.

---

## Question 6. What CVE was exploited to gain root access?

`CVE-2021-3156`

---

## Question 7. What type of vulnerability is this?

I looked up the CVE number on the NIST website. The vulnerability type is `Heap-Based Buffer Overflow`.

---

## Question 8. What file was exfiltrated once root was gained?

After getting root access, the attacker read the `/etc/shadow` file, which stores the system's password hashes.

<img width="1867" height="732" alt="Image" src="https://github.com/user-attachments/assets/bfa90a09-a4b4-459f-a5ba-bc6823b91ebd" />
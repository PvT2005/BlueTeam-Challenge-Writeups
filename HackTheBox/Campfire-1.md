# Campfire-1 — HackTheBox Writeup

---

## Question 1. Analyzing Domain Controller Security Logs, can you confirm the UTC date & time when the kerberoasting activity occurred?

Kerberoasting is an attack technique that exploits a weakness in the service ticket granting phase of the Kerberos protocol within Active Directory environments. The goal of this technique is to steal the plaintext passwords of service accounts. I noticed that the Ticket Encryption Type is `0x17`, which corresponds to the RC4-HMAC algorithm — a very weak encryption method that is easy to crack. Additionally, the service name belongs to a user account, which is a strong indicator of a Kerberoasting attack. The activity occurred at `2024-05-21 03:18:09`.

<img width="1281" height="886" alt="Image" src="https://github.com/user-attachments/assets/d6e5b8ec-c2c2-4102-b0a7-bcffe960542c" />

---

## Question 2. What is the Service Name that was targeted?

From the same event in Question 1, the Service Name that was targeted is `MSSQLService`.

---

## Question 3. It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?

I found the IP Address of the workstation from the same Security Event Log in Question 1. The answer is `172.17.79.129`.

---

## Question 4. Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network?

I examined the PowerShell logs and found evidence that the attacker bypassed the PowerShell script execution policy to run malicious scripts without restrictions. PowerShell has several execution policies that can prevent unauthorized scripts from running, but by bypassing these policies, attackers can evade detection mechanisms.

<img width="1316" height="848" alt="Image" src="https://github.com/user-attachments/assets/435aaaab-e024-4d0a-804d-22f1aed0bba7" />

I found evidence that the script used is `PowerView.ps1`. PowerView is a PowerShell tool designed for network and Active Directory enumeration. It is part of the PowerSploit framework and is often used by penetration testers and attackers for:

- **AD Enumeration**: Discovering information about the domain, users, groups, computers, and more.
- **Finding Privileged Accounts**: Identifying accounts with high privileges that might be targeted for attacks.
- **Mapping AD Relationships**: Understanding trust relationships, group memberships, and user-to-computer associations.

<img width="1312" height="892" alt="Image" src="https://github.com/user-attachments/assets/4dfa1f61-641d-4da1-a65a-ecf599572732" />

---

## Question 5. When was this script executed? (UTC)

From the PowerShell log in Question 4, the script was executed at `2024-05-21 03:16:32`.

---

## Question 6. What is the full path of the tool used to perform the actual kerberoasting attack?

I parsed the Prefetch files using PECmd (by Eric Zimmerman) and opened the resulting CSV in Timeline Explorer. I then looked for any executables that were run around the timeline we established so far. I spotted an executable named after a well-known Active Directory offensive tool.

<img width="810" height="585" alt="Image" src="https://github.com/user-attachments/assets/7f90ddb5-b0e0-46e0-8063-894b8c8efd8a" />

To get the full path of the file, I went to the Files Loaded column and double-clicked the value to see all files loaded by this tool at execution. The answer is `C:\Users\Alonzo.spire\Downloads\Rubeus.exe`.

<img width="922" height="605" alt="Image" src="https://github.com/user-attachments/assets/6f2a8fda-9423-4866-ab95-f54bf5dfff4e" />

---

## Question 7. When was the tool executed to dump credentials? (UTC)

From the Prefetch analysis in Question 6, the tool was executed to dump credentials at `2024-05-21 03:18:08`.
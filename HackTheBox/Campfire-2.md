# Campfire-2 — HackTheBox Writeup

---

## Question 1. When did the ASREP Roasting attack occur, and when did the attacker request the Kerberos ticket for the vulnerable user?

ASREP Roasting is an attack where the attacker sends a fake AS-REQ request to the KDC server, asking for a TGT ticket. Because the target account does not require pre-authentication, the KDC just accepts the request and sends back an AS-REP response. Inside this AS-REP response, there is a piece of data encrypted with the password hash of the target user. The attacker then takes this AS-REP packet offline and uses cracking tools to recover the original password.

I noticed that a Kerberos authentication ticket was requested from user `arthur.kyle`, and the Ticket Encryption Type is `0x17`, which is a weak encryption algorithm. This is a strong indicator of an ASREP Roasting attack. So the attacker requested the Kerberos ticket for the vulnerable user at `2024-05-29 06:36:40`.

<img width="1337" height="932" alt="Image" src="https://github.com/user-attachments/assets/d0c21c23-477d-428e-bb9d-8be37147cc54" />

---

## Question 2. Please confirm the User Account that was targeted by the attacker.

The User Account that was targeted by the attacker is `arthur.kyle`.

---

## Question 3. What was the SID of the account?

The SID of the account arthur.kyle is `S-1-5-21-3239415629-1862073780-2394361899-1601`.

<img width="1337" height="932" alt="Image" src="https://github.com/user-attachments/assets/c1763f08-f6bb-49bf-a28a-66dac2a025fd" />

---

## Question 4. It is crucial to identify the compromised user account and the workstation responsible for this attack. Please list the internal IP address of the compromised asset to assist our threat-hunting team.

The internal IP address of the compromised asset is `172.17.79.129`.

---

## Question 5. We do not have any artifacts from the source machine yet. Using the same DC Security logs, can you confirm the user account used to perform the ASREP Roasting attack so we can contain the compromised account/s?

The system logged Event ID 4769 for `happy.grunwald`, which proves that this account had already been fully compromised before the attack. The user account used to perform the ASREP Roasting attack is `happy.grunwald`.

<img width="1322" height="925" alt="Image" src="https://github.com/user-attachments/assets/19dc26fb-b1c1-4c87-8bb2-f8bd4c09b1a2" />
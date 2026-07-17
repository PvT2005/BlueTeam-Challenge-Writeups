# Reaper — HackTheBox Writeup

---

## Question 1. What is the IP Address for Forela-Wkstn001?

I filtered for `nbns` (NetBIOS Name Service) to find the Refresh packet, which allows a device to refresh its NetBIOS name registration on the network. I can see that this IP address is refreshing its NetBIOS name as `Forela-Wkstn001`.

<img width="1911" height="940" alt="Image" src="https://github.com/user-attachments/assets/872742be-5a1e-4fd6-b288-4f08834031da" />

---

## Question 2. What is the IP Address for Forela-Wkstn002?

Using the same method, the IP address of Forela-Wkstn002 is `172.17.79.136`.

<img width="1886" height="575" alt="Image" src="https://github.com/user-attachments/assets/c79cd42c-3c43-4ed1-8f9d-cfe35a36e9af" />

---

## Question 3. What is the username of the account whose hash was stolen by attacker?

I filtered for the `ntlmssp` protocol in Wireshark and observed the packet exchange process between several IP addresses. This is a classic scenario of an NTLM Relay Attack. The attacker did not need to crack the password of user `arthur.kyle`. Instead, they relayed the ongoing authentication credentials to gain unauthorized access to Server 172.17.79.129.

<img width="1917" height="431" alt="Image" src="https://github.com/user-attachments/assets/dd346988-7143-4413-8f4a-457ea96cf006" />

---

## Question 4. What is the IP Address of Unknown Device used by the attacker to intercept credentials?

The IP address of the unknown device used by the attacker to intercept credentials is `172.17.79.135`.

---

## Question 5. What was the fileshare navigated by the victim user account?

I filtered for `smb2` traffic in Wireshark and searched for the keyword `BAD_NETWORK_NAME` in the packet details. The answer is `\\DC01\Trip`.

<img width="1878" height="897" alt="Image" src="https://github.com/user-attachments/assets/a60ebdc5-b05c-4433-b03e-55e24417b3fd" />

---

## Question 6. What is the source port used to logon to target workstation using the compromised account?

In the provided Event Logs, I filtered for Event ID 4624 with Logon Type 3. The Logon Process value is `NtLmSsp` and the Authentication Package value is `NTLM`. The source port is `40252`.

<img width="1221" height="927" alt="Image" src="https://github.com/user-attachments/assets/9c616f6f-6e10-40ec-a864-9b94c4fe805a" />

---

## Question 7. What is the Logon ID for the malicious session?

From the same event in Question 6, the Logon ID for the malicious session is `0x64A799`.

---

## Question 8. The detection was based on the mismatch of hostname and the assigned IP Address. What is the workstation name and the source IP Address from which the malicious logon occur?

In Question 2, I identified the IP address of Forela-Wkstn002 as 172.17.79.136. However, in this logon event, the source IP address for Forela-Wkstn002 appears as 172.17.79.135, which is the attacker's address. This mismatch is evidence that the attacker successfully performed an NTLM Relay Attack. The answer is `FORELA-WKSTN002, 172.17.79.135`.

<img width="1180" height="902" alt="Image" src="https://github.com/user-attachments/assets/df9d8d4b-cfb5-4159-b149-b9fdd1791ddd" />

---

## Question 9. At what UTC time did the malicious logon happen?

`2024-07-31 04:55:16`

---

## Question 10. What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?

I filtered for Event ID 5140 and found the share name accessed as part of the authentication process by the malicious tool used by the attacker is `\\*\IPC$`.

<img width="1212" height="942" alt="Image" src="https://github.com/user-attachments/assets/e013c4c0-4b64-4024-81c7-eca8d7826621" />
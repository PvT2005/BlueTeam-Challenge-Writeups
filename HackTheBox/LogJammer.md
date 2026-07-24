# LogJammer — HackTheBox Writeup

---

## Question 1. When did the cyberjunkie user first successfully log into his computer? (UTC)

I filtered by Event ID 4624 to find the first successful logon event for the cyberjunkie user. The first successful logon time is `27/03/2023 14:37:09`.

<img width="1562" height="902" alt="Image" src="https://github.com/user-attachments/assets/ac699bdf-cb13-4777-b547-d61547861028" />

---

## Question 2. The user tampered with firewall settings on the system. Analyze the firewall event logs to find out the Name of the firewall rule added?

The Rule Name is `Metasploit C2 Bypass`. Metasploit is a very well-known framework that hackers use to exploit vulnerabilities and spread malware.

The Direction is Outbound and the Action is Allow. This rule allows outgoing connections. It actively connects to a C2 (Command and Control) server to receive commands or send data out, which makes it easy to bypass NAT/Firewall systems.

<img width="1561" height="917" alt="Image" src="https://github.com/user-attachments/assets/b664b3f3-9fd1-4001-9356-409a21edb3bc" />

---

## Question 3. Whats the direction of the firewall rule?

The direction of the firewall rule is `Outbound`.

---

## Question 4. The user changed audit policy of the computer. Whats the Subcategory of this changed policy?

To find the changed audit policy event, I searched in the Security Event Log. I filtered by Event ID 4719, which records changes to system audit policies. The Subcategory of this changed policy is `Other Object Access Events`.

<img width="1561" height="912" alt="Image" src="https://github.com/user-attachments/assets/7bfff561-a907-42c5-969c-de2327a5c337" />

---

## Question 5. The user "cyberjunkie" created a scheduled task. Whats the name of this task?

I filtered by Event ID 4698, which records the creation of a scheduled task, with the username cyberjunkie. The name of this task is `HTB-AUTOMATION`.

<img width="1585" height="921" alt="Image" src="https://github.com/user-attachments/assets/e57ad73c-b518-41a9-919e-da58d577f5c1" />

---

## Question 6. Whats the full path of the file which was scheduled for the task?

The full path of the file which was scheduled for the task is `C:\Users\CyberJunkie\Desktop\Automation-HTB.ps1`.

<img width="1566" height="897" alt="Image" src="https://github.com/user-attachments/assets/d9d6ce40-67ba-4ddb-9106-ba90cf9f2770" />

---

## Question 7. What are the arguments of the command?

Based on the scheduled task details from Question 6, the answer is `-A cyberjunkie@hackthebox.eu`.

---

## Question 8. The antivirus running on the system identified a threat and performed actions on it. Which tool was identified as malware by antivirus?

`Sharphound` was identified as malware by the antivirus.

<img width="1571" height="926" alt="Image" src="https://github.com/user-attachments/assets/65117c8a-8bcc-49c6-814d-4536a2890ca0" />

---

## Question 9. Whats the full path of the malware which raised the alert?

Looking at the screenshot from Question 8, the full path of the malware is `C:\Users\CyberJunkie\Downloads\SharpHound-v1.1.0.zip`.

---

## Question 10. What action was taken by the antivirus?

Windows Defender logs its response under the same pair of events: Event ID 1116 and Event ID 1117. The response action can be quarantine, remove, block, or allow, depending on Defender settings. The action taken is `Quarantine`.

<img width="1547" height="907" alt="Image" src="https://github.com/user-attachments/assets/42854ddd-45a0-42a1-870c-1ea903e96ae0" />

---

## Question 11. The user used Powershell to execute commands. What command was executed by the user?

I checked the PowerShell Operational log. Event ID 4104 does not just give you fragments — it records the full ScriptBlock, including the exact command typed by the user. It looks like the attacker was checking the hash of the malware file. The answer is `Get-FileHash -Algorithm md5 .\Desktop\Automation-HTB.ps1`.
<img width="1555" height="918" alt="Image" src="https://github.com/user-attachments/assets/7cd7ad50-5436-4fe5-8076-6428e75b55a9" />
---

## Question 12. We suspect the user deleted some event logs. Which Event log file was cleared?

Event ID 104 in System.evtx tells you exactly what was cleared and by whom. In this case, the user targeted the firewall log. The answer is `Microsoft-Windows-Windows Firewall With Advanced Security/Firewall`.

<img width="1539" height="935" alt="Image" src="https://github.com/user-attachments/assets/7a8c0ca3-e441-42ad-9801-eff076dd689b" />
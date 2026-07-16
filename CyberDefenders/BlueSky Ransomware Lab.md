# BlueSky Ransomware Lab — CyberDefenders Writeup

---

## Question 1. Knowing the source IP of the attack allows security teams to respond to potential threats quickly. Can you identify the source IP responsible for potential port scanning activity?

The source IP responsible for potential port scanning activity is `87.96.21.84`.

---

## Question 2. During the investigation, it's essential to determine the account targeted by the attacker. Can you identify the targeted account username?

I noticed many connections to port 1433, which is the default TCP port for Microsoft SQL Server. I filtered by `TDS` and saw that normal sessions set up encryption during the TDS Pre-Login step, but the attacker's sessions did not use encryption.

<img width="1920" height="539" alt="Image" src="https://github.com/user-attachments/assets/54505ac9-3f70-41d0-aa75-abfaaf8e03d7" />
<img width="1906" height="547" alt="Image" src="https://github.com/user-attachments/assets/bdd6f643-b7c4-4aa9-b944-214448997a5a" />

Based on the unencrypted `TDS7 login` packet, the targeted account username is `sa`.

<img width="1911" height="665" alt="Image" src="https://github.com/user-attachments/assets/127d5469-abd6-4e63-b0e0-617328358f89" />

---

## Question 3. We need to determine if the attacker succeeded in gaining access. Can you provide the correct password discovered by the attacker?

The correct password is `cyb3rd3f3nd3r$`.

---

## Question 4. Attackers often change some settings to facilitate lateral movement within a network. What setting did the attacker enable to control the target host further and execute further commands?

Looking at the TDS Query details, the attacker opened a Windows Command Prompt directly from inside SQL Server and ran operating system commands.

<img width="1917" height="647" alt="Image" src="https://github.com/user-attachments/assets/205e2195-9b55-4b5c-a2d4-fe86b3a468c6" />

---

## Question 5. Process injection is often used by attackers to escalate privileges within a system. What process did the attacker inject the C2 into to gain administrative privileges?

In a normal Windows environment, the PowerShell HostName is usually `ConsoleHost` or the name of the calling application. Here the HostName shows `MSFConsole`, which means a Metasploit payload was successfully executed on this machine and is using the PowerShell engine to run remote commands. The combination of MSFConsole and winlogon.exe is a classic sign of Process Injection. Instead of running a suspicious `.exe` file that antivirus could easily detect, the attacker injected malicious code directly into the memory space of the legitimate `winlogon.exe` process.

<img width="1561" height="915" alt="Image" src="https://github.com/user-attachments/assets/a6b0d353-1591-4dda-a0d4-5f02c50edb17" />

---

## Question 6. Following privilege escalation, the attacker attempted to download a file. Can you identify the URL of this file downloaded?

I filtered the packets by combining the attacker's IP address with the HTTP protocol. The first line shows the first file being downloaded. The answer is `http://87.96.21.84/checking.ps1`.

<img width="1920" height="543" alt="Image" src="https://github.com/user-attachments/assets/f49d06c9-5882-45e6-ab5c-998a6a64d304" />

---

## Question 7. Understanding which group Security Identifier (SID) the malicious script checks to verify the current user's privileges can provide insights into the attacker's intentions. Can you provide the specific Group SID that is being checked?

I followed the TCP Stream of packet 127 and found the content of checking.ps1. This command checks whether the current process is running with Administrator privileges by looking at the system SID `S-1-5-32-544`.

<img width="1895" height="873" alt="Image" src="https://github.com/user-attachments/assets/fe1e6f3b-896c-40b7-bed7-41f893e3906f" />

---

## Question 8. Windows Defender plays a critical role in defending against cyber threats. If an attacker disables it, the system becomes more vulnerable to further attacks. What are the registry keys used by the attacker to disable Windows Defender functionalities? Provide them in the same order found.

In `checking.ps1`, the attacker forced all the following registry keys to a disabled value under `HKLM:\SOFTWARE\Microsoft\Windows Defender`: `DisableAntiSpyware,DisableRoutinelyTakingAction,DisableRealtimeMonitoring,SubmitSamplesConsent,SpynetReporting`.

<img width="1306" height="867" alt="Image" src="https://github.com/user-attachments/assets/80581441-a562-496b-892e-4bfeffcf6b69" />

---

## Question 9. Can you determine the URL of the second file downloaded by the attacker?

In `checking.ps1`, the URL of the second file downloaded by the attacker is `http://87.96.21.84/del.ps1`.

<img width="1122" height="870" alt="Image" src="https://github.com/user-attachments/assets/15c4e69a-c6a5-4529-ba85-bb1fecac191e" />

---

## Question 10. Identifying malicious tasks and understanding how they were used for persistence helps in fortifying defenses against future attacks. What's the full name of the task created by the attacker to maintain persistence?

The function `CleanerEtc` runs only after confirming that the script has Administrator privileges. It downloads a script called del.ps1 from the attacker's server at `87.96.21.84` and saves it as a hidden file in the system directory `C:\ProgramData\del.ps1`. Then it uses `schtasks.exe` to create a scheduled task named `\Microsoft\Windows\MUI\LPupdate`. This name is designed to look like a legitimate Windows language pack update process, so it can avoid detection by users and administrators when they check the task list.

<img width="1917" height="227" alt="Image" src="https://github.com/user-attachments/assets/fa6eb850-f6d4-45e5-9acc-23e3e4ed44f4" />

---

## Question 11. Based on your analysis of the second malicious file, What is the MITRE ID of the main tactic the second file tries to accomplish?

In del.ps1, I can see that it attempts to stop several processes on the system — in particular, processes related to security monitoring. According to the MITRE ATT&CK framework, this activity falls under the tactic Defense Evasion. The relevant MITRE tactic ID is `TA0005`.

<img width="1363" height="588" alt="Image" src="https://github.com/user-attachments/assets/408c269b-5392-4d2d-8ebe-bcffb24e0d47" />

---

## Question 12. What's the invoked PowerShell script used by the attacker for dumping credentials?

I followed the HTTP Stream in packet 4284 and found a PowerShell script named `Invoke-PowerDump.ps1`.

<img width="1618" height="879" alt="Image" src="https://github.com/user-attachments/assets/ddf0ce28-27f3-4b94-8150-2d64ba458794" />

---

## Question 13. Understanding which credentials have been compromised is essential for assessing the extent of the data breach. What's the name of the saved text file containing the dumped credentials?

This command runs the PowerDump tool loaded from `$EncodedCommand`, steals all password hashes from the current machine, and writes them to a hidden file at `C:\ProgramData\hashes.txt`.

<img width="1530" height="831" alt="Image" src="https://github.com/user-attachments/assets/eed5ed4e-f3dd-409b-ab8f-7d53c7989c6e" />
<img width="1916" height="962" alt="Image" src="https://github.com/user-attachments/assets/34b2a188-4ecf-4226-9dc2-93ae93f9e5af" />

---

## Question 14. Knowing the hosts targeted during the attacker's reconnaissance phase, the security team can prioritize their remediation efforts on these specific hosts. What's the name of the text file containing the discovered hosts?

The script `ichigo-lite.ps1` appears to have downloaded a text file named `extracted_hosts.txt` that contains a list of internal IP addresses the attacker wanted to spread to.

<img width="1917" height="887" alt="Image" src="https://github.com/user-attachments/assets/78a72bb4-969b-4532-8d61-8a5589a4d30e" />

I checked the content of `extracted_hosts.txt` and confirmed my guess was correct. The name of the text file containing the discovered hosts is `extracted_hosts.txt`.

<img width="1196" height="437" alt="Image" src="https://github.com/user-attachments/assets/7cf01683-08e8-417e-a28b-efcee718a4c9" />

---

## Question 15. After hash dumping, the attacker attempted to deploy ransomware on the compromised host, spreading it to the rest of the network through previous lateral movement activities using SMB. You're provided with the ransomware sample for further analysis. By performing behavioral analysis, what's the name of the ransom note file?

The function `Download-FileFromURL` downloads a file named javaw.exe from the attacker's server and saves it to `C:\ProgramData\`. This looks like the file we are looking for. I calculated its hash and submitted it to VirusTotal. Under the Behavior tab, I went to Files Opened and found the ransom note name is `# DECRYPT FILES BLUESKY #`.

<img width="1917" height="867" alt="Image" src="https://github.com/user-attachments/assets/54d5227e-7caf-45b9-83fb-b19e7d6e617f" />
<img width="1917" height="876" alt="Image" src="https://github.com/user-attachments/assets/91db1311-ec59-4ccf-898b-2c72ea4e7e90" />

---

## Question 16. In some cases, decryption tools are available for specific ransomware families. Identifying the family name can lead to a potential decryption solution. What's the name of this ransomware family?

The name of this ransomware family is `bluesky`.

<img width="1916" height="877" alt="Image" src="https://github.com/user-attachments/assets/73ba5e78-b8fa-4290-91a9-ce73958bef57" />
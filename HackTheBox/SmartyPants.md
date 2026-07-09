# SmartyPants — HackTheBox Writeup

---

## Question 1. The attacker logged in to the machine where Dutch saves critical files, via RDP on 24th January 2025. Please determine the timestamp of this login.

I used Eric Zimmerman's EvtxECmd tool to convert the Event Logs folder into a CSV, then opened it with Timeline Explorer.

<img width="1604" height="669" alt="Image" src="https://github.com/user-attachments/assets/5928413a-d082-4e93-b33f-64f6a4a3668a" />

I looked into the RemoteConnectionManager event logs that record RDP logon activities, added a filter for the RemoteConnectionManager channel and Event ID 1149 — the answer is `2024-01-24 10:15:14`.

<img width="1907" height="262" alt="Image" src="https://github.com/user-attachments/assets/0f3a5608-1eb5-4594-8075-f9a12c8e530a" />

---

## Question 2. The attacker downloaded a few utilities that aided them for their sabotage and extortion operation. What was the first tool they downloaded and installed?

I used SmartScreen Debug logs for this — they also record execution and file access activities. Added a filter for "SmartScreen" in Timeline Explorer and sorted in ascending order. The activity after msedge was interesting — it showed WinRAR setup was downloaded and installed.

<img width="1917" height="711" alt="Image" src="https://github.com/user-attachments/assets/eeba7af9-8eef-4cc2-a16a-90aff5f23cb5" />

---

## Question 3. They then proceeded to download and then execute the portable version of a tool that could be used to search for files on the machine quickly and efficiently. What was the full path of the executable?

I noticed another executable called Everything.exe executed after the WinRAR activity. Googled it — it's a search and indexing tool that works faster than built-in Windows search. Threat actors use it to quickly find critical files and documents to exfiltrate.

<img width="1917" height="711" alt="Image" src="https://github.com/user-attachments/assets/2f9ed4ce-618e-4b07-8d90-a9323756f92f" />

---

## Question 4. What is the execution time of the tool from task 3?

`2025-01-24 10:17:33`

---

## Question 5. The utility was used to search for critical and confidential documents stored on the host, which the attacker could steal and extort the victim. What was the first document that the attacker got their hands on and breached the confidentiality of that document?

Moving on according to the timeline we have from SmartScreen logs, we notice the path of the document. The first file according to the timeline was the "Ministry of Defense Audit.pdf", so the answer is `C:\Users\Dutch\Documents\2025- Board of directors Documents\Ministry Of Defense Audit.pdf`.

<img width="1912" height="812" alt="Image" src="https://github.com/user-attachments/assets/bc4b512f-6a01-4b18-a3c5-14c776f3e083" />

---

## Question 6. Find the name and path of the second stolen document as well.

Right after the first one, there's another document access event — `C:\Users\Dutch\Documents\2025- Board of directors Documents\2025-BUDGET-ALLOCATIONCONFIDENTIAL.pdf`.

---

## Question 7. The attacker installed a Cloud utility as well to steal and exfiltrate the documents. What is the name of the cloud utility?

MEGAsync is the official desktop app for MEGA cloud storage. I saw evidence of execution of a file called MegaSyncSetup64.exe — so the cloud utility is **MegaSync**.

<img width="902" height="532" alt="Image" src="https://github.com/user-attachments/assets/a25c2c0b-9ee9-4dac-9cac-9409531fea72" />

<img width="1907" height="775" alt="Image" src="https://github.com/user-attachments/assets/cf5ae142-9ab0-410c-b785-765ecc1afbab" />

---

## Question 8. When was this utility executed?

The event from the previous task was just the setup installer, not the tool itself. Moving on chronologically, I see cmd.exe and WinRAR being executed — looks like a staging attempt before exfiltrating with MegaSync. Then I found another Mega entry, this time executing from the AppData folder, meaning it was installed. Execution time: `2025-01-24 10:22:19`.

<img width="1885" height="812" alt="Image" src="https://github.com/user-attachments/assets/a4eaeca8-29f2-49e0-8550-66b3ea2680f7" />

---

## Question 9. The Attacker also proceeded to destroy the data on the host so it is unrecoverable. What utility was used to achieve this?

Quick search on the name — it's a utility to permanently remove data from drives, downloaded from "file_shredder_setup.exe". I also see the tool itself running. The answer is **File Shredder**.

<img width="1917" height="817" alt="Image" src="https://github.com/user-attachments/assets/9e33d6b0-3c99-48ac-b98a-0770715e2f58" />

---

## Question 10. The attacker cleared 2 important logs, thinking they covered all their tracks. When was the security log cleared?

Event ID 1102 is generated when the audit log is cleared. Filtered for Event ID 1102 — the security log was cleared at `2025-01-24 10:28:41`.

<img width="1912" height="737" alt="Image" src="https://github.com/user-attachments/assets/a276d8d6-99eb-4179-81f9-3fef3602db84" />
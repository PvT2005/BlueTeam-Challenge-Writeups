# RomCom — HackTheBox Writeup

---

## Question 1. What is the CVE assigned to the WinRAR vulnerability exploited by the RomCom threat group in 2025?

The answer is `CVE-2025-8088`. This is a Path Traversal vulnerability that allows attackers to write arbitrary files by exploiting the Alternate Data Streams feature of the NTFS file system.

---

## Question 2. What is the nature of this vulnerability?

`A path traversal` vulnerability affecting the Windows version of WinRAR allows the attackers to execute arbitrary code by crafting malicious archive files. This vulnerability was exploited in the wild and was discovered by Anton Cherepanov, Peter Košinár, and Peter Strýček from ESET.

<img width="1835" height="872" alt="Image" src="https://github.com/user-attachments/assets/a1c20ebf-c020-4bcc-8e35-cfbe8cdaf050" />

---

## Question 3. What is the name of the archive file under Susan's documents folder that exploits the vulnerability upon opening the archive file?

I parsed the `$MFT` using MFTECmd and loaded the output into Timeline Explorer. To find the name of the archive file under Susan's documents folder that exploits the vulnerability upon opening, I filtered the Extension column by `.rar` and searched within the Documents directory. The file that Susan opened was `Pathology-Department-Research-Records.rar`.

<img width="864" height="302" alt="Image" src="https://github.com/user-attachments/assets/081d6006-e3aa-4282-b4b0-69eab11def2e" />

---

## Question 4. When was the archive file created on the disk?

Looking at the `Created0x10` column from Question 3, the archive file was created on the disk at `2025-09-02 08:13:50`.

---

## Question 5. When was the archive file opened?

I parsed the `$J` (USN Journal) using MFTECmd and loaded the output into Timeline Explorer. I found the record for the `.lnk` file of the archive file with the Update Reason set to `FileCreate`. The Update Timestamp indicates when the file was created, so the archive file was opened at `2025-09-02 08:14:04`.

<img width="794" height="305" alt="Image" src="https://github.com/user-attachments/assets/732aa67b-7f03-4354-babf-0db35c0e700e" />

---

## Question 6. What is the name of the decoy document extracted from the archive file, meant to appear legitimate and distract the user?

I was able to see this answer from the initial analysis of the MFT file, looking into the contents of the Documents directory. It is the `.pdf` file located below the `.rar` file. As the hint suggests, I verified this by filtering for `FileCreate` and looking at the file creation timestamps. The decoy document is `Genotyping_Results_B57_Positive.pdf`.

<img width="868" height="309" alt="Image" src="https://github.com/user-attachments/assets/f47d0535-177d-4d77-9954-bb582bf67b19" />

---

## Question 7. What is the name and path of the actual backdoor executable dropped by the archive file?

I searched the `$J` and filtered Update Reasons by `FileCreate`, which revealed a suspicious `.exe` file.

<img width="786" height="443" alt="Image" src="https://github.com/user-attachments/assets/4004579b-8a7d-46cd-b434-19a47638f852" />

I then searched the `$MFT` for the filename `ApbxHelper.exe` and found the full path of the actual backdoor executable: `C:\Users\Susan\Appdata\Local\ApbxHelper.exe`.

<img width="795" height="110" alt="Image" src="https://github.com/user-attachments/assets/9710ea47-0f08-43c0-ab39-46933ce8a5f8" />

---

## Question 8. The exploit also drops a file to facilitate the persistence and execution of the backdoor. What is the path and name of this file?

Using the same approach as Question 7, I found that the persistence file is `C:\Users\Susan\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\Display Settings.lnk`.

<img width="860" height="291" alt="Image" src="https://github.com/user-attachments/assets/f6a49453-423b-4d74-9145-cb12e126755a" />

<img width="858" height="119" alt="Image" src="https://github.com/user-attachments/assets/fb5d512d-19f9-45b1-b297-ea79f521daee" />

---

## Question 9. What is the associated MITRE Technique ID discussed in the previous question?

The associated MITRE Technique ID is `T1547.009`, which covers Boot or Logon Autostart Execution via Shortcut Modification — including `.lnk` files and writing shortcuts to the Windows Startup folder.

<img width="1867" height="959" alt="Image" src="https://github.com/user-attachments/assets/cbfc1020-9cb1-4866-8136-fd81ce4a67bb" />

---

## Question 10. When was the decoy document opened by the end user, thinking it to be a legitimate document?

The `.lnk` file is generated when the file is opened. Looking again at the `$MFT` file, I located the highlighted `.pdf` file. Underneath it, the `.lnk` file and its creation date are visible. The decoy document was opened at `2025-09-02 08:15:05`.

<img width="819" height="126" alt="Image" src="https://github.com/user-attachments/assets/40d37f44-5ece-477f-a5d6-34d75fd354b4" />

# CrownJewel-1 — HackTheBox Writeup

---

## Question 1. Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.

I opened the System Event Log and filtered by the keyword `shadow`. In the Details tab under the System dropdown, the UTC time for this event is `2024-05-14 03:42:16`.

<img width="1647" height="936" alt="Image" src="https://github.com/user-attachments/assets/8d40bc70-820a-467c-be73-382984d3524d" />

---

## Question 2. When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the two user groups the volume shadow copy process queries and the machine account that did it.

Event ID 4799 indicates that a local security group membership was enumerated. The VSSVC.exe process checks whether the account that issued the command belongs to the `Administrators, Backup Operators` groups. The Account Name is `DC01$`.

<img width="1646" height="897" alt="Image" src="https://github.com/user-attachments/assets/39658ad2-15b4-421b-8001-a0840965a144" />

<img width="1641" height="907" alt="Image" src="https://github.com/user-attachments/assets/a3b06413-d771-4561-8d92-db4cbae4e55a" />

---

## Question 3. Identify the Process ID (in Decimal) of the volume shadow copy service process.

The answer is `4496`.

<img width="1252" height="900" alt="Image" src="https://github.com/user-attachments/assets/3ffa3165-6ac1-4376-a96d-51ddc1f2d7a1" />

---

## Question 4. Find the assigned Volume ID/GUID value to the Shadow copy snapshot when it was mounted.

I filtered by Event ID 4 and found Device Name: `\Device\HarddiskVolumeShadowCopy1`, which is the virtual disk created by the Volume Shadow Copy. The GUID assigned to the Shadow Copy when it was successfully mounted at 10:44:22 AM is `{06c4a997-cca8-11ed-a90f-000c295644f9}`.

<img width="1297" height="871" alt="Image" src="https://github.com/user-attachments/assets/9e93510a-5dd6-417a-8393-d9bbae356e6e" />

---

## Question 5. Identify the full path of the dumped NTDS database on disk.

I parsed the MFT file using MFTECmd and opened the resulting CSV in Timeline Explorer. I filtered by the incident date (14 May 2024) and searched for `NTDS.dit` in the file name field. The suspicious path for the NTDS.dit file is `C:\Users\Administrator\Documents\backup_sync_Dc\Ntds.dit`.

<img width="862" height="167" alt="Image" src="https://github.com/user-attachments/assets/01efb9ce-8811-426b-971d-7545f6dbbca0" />

---

## Question 6. When was newly dumped ntds.dit created on disk?

`2024-05-14 03:44:22`

---

## Question 7. A registry hive was also dumped alongside the NTDS database. Which registry hive was dumped and what is its file size in bytes?

I filtered the MFT CSV by the Parent Path field, restricting results to the attacker's dump directory: `.\Users\Administrator\Documents\backup_sync_dc`. The registry hive file and its size is `SYSTEM, 17563648`.

<img width="1605" height="89" alt="Image" src="https://github.com/user-attachments/assets/22a374a1-0471-4774-903a-79fb404acb46" />
# Operation Blackout 2025: Smoke & Mirrors — HackTheBox Writeup

---

## Question 1. The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?

The attacker disabled LSA process protection on the Windows system by modifying the registry key `HKLM\SYSTEM\CurrentControlSet\Control\LSA`.

<img width="1545" height="837" alt="Image" src="https://github.com/user-attachments/assets/f3443a8d-b9fa-4456-975c-d7347da8272f" />

---

## Question 2. Which PowerShell command did the attacker first execute to disable Windows Defender?

The attacker used a command to remove all the virus signature databases that Defender had updated over time, forcing it back to its default state. They also ran `Set-MpPreference -DisableIOAVProtection $true -DisableEmailScanning $true -DisableBlockAtFirstSeen $true` to turn off automatic scanning of files downloaded from the Internet, disable inbound/outbound email scanning, and disable the Block at First Sight feature.

<img width="1552" height="897" alt="Image" src="https://github.com/user-attachments/assets/26572733-ffae-42c0-b6d9-9f23f525400d" />
<img width="1166" height="862" alt="Image" src="https://github.com/user-attachments/assets/d0ac6ad1-5411-4273-9a48-d1cba1ab0888" />

---

## Question 3. The attacker loaded an AMSI patch written in PowerShell. Which function in the DLL is being patched by the script to effectively disable AMSI?

The script finds the starting address of the `AmsiScanBuffer` function, then overwrites its first 3 bytes with `0x31, 0xC0, 0xC3`. This means whenever PowerShell calls `AmsiScanBuffer` to scan a script, the function immediately returns a "clean" result before doing any actual scanning.

<img width="1565" height="895" alt="Image" src="https://github.com/user-attachments/assets/e7ecf918-b947-4244-acf3-23e3c8afb609" />
<img width="1018" height="288" alt="Image" src="https://github.com/user-attachments/assets/a39ca23f-006d-4abc-b116-b4d17362ca97" />

---

## Question 4. Which command did the attacker use to restart the machine in Safe Mode?

The attacker used `bcdedit /set safeboot network` to modify the boot configuration of the operating system, forcing Windows to automatically start in Safe Mode with Networking on the next reboot.

<img width="1193" height="866" alt="Image" src="https://github.com/user-attachments/assets/82def9da-0739-4206-9194-bab495621549" />

---

## Question 5. Which PowerShell command did the attacker use to disable PowerShell command history logging?

The PowerShell command the attacker used to disable command history logging is `Set-PSReadlineOption -HistorySaveStyle SaveNothing`.

<img width="1161" height="860" alt="Image" src="https://github.com/user-attachments/assets/a759fcfd-76e6-4d41-a03a-3feb29b23bdd" />
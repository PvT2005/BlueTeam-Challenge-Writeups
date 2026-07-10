# Operation Blackout 2025: Phantom Check — HackTheBox Writeup

---

## Question 1. Which WMI class did the attacker use to retrieve model and manufacturer information for virtualization detection?

One of the commonly used cmdlets for retrieving information via WMI classes in PowerShell is 'Get-WmiObject'. Searching for this in the event logs reveals the use of the 'Win32_ComputerSystem' class to obtain model and manufacturer details.

<img width="1587" height="499" alt="Image" src="https://github.com/user-attachments/assets/774de27d-f3cd-4695-8531-07698f1cd255" />

<img width="1570" height="882" alt="Image" src="https://github.com/user-attachments/assets/7b5ce663-ff04-4abe-a947-c77947f4ce36" />

---

## Question 2. Which WMI query did the attacker execute to retrieve the current temperature value of the machine?

I filtered for Event ID 4103 and searched for the commonly used WMI cmdlet 'Get-WmiObject'. This revealed the query `SELECT * FROM MSAcpi_ThermalZoneTemperature`, which is the WMI query the attacker executed to retrieve the current temperature value of the machine.

<img width="1572" height="777" alt="Image" src="https://github.com/user-attachments/assets/c9aa0f8e-6372-4a16-984d-4a87ca9c5776" />

---

## Question 3. The attacker loaded a PowerShell script to detect virtualization. What is the function name of the script?

Filtering the event logs in 'Windows-PowerShell-Operational' to show all entries with Event ID 4104 reveals the PowerShell scripts that were executed. Upon reviewing the logs, I find that the attacker loaded a virtualization detection script containing the function 'Check-VM'.

<img width="1576" height="877" alt="Image" src="https://github.com/user-attachments/assets/624d9ff0-aed1-4bb4-83c7-489e18917307" />

---

## Question 4. Which registry key did the above script query to retrieve service details for virtualization detection?

Going through the above identified script, I find that the script retrieves service details from 'HKLM:\SYSTEM\ControlSet001\Services'.

<img width="1282" height="850" alt="Image" src="https://github.com/user-attachments/assets/9252aac3-2d67-445a-bcc1-caf2867cfa72" />

---

## Question 5. The VM detection script can also identify VirtualBox. Which processes is it comparing to determine if the system is running VirtualBox?

I scroll down until we come across the 'VirtualBox' comment in the script. Just below it, we can see that the script retrieves process details using the 'Get-Process' cmdlet and compares them with 'vboxservice.exe' and 'vboxtray.exe' to determine whether it's running in a VirtualBox environment.

<img width="1277" height="726" alt="Image" src="https://github.com/user-attachments/assets/364651ed-454e-4057-a35c-fa3ef47ca11a" />

---

## Question 6. The VM detection script prints any detection with the prefix 'This is a'. Which two virtualization platforms did the script detect?

Thoroughly analyzing the script, I do observe that it prints virtual machine detection results using the phrase 'This is a' followed by the name of the VM. To find the output, I search for the string 'This is a' within the 'Microsoft-Windows-PowerShell' logs and confirm that, according to the script, the operating system is running inside either 'Hyper-V' or 'VMware'.

<img width="1257" height="877" alt="Image" src="https://github.com/user-attachments/assets/cc5bb24b-fa61-4c47-afa6-97e9a2b597d7" />
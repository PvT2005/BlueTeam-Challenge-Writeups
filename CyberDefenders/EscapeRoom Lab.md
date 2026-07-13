# EscapeRoom Lab — CyberDefenders Writeup

---

## Question 1. What service did the attacker use to gain access to the system?

I opened the capture file in Wireshark, went to **Statistics → Protocol Hierarchy**, and noticed that most of the traffic is SSH. There are many brute-force connection attempts from different source ports of `23.20.23.147` to port 22 on `10.252.174.188`, so the attacker used **SSH**.

<img width="1222" height="352" alt="Image" src="https://github.com/user-attachments/assets/d9596307-c069-44fb-bab0-cfc2f6880cb0" />
<img width="1890" height="712" alt="Image" src="https://github.com/user-attachments/assets/1ca96341-d88d-4f01-8b53-173eab8e786a" />

---

## Question 2. What attack type was used to gain access to the system?

When we look closer, we see a lot of traffic across the SSH protocol. The attacker used the **brute force** technique.

---

## Question 3. What was the tool the attacker possibly used to perform this attack?

**hydra**

---

## Question 4. How many failed attempts were there?

There are 54 total SSH connection attempts. 2 of them were successful, so there are **52** failed attempts.

<img width="1919" height="891" alt="Image" src="https://github.com/user-attachments/assets/e135e9f0-f91f-41c1-8ca6-96d1b950b25e" />

---

## Question 5. What credentials (username:password) were used to gain access? Refer to shadow.log and sudoers.log.

I used John the Ripper to crack the password hashes from the shadow.log file. The answer is **manager:forgot**.

<img width="1065" height="217" alt="Image" src="https://github.com/user-attachments/assets/e1b2e1e2-e274-43dc-923e-441b983f4920" />

---

## Question 6. What other credentials (username:password) could have been used to gain access also have SUDO privileges? Refer to shadow.log and sudoers.log.

The only other hashes that John cracked were for the user **sean:spectre**.

---

## Question 7. What is the tool used to download malicious files on the system?

I used the `http` filter in Wireshark and found a GET request with a User-Agent of `Wget/1.13.4`. So the tool is **wget**.

<img width="1915" height="932" alt="Image" src="https://github.com/user-attachments/assets/5a2b253e-76d3-48ab-b212-15a86289f804" />

---

## Question 8. How many files the attacker download to perform malware installation?

I went to **File → Export Objects → HTTP**. There are 3 files named `1`, `2`, `3` with the content type `text/html`, while the remaining files are `image/bmp`. So the attacker downloaded **3** files to perform malware installation.

<img width="937" height="552" alt="Image" src="https://github.com/user-attachments/assets/96191622-1868-4fb8-93a6-6139edc60afe" />

---

## Question 9. What is the main malware MD5 hash?

I analyzed all 3 malware samples on VirusTotal. The first file is confirmed as malware, the second file is identified as a rootkit, and the third file is a Bash script that automates the infection — it keeps access and installs the rootkit to hide the attacker's presence. So the first file should be the main malware.

<img width="1346" height="512" alt="Image" src="https://github.com/user-attachments/assets/9d7ca84a-9005-4398-8f74-599ea8caa32d" />
<img width="1343" height="600" alt="Image" src="https://github.com/user-attachments/assets/1bceb679-f11c-4057-9bb0-3f13c7e49016" />
<img width="1451" height="577" alt="Image" src="https://github.com/user-attachments/assets/f5afbe70-5aff-4978-a249-27c1292fff99" />
<img width="962" height="645" alt="Image" src="https://github.com/user-attachments/assets/e72ec4e5-b494-48d0-ab01-bc239abb0879" />

---

## Question 10. What file has the script modified so the malware will start upon reboot?

The script overwrites the content of `/etc/rc.local` to make sure that every time the server restarts, the malware named `mail` will automatically run in the background. It sleeps for 1 second, gets the PID of the `mail` process, and writes it to `/proc/dmesg`.

<img width="955" height="451" alt="Image" src="https://github.com/user-attachments/assets/5f3ae9e4-4180-429f-a4d0-18149b8ef9c0" />

---

## Question 11. Where did the malware keep local files?

The malware was renamed as `mail` and stored in the `/var/mail` directory.

<img width="862" height="472" alt="Image" src="https://github.com/user-attachments/assets/8203ea22-9ab1-4864-a17e-93ae6d6f4e34" />

---

## Question 12. What is missing from ps.log?

The kernel module `sysmod.ko` was programmed to constantly monitor `/proc/dmesg`. When it reads the PID of `mail` written to `/proc/dmesg`, it hooks deep into the operating system to hide that PID. As a result, tools like `ps`, `top`, and `netstat` cannot see the running malware process.

<img width="1686" height="937" alt="Image" src="https://github.com/user-attachments/assets/d0775f4c-6e9d-4c72-ade4-a9f0d1543aaf" />

---

## Question 13. What is the main file that used to remove this information from ps.log?

The attacker downloaded the second file named `2`, renamed it to `sysmod.ko`, then ran `depmod -a` to tell the OS to scan and update the list of dependent modules so the new `sysmod.ko` module is recognized. The attacker also added the module name `sysmod` to `/etc/modules` so it loads automatically every time the kernel starts up. The file is **sysmod.ko**.

<img width="807" height="227" alt="Image" src="https://github.com/user-attachments/assets/ac7dd4c5-9ea2-4e26-a2c7-3ce06654c544" />

---

## Question 14. Inside the Main function, what is the function that causes requests to those servers?

The function that causes requests to those servers is **requestFile**.

<img width="614" height="410" alt="Image" src="https://github.com/user-attachments/assets/596a23dc-548f-440f-92d7-3e742cb379f6" />

---

## Question 15. One of the IP's the malware contacted starts with 17. Provide the full IP.

**174.129.57.253**

<img width="932" height="427" alt="Image" src="https://github.com/user-attachments/assets/58b8cc60-6e51-4f81-bd90-35affeb5ffcd" />

---

## Question 16. How many files the malware requested from external servers?

**9**

<img width="917" height="477" alt="Image" src="https://github.com/user-attachments/assets/dab05673-4008-4d36-b27e-0dabc9e36ccd" />

---

## Question 17. What are the commands that the malware was receiving from attacker servers?

The `processMessage` function checks the content of `param_1` using `if` and `else if` conditions. It compares the first 4 bytes of data with `NOP` and `RUN`. `NOP` is used to keep the connection alive, and `RUN` tells the malware to execute a file or system command that is passed along with it.

<img width="1449" height="592" alt="Image" src="https://github.com/user-attachments/assets/369a3e99-12f3-4199-a035-271e8bbdf41b" />
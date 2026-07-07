# Network Analysis - Ransomware — Blue Team Labs Writeup

---

## Question 1. What is the operating system of the host from which the network traffic was captured?

To get the details of the host machine, I went to **Statistics → Capture File Properties** in Wireshark:

<img width="742" height="127" alt="Image" src="https://github.com/user-attachments/assets/f6818db7-3d05-4cc8-a4df-a5b5a7134009" />

---

## Question 2. What is the full URL from which the ransomware executable was downloaded?

I used the filter `http.request` to find HTTP requests — this shows which URL the executable was downloaded from. The URL is `http://10.0.2.15:8000/safecrypt.exe`.

<img width="1911" height="527" alt="Image" src="https://github.com/user-attachments/assets/cd41dbc4-4528-4f52-b484-38fc43a68307" />

---

## Question 3. Name the ransomware executable file?

`safecrypt.exe`

---

## Question 4. What is the MD5 hash of the ransomware?

`4a1d88603b1007825a9c6b36d1e5de44`

---

## Question 5. What is the name of the ransomware?

I uploaded the hash to VirusTotal and got the result — **teslacrypt**:

<img width="1577" height="570" alt="Image" src="https://github.com/user-attachments/assets/85915749-9297-45e7-bdbf-2fae2af450d0" />

---

## Question 6. What is the encryption algorithm used by the ransomware, according to the ransom note?

According to the ransom note, the encryption algorithm is **RSA-4096**:

<img width="1367" height="131" alt="Image" src="https://github.com/user-attachments/assets/dd892981-d920-43ac-8ae9-005e5a25767b" />

---

## Question 7. What is the domain beginning with 'd' that is related to ransomware traffic?

In the Relations tab on VirusTotal, the domain is `dunyamuzelerimuzesi.com`:

<img width="1367" height="617" alt="Image" src="https://github.com/user-attachments/assets/56190bf3-c8da-4856-9f4b-6cc82c648094" />

---

## Question 8. Decrypt the Tender document and submit the flag

I looked for tools that can decrypt TeslaCrypt and found **TeslaCrypt Decryption Tool by TrendMicro**. I loaded the "Tender document" file into the tool, ran the decryption, and got the flag:

<img width="924" height="400" alt="Image" src="https://github.com/user-attachments/assets/f8550393-339a-4465-a7d9-a17d4d1b7d61" />
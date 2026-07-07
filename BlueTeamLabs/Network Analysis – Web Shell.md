# Network Analysis – Web Shell — Blue Team Labs Writeup

---

## Question 1. What is the IP responsible for conducting the port scan activity?

I can see that many machines are being scanned by `10.251.96.4`:

<img width="1690" height="511" alt="Image" src="https://github.com/user-attachments/assets/ec77341a-b925-41c9-b584-3000cde8715c" />

<img width="1692" height="617" alt="Image" src="https://github.com/user-attachments/assets/174deca1-3e89-4db1-9aff-a8ea92ba706c" />

---

## Question 2. What is the port range scanned by the suspicious host?

I sorted by TCP and found that the scanned ports (port B) are in the range **1–1024**:

<img width="1812" height="616" alt="Image" src="https://github.com/user-attachments/assets/3b0f0488-8fd7-45ab-8643-3af257017383" />

<img width="1800" height="602" alt="Image" src="https://github.com/user-attachments/assets/1e50e165-15ec-4894-82c6-fac2bdbc4344" />

---

## Question 3. What is the type of port scan conducted?

I filtered the TCP packets and saw a pattern — the attacker sends a SYN, gets a SYN-ACK back, then resets. This is **TCP SYN** scan.

---

## Question 4. Two more tools were used to perform reconnaissance against open ports, what were they?

I used the `http` filter and looked at the GET requests. The User-Agent shows two tools: **gobuster 3.0.1** and **sqlmap 1.4.7**.

<img width="1581" height="566" alt="Image" src="https://github.com/user-attachments/assets/37e20cbe-b6fb-4495-bf68-b6fa6749e174" />

<img width="1402" height="387" alt="Image" src="https://github.com/user-attachments/assets/596bd604-6eac-44d6-84ed-d3949dbb0388" />

---

## Question 5. What is the name of the php file through which the attacker uploaded a web shell?

The attacker used `editprofile.php` to upload the web shell:

<img width="1200" height="697" alt="Image" src="https://github.com/user-attachments/assets/44c3ba09-333f-4190-b31f-94c5bac01a14" />

---

## Question 6. What is the name of the web shell that the attacker uploaded?

I looked at the POST request to `upload.php` and found the web shell file name is `dbfunctions.php`:

<img width="1872" height="931" alt="Image" src="https://github.com/user-attachments/assets/d2de4040-cfd6-48e1-99e0-bf9142ed81b2" />

<img width="1129" height="860" alt="Image" src="https://github.com/user-attachments/assets/0fd24617-a327-44dd-9dec-403c8d4487fd" />

---

## Question 7. What is the parameter used in the web shell for executing commands?

I checked the PHP code in the web shell — the parameter is `cmd`:

<img width="1077" height="712" alt="Image" src="https://github.com/user-attachments/assets/29e4a28b-c8b5-49bc-8ae3-da9f4fa3901f" />

---

## Question 8. What is the first command executed by the attacker?

I need to find the commands sent to the `cmd` parameter of `dbfunctions.php` — the first command is `id`:

<img width="1917" height="457" alt="Image" src="https://github.com/user-attachments/assets/685a99ce-5065-4214-a81b-890cd01d63fb" />

---

## Question 9. What is the type of shell connection the attacker obtains through command execution?

The command tells the server to open a **reverse shell** back to `10.251.96.4` on port `4422`:

<img width="1883" height="868" alt="Image" src="https://github.com/user-attachments/assets/e11b633e-465f-4691-bdb5-a10b52877bd2" />

---

## Question 10. What is the port he uses for the shell connection?

`4422`

<img width="1532" height="593" alt="Image" src="https://github.com/user-attachments/assets/c73b1516-d09c-4dde-898e-3aa4092ef950" />
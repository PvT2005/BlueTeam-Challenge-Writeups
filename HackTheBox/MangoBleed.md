# MangoBleed — HackTheBox Writeup
## Question 1. BWhat is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

I briefed about the scenario that the incident is related to a MongoDB heap memory disclosure
vulnerability called MongoBleed so answer is CVE-2025-14847
<img width="1066" height="605" alt="Image" src="https://github.com/user-attachments/assets/ac302b41-1a5c-4135-a069-c40c4e844ad4" />

## Question 1. What is the version of MongoDB installed on the server that the CVE exploited?
 Analyze the MongoDB log file located at /var/log/mongodb, I see the version of MongoDB is 8.0.16
<img width="1917" height="540" alt="Image" src="https://github.com/user-attachments/assets/fe6d10d5-0f96-4a90-8788-8af3008a9a3e" />

## Question 1. Analyze the MongoDB logs to identify the attacker's remote IP address used to exploit the CVE.
Tìm kiếm với từ khóa `Connection` và ta tìm thấy rất nhiều nỗ lực kết nối từ địa chỉ 65.0.76.43 qua rất nhiều cổng
<img width="1895" height="941" alt="Image" src="https://github.com/user-attachments/assets/b30b48d5-7161-493f-956c-67794bdbb80e" />

## Question 1. Based on the MongoDB logs, determine the exact date and time the attacker’s exploitation activity began (the earliest confirmed malicious event)
Ta xác định được thời điểm attacker’s exploitation activity began nỗ lực kết nối đến máy chủ MongoDB là 2025-12-29 05:25:52 
<img width="1892" height="937" alt="Image" src="https://github.com/user-attachments/assets/90b2f029-7fe5-4911-909d-ca0daaed130f" />

## Question 1. Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.
75260

## Question 1. The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?


## Question 1. Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.


## Question 1. The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?


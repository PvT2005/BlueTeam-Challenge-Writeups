# EscapeRoom Lab — Blue Team Labs Writeup
## Question 1. What service did the attacker use to gain access to the system?
view statistics, and open protocol hierarchy statistics. We See More Traffic Through the SSH Protocol.Rất nhiều nỗ lực bruteforce kết nối ssh từ nhiều cổng khác nhau của địa chỉ 23.20.23.147 đến port 22 của 10.252.174.188 
<img width="1222" height="352" alt="Image" src="https://github.com/user-attachments/assets/d9596307-c069-44fb-bab0-cfc2f6880cb0" />
<img width="1890" height="712" alt="Image" src="https://github.com/user-attachments/assets/1ca96341-d88d-4f01-8b53-173eab8e786a" />

## Question 2. What attack type was used to gain access to the system?
When we investigate, we see a lot of traffic across the SSH protocol. The attacker uses the brute force technique.

## Question 3. What was the tool the attacker possibly used to perform this attack?
hydra

## Question 4. How many failed attempts were there?
54 total number of attempts 2 were successful so 52 failed attempts.
<img width="1919" height="891" alt="Image" src="https://github.com/user-attachments/assets/e135e9f0-f91f-41c1-8ca6-96d1b950b25e" />

## Question 5. What credentials (username:password) were used to gain access? Refer to shadow.log and sudoers.log.


## Question 6. What other credentials (username:password) could have been used to gain access also have SUDO privileges? Refer to shadow.log and sudoers.log.
The only other hashes cracked were for the user sean:spectre
## Question 7. What is the tool used to download malicious files on the system?
dùng filter `http` ta thấy được GET request có user agent là Wget/1.13.4  vì thế wget is tool.
<img width="1915" height="932" alt="Image" src="https://github.com/user-attachments/assets/5a2b253e-76d3-48ab-b212-15a86289f804" />

## Question 8. How many files the attacker download to perform malware installation?
`Export` --> `HTTP` ta có thấy có 3 file 1,2,3 có định dạng text/html và còn lại là định dạng image/bmp so the attacker download 3 files to perform malware installation.
<img width="937" height="552" alt="Image" src="https://github.com/user-attachments/assets/96191622-1868-4fb8-93a6-6139edc60afe" />


## Question 9. What is the main malware MD5 hash?
analyze the 3 malware samples in virustotal we can see first file is confirmed to be a malware while second file identified as a rootkit on VirusTotal, file thứ 3 là đoạn Bash script tự động hóa để lây nhiễm, duy trì quyền truy cập và cấy ghép Rootkit nhằm che giấu hoàn toàn sự hiện diện then the first should be main file and rootkit purpose is still unknown
<img width="1346" height="512" alt="Image" src="https://github.com/user-attachments/assets/9d7ca84a-9005-4398-8f74-599ea8caa32d" />
<img width="1343" height="600" alt="Image" src="https://github.com/user-attachments/assets/1bceb679-f11c-4057-9bb0-3f13c7e49016" />
<img width="1451" height="577" alt="Image" src="https://github.com/user-attachments/assets/f5afbe70-5aff-4978-a249-27c1292fff99" />
<img width="962" height="645" alt="Image" src="https://github.com/user-attachments/assets/e72ec4e5-b494-48d0-ab01-bc239abb0879" />

## Question 10. What file has the script modified so the malware will start upon reboot?
ghi đè nội dung vào file /etc/rc.local và đảm bảo rằng mỗi khi máy chủ khởi động lại:Mã độc mail sẽ tự động chạy ngầm, sleep 1, lấy PID của tiến trình mail và ghi vào /proc/dmesg.
<img width="955" height="451" alt="Image" src="https://github.com/user-attachments/assets/5f3ae9e4-4180-429f-a4d0-18149b8ef9c0" />

## Question 11. Where did the malware keep local files?
The malware was renamed as ‘mail’ in the /var/mail directory.
<img width="862" height="472" alt="Image" src="https://github.com/user-attachments/assets/8203ea22-9ab1-4864-a17e-93ae6d6f4e34" />

## Question 12. What is missing from ps.log?
sysmod.ko vừa được nạp ở trên đã được lập trình để liên tục theo dõi /proc/dmesg. Khi nó đọc được PID của mail được ghi vào /proc/dmesg, nó sẽ can thiệp sâu vào hệ điều hành để che giấu PID này. Kết quả là các công cụ quản lý như ps, top, hay netstat sẽ không thể nhìn thấy tiến trình mã độc đang chạy.
<img width="1686" height="937" alt="Image" src="https://github.com/user-attachments/assets/d0775f4c-6e9d-4c72-ade4-a9f0d1543aaf" />

## Question 13. What is the main file that used to remove this information from ps.log?
Kẻ tấn công lấy file thứ hai tên là 2, đổi tên file này thành sysmod.ko, epmod -a  yêu cầu hệ điều hành quét lại và cập nhật danh sách các module phụ thuộc giúp module sysmod.ko mới được thêm vào, ghi thêm tên module sysmod vào file /etc/modules để tự động nạp mỗi khi Kernel khởi động. The file is sysmod.ko.
<img width="807" height="227" alt="Image" src="https://github.com/user-attachments/assets/ac7dd4c5-9ea2-4e26-a2c7-3ce06654c544" />


## Question 14. Inside the Main function, what is the function that causes requests to those servers?


## Question 15. One of the IP's the malware contacted starts with 17. Provide the full IP.
174.129.57.253
<img width="932" height="427" alt="Image" src="https://github.com/user-attachments/assets/58b8cc60-6e51-4f81-bd90-35affeb5ffcd" />

## Question 16. How many files the malware requested from external servers?
9
<img width="917" height="477" alt="Image" src="https://github.com/user-attachments/assets/dab05673-4008-4d36-b27e-0dabc9e36ccd" />

## Question 17. What are the commands that the malware was receiving from attacker servers? 
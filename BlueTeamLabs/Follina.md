# Follina — Blue Team Labs Writeup

---

## Question 1. What is the SHA1 hash value of the sample?

First step — upload the doc to VirusTotal:

<img width="1907" height="731" alt="Image" src="https://github.com/user-attachments/assets/b1d23a10-5f5a-4f06-b115-355e948d4703" />

SHA1 hash is right here in the Details tab:

<img width="1421" height="495" alt="Image" src="https://github.com/user-attachments/assets/e1ea5e47-50a7-4dc4-af56-a3ad2479898d" />

---

## Question 2. According to VirusTotal, what is the full filetype of the provided sample?

Same Details tab on VirusTotal, scroll down a bit:

<img width="1421" height="495" alt="Image" src="https://github.com/user-attachments/assets/80a17bc4-4c96-49ce-ba13-3086d75668a9" />

---

## Question 3. Extract the URL that is used within the sample and submit it

`.docx` is just a ZIP file so I used `unzip` to extract everything:

<img width="951" height="299" alt="Image" src="https://github.com/user-attachments/assets/1da474d8-5a32-4f0b-8b09-6d290d55fe6a" />

So `.rels` files basically define relationships between the document and external/internal resources. Attackers abuse this for **Template Injection** — they modify the `.rels` file, stick a malicious URL in the `Target` attribute, and when the victim opens the Word file it automatically fetches and executes whatever is at that URL.

Checked `word/_rels/document.xml.rels` and found the malicious target:

```
https[:]//www.xmlformats.com/office/word/2022/wordprocessingDrawing/RDF842l[.]html
```

<img width="1264" height="237" alt="Image" src="https://github.com/user-attachments/assets/98201d1b-d1f7-40d9-9c1d-3082609964e6" />

---

## Question 4. What is the name of the XML file that is storing the extracted URL? 

Got this from Q3 already — it's `document.xml.rels`.

---

## Question 5. The extracted URL accesses an HTML file that triggers the vulnerability to execute a malicious payload. According to the HTML processing functions, any files with fewer than `<Number>` bytes would not invoke the payload. Submit the `<Number>`

There's a minimum file size check — if the HTML file is too small, the payload won't run:

<img width="906" height="232" alt="Image" src="https://github.com/user-attachments/assets/d902b8b2-49ce-4361-b2ea-9b3de57c28e1" />

---

## Question 6. After execution, the sample will try to kill a process if it is already running. What is the name of this process?

Found [this blog post from Keysight](https://www.keysight.com/blogs/en/tech/nwvs/2022/06/07/keysights-take-on-cve-2022-30190-msdt-follina-exploit) that explains the whole chain pretty well:

<img width="1272" height="575" alt="Image" src="https://github.com/user-attachments/assets/f2288adb-71ee-4dcb-8a5c-0d32ff79af8f" />

---

## Question 7. You were asked to write a process-based detection rule using Windows Event ID 4688. What would be the ProcessName and ParentProcessName used in this detection rule?

Looking at the attack flow:

- ProcessName: `msdt.exe`
- ParentProcessName: `winword.exe`

Makes sense — Word spawns MSDT through the exploit.

<img width="1562" height="662" alt="Image" src="https://github.com/user-attachments/assets/ce588651-bce3-49d0-a3f3-79e5d41bab8d" />

---

## Question 8. Submit the MITRE technique ID used by the sample for Execution

In VirusTotal go to **Behavior → MITRE ATT&CK Tactics and Techniques → Execution**:

<img width="1631" height="542" alt="Image" src="https://github.com/user-attachments/assets/88ebab00-b81b-4915-822f-d1d6355ab190" />

---

## Question 9. Submit the CVE associated with the vulnerability that is being exploited

<img width="1187" height="580" alt="Image" src="https://github.com/user-attachments/assets/b03b4a27-648e-4d89-b564-1e2411ec3421" />
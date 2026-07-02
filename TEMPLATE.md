# [Challenge Name]

> **Platform:** [CyberDefenders / LetsDefend / Blue Team Labs Online]  
> **Category:** [Phishing / Memory Forensics / Network Forensics / Incident Response / Endpoint Forensics]  
> **Difficulty:** [Easy / Medium / Hard]  
> **Date:** YYYY-MM-DD  

---

## 📋 Scenario

<!-- Paste the challenge description here. What are we investigating? -->

---

## 🛠️ Tools Used

- Tool 1 (purpose)
- Tool 2 (purpose)

---

## 🔍 Investigation

### Step 1: Initial Triage

<!-- First look — what do we see? What stands out? -->

### Step 2: Evidence Analysis

<!-- Deep dive into the evidence. Show your work with screenshots. -->

<!-- ![Description](screenshots/screenshot_01.png) -->

### Step 3: IOC Extraction

<!-- Extract and defang all indicators of compromise. -->

| IOC Type | Value | Context |
|---|---|---|
| IP | `x.x.x[.]x` | Source of attack |
| Domain | `evil[.]com` | Phishing domain |
| SHA256 | `abc123...` | Malicious attachment |
| URL | `hxxp[:]//evil[.]com/login` | Credential harvesting page |

### Step 4: Threat Intelligence Enrichment

<!-- Query IOCs against VirusTotal, AbuseIPDB, URLScan.io, etc. -->
<!-- Include screenshots of results. -->

---

## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique Name | Evidence |
|---|---|---|---|
| Initial Access | T1566.001 | Phishing: Spearphishing Attachment | Malicious .docm sent via email |
| Execution | T1204.002 | User Execution: Malicious File | User opened attachment |

---

## 📊 Timeline

<!-- Reconstruct the timeline of events. -->

| Time (UTC) | Event | Source |
|---|---|---|
| HH:MM:SS | Event description | Log source |

---

## ✅ Verdict

**Classification:** `True Positive` / `False Positive`  
**Severity:** `Low` / `Medium` / `High` / `Critical`

**Summary:**  
<!-- 2-3 sentences explaining what happened. -->

---

## 💡 Recommendations

1. <!-- Action item 1 -->
2. <!-- Action item 2 -->
3. <!-- Action item 3 -->

---

## 📝 Lessons Learned

<!-- What did you learn from this challenge? Any new tools, techniques, or concepts? -->

---

## 🏁 Challenge Answers

<!-- If the platform has specific questions, list your answers here. -->
<!-- Be careful: some platforms prohibit sharing answers publicly. Check their policy first. -->

<details>
<summary>Click to reveal answers (spoiler)</summary>

| # | Question | Answer |
|---|---|---|
| Q1 | — | — |
| Q2 | — | — |

</details>

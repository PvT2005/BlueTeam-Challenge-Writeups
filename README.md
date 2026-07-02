# 🛡️ Blue Team Challenge Writeups

A collection of detailed writeups from Blue Team challenges and SOC analyst exercises. Each writeup documents the full investigation process — from initial alert triage to verdict and remediation — following a structured SOC analyst workflow.

## 🎯 Purpose

- Practice and demonstrate **SOC Tier 1 analyst skills**: alert triage, log analysis, phishing investigation, malware analysis, and incident response.
- Document findings using a **repeatable, professional methodology** mapped to the MITRE ATT&CK framework.
- Build a portfolio that showcases hands-on defensive security capabilities.

## 📂 Platforms

| Platform | Focus Area | Directory |
|---|---|---|
| [CyberDefenders](https://cyberdefenders.org/) | Memory Forensics, Network Forensics, Endpoint Forensics | [`CyberDefenders/`](CyberDefenders/) |
| [LetsDefend](https://letsdefend.io/) | SOC Alert Triage, Phishing Analysis, Incident Response | [`LetsDefend/`](LetsDefend/) |
| [Blue Team Labs Online](https://blueteamlabs.online/) | Investigation, Incident Response, Reverse Engineering | [`BlueTeamLabs/`](BlueTeamLabs/) |

## 🔬 Methodology

Every writeup follows this standard investigation workflow:

```
1. Alert / Challenge Overview
   └── What are we investigating?

2. Evidence Collection
   └── Gather artifacts: logs, PCAPs, memory dumps, emails

3. Analysis
   ├── Header / Metadata inspection
   ├── IOC extraction (IPs, domains, hashes, URLs)
   ├── Threat intelligence enrichment (VirusTotal, AbuseIPDB, etc.)
   └── Timeline reconstruction

4. Findings
   ├── MITRE ATT&CK mapping
   └── IOC summary table

5. Verdict & Recommendations
   └── True Positive / False Positive + remediation steps
```

## 🛠️ Tools Used

| Category | Tools |
|---|---|
| SIEM & Log Analysis | Splunk, Wazuh, ELK |
| Network Analysis | Wireshark, NetworkMiner, Zeek |
| Memory Forensics | Volatility 3, Strings |
| Threat Intelligence | VirusTotal, AbuseIPDB, URLScan.io, Cisco Talos |
| Email Analysis | MXToolbox, CyberChef, PhishTool |
| Malware Analysis | ANY.RUN, Hybrid Analysis, PE Studio |
| Utilities | CyberChef, WHOIS, DNS lookup |

## 📊 Progress Tracker

### CyberDefenders
| # | Challenge | Category | Difficulty | Status |
|---|---|---|---|---|
| 1 | — | — | — | ⬜ Not Started |

### LetsDefend
| # | Alert ID | Category | Difficulty | Status |
|---|---|---|---|---|
| 1 | — | — | — | ⬜ Not Started |

### Blue Team Labs Online
| # | Challenge | Category | Difficulty | Status |
|---|---|---|---|---|
| 1 | — | — | — | ⬜ Not Started |

> **Status Legend:** ⬜ Not Started · 🔄 In Progress · ✅ Completed

## ⚠️ Disclaimer

All analysis is performed in isolated virtual environments. Samples and IOCs are **defanged** for safety. These writeups are for educational purposes only.

## 📬 Contact

- **GitHub:** [PvT2005](https://github.com/PvT2005)
- **Email:** pvt1772005@gmail.com

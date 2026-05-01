# 🛡️ Network Attack Detection Lab (SOC Mini Project)

> A hands-on SOC analyst simulation — detecting, capturing, and documenting a full cyber kill chain attack in a containerised lab environment.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Tools](https://img.shields.io/badge/tools-Wireshark%20%7C%20tcpdump%20%7C%20Nmap%20%7C%20Hydra-blue)

---

## 📸 Overview

Built a mini Security Operations Centre (SOC) environment using Docker containers. Captured network traffic with Wireshark and tcpdump, then documented a full cyber kill chain attack.

- ✅ Deployed Kali Linux attacker and DVWA target containers
- ✅ Established network traffic baseline (mDNS, ARP)
- ✅ Captured and analysed baseline traffic (mDNS/ARP) — SOC‑001
- ✅ Simulated 6 attacks across the full Cyber Kill Chain
- ✅ Documented 7 security findings in professional SOC report format
- ✅ Mapped all attacks to MITRE ATT&CK framework

---

## ⚔️ Attacks Simulated

| # | Attack | Tool | Severity | MITRE ATT&CK |
|---|--------|------|----------|--------------|
| 0 | **Baseline Traffic (mDNS)** | Wireshark | Info | — |
| 1 | Reconnaissance | Nmap 7.99 | Medium | T1046 |
| 2 | Information Disclosure | Browser | Medium | T1082 |
| 3 | Brute Force | Hydra v9.6 | High | T1110 |
| 4 | SQL Injection | Manual | Critical | T1190 |
| 5 | Command Injection (RCE) | Manual | Critical | T1059 |
| 6 | Webshell Upload | PHP | Critical | T1505.003 |

---

## 🔍 Key Findings

### 🔴 CRITICAL — Full System Compromise
- Dumped all database users via SQL Injection
- Achieved Remote Code Execution via Command Injection
- Uploaded PHP webshell for persistent access
- Exfiltrated /etc/passwd file

### 🟡 MEDIUM — Reconnaissance
- Nmap discovered Apache 2.4.25 on Debian
- DVWA setup page leaked PHP version, MySQL credentials

### 🔴 HIGH — Weak Authentication
- Hydra cracked password via dictionary attack
- No account lockout or rate limiting detected

---

## 🛠️ Tools Used

| Category | Tools |
|----------|-------|
| Virtualisation | Docker Desktop, WSL 2 |
| Attack OS | Kali Linux |
| Target | DVWA |
| Packet Capture | Wireshark, tcpdump |
| Attack Tools | Nmap, Hydra, curl, PHP webshell |
| Analysis | MITRE ATT&CK, Cyber Kill Chain |

---

## 📚 Skills Demonstrated

- Network Traffic Analysis (Wireshark, tcpdump)
- SOC Triage (baseline vs attack traffic)
- Incident Documentation (SOC report writing)
- Offensive Security Basics (SQLi, RCE, Brute Force)
- Threat Mapping (MITRE ATT&CK, Cyber Kill Chain)
- Docker Container Networking

---

## 🔵 Blue Team / SOC Detection Notes

> How a defender would spot each attack — the thinking behind the triage.

| Attack | Detection Method |
|--------|------------------|
| Nmap Scan | Alert on >50 SYN packets from a single source in <5 seconds (simple threshold rule). Monitor for sequential port sweeps in firewall logs. |
| Info Disclosure | Monitor web server access logs for hits to /setup.php, phpinfo.php, or paths containing .env. These pages should never be accessed in production. |
| Brute Force | SIEM rule: >5 failed POST requests to /login.php from the same IP within 10 seconds. Track the ratio of 302 Found vs 200 OK responses on the login endpoint. |
| SQL Injection | WAF signature detection on SQL keywords in HTTP parameters. Monitor database error logs for syntax errors indicating injection attempts. |
| Command Injection | Detect shell metacharacters (semicolons, pipes, backticks, dollar signs) in HTTP request body. Alert on web server spawning child processes like whoami or cat. |
| Webshell Upload | File integrity monitoring on upload directories. Flag any .php file created inside /uploads/. Monitor for GET requests to suspicious paths with parameters such as ?cmd= or ?exec=. |

**Key SOC Insight:** Baseline traffic (mDNS, ARP, DHCP) makes up most network noise. A strong analyst establishes "normal" first, then hunts for deviations — exactly what SOC‑001 captures in this lab.

---

## 📁 Evidence Inventory

| File | Finding | Description |
|------|---------|-------------|
| `01-docker-containers.png` | Lab Setup | Docker Desktop running both containers |
| `02-dvwa-setup-page.png` | SOC-002a | Information disclosure — PHP version, MySQL creds |
| `04-sqli-dump.png` | SOC-004 | SQL Injection — 5 users dumped |
| `09-sqli-browser-success.png` | SOC-004 | SQL Injection in browser |
| `10-command-injection-whoami.png` | SOC-005 | Command Injection — www-data output |
| `11-webshell-directory-listing.png` | SOC-006 | Directory listing showing shell.php |
| `12-webshell-passwd-dump.png` | SOC-006 | /etc/passwd exfiltrated via webshell |
| `14-wireshark-webshell-sqli.png` | SOC-004/006 | Wireshark capture of SQLi + webshell |
| `captures/dvwa-attacks.pcapng` | All Findings | Full Wireshark packet capture — SQLi, command injection, webshell traffic |

Full set in [evidence/](evidence/)

---

## 🚀 Quick Start (Reproduce This Lab)

```bash
git clone https://github.com/laiba1305/soc-network-attack-lab.git
cd soc-network-attack-lab

# Start DVWA (host access at localhost:8080, container port 80 at 172.17.0.2)
docker run -d -p 8080:80 --name dvwa-target vulnerables/web-dvwa

# Start Kali attacker
docker run -it --name kali-attacker kalilinux/kali-rolling bash

# Inside Kali
apt update && apt install -y nmap hydra curl tcpdump

# Attack from Kali to DVWA's container IP
nmap -sV 172.17.0.2
Note: All attacks target the container IP 172.17.0.2:80 directly, simulating an internal network attack. The host mapping localhost:8080 is for browser access only.

See scripts/attacks.sh for the complete attack sequence.

📄 Full Report
The complete SOC analyst report with detailed findings, IoCs, and remediation roadmap:
📄 SOC_Report_Laiba_Rana.docx

⚠️ Disclaimer
For educational purposes only. This project demonstrates attacks in a controlled, isolated lab environment. Do not use these techniques against systems you do not own or have explicit permission to test.

⭐ If this project helped you, give it a star!
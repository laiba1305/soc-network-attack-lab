# 🛡️ Network Attack Detection Lab (SOC Mini Project)

> A hands-on SOC analyst simulation — detecting, capturing, and documenting a full cyber kill chain attack in a containerised lab environment.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Tools](https://img.shields.io/badge/tools-Wireshark%20%7C%20tcpdump%20%7C%20Nmap%20%7C%20Hydra-blue)

---

## 📸 Overview

Built a mini Security Operations Centre (SOC) environment using Docker containers. Captured network traffic with Wireshark and tcpdump, then documented a full cyber kill chain attack.

- ✅ Deployed Kali Linux attacker and DVWA target containers
- ✅ Established network traffic baseline (mDNS, ARP)
- ✅ Simulated complete 7-stage cyber kill chain
- ✅ Captured packets in Wireshark & tcpdump
- ✅ Documented 6 security findings in SOC report
- ✅ Mapped all attacks to MITRE ATT&CK framework

---

## ⚔️ Attacks Simulated

| # | Attack | Tool | Severity | MITRE ATT&CK |
|---|--------|------|----------|--------------|
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

## 🚀 Quick Start

```bash
git clone https://github.com/laiba1305/soc-network-attack-lab.git
cd soc-network-attack-lab
docker run -d -p 8080:80 --name dvwa-target vulnerables/web-dvwa
docker run -it --name kali-attacker kalilinux/kali-rolling bash
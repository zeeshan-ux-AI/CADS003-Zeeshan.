# Applied Penetration Testing & Web Reconnaissance Labs

<p align="center">
  <img src="https://img.shields.io/badge/OS-Kali%20Linux-blue?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Kali Linux">
  <img src="https://img.shields.io/badge/Tools-Metasploit%20%7C%20Nmap%20%7C%20Wireshark-orange?style=for-the-badge" alt="Tools">
  <img src="https://img.shields.io/badge/Focus-Penetration%20Testing-red?style=for-the-badge" alt="Focus">
</p>

## 📌 Project Overview
This repository contains a comprehensive collection of hands-on cybersecurity lab exercises focused on **Web Application Reconnaissance**, **Network Discovery**, and **Penetration Testing Techniques**. The primary objective is to demonstrate practical knowledge of information gathering, vulnerability assessment, network pivoting, and traffic analysis using industry-standard security tools in a controlled environment.

---

## 🛠️ Tools & Technologies Used

| Category | Tools & Technologies |
| :--- | :--- |
| **Operating System** | Kali Linux |
| **Reconnaissance & OSINT** | `whois`, `dig`, `nslookup`, `whatweb`, `theHarvester` |
| **WAF Fingerprinting** | `wafw00f` |
| **Network Scanning & Pivoting** | `Nmap`, `Metasploit Framework (MSF6)` |
| **Brute-Forcing & Cracking** | `Hydra`, `John the Ripper` |
| **Network Analysis** | `Wireshark` (Traffic Inspection & Packet Analysis) |

---

## 🚀 Lab Exercises & Core Commands

### 1. Web Application Reconnaissance
Conducted passive and active reconnaissance against target domains (`vocabulary.com` and `crazygames.com`) to extract infrastructure details.

* **Domain WHOIS Lookup:**
  ```bash
  whois vocabulary.com

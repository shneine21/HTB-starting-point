# HTB-starting-point
Documentation and technical writeups for Hack The Box labs
# Hack The Box - Starting Point (Tier 0) Writeups

This repository contains concise writeups and technical notes for completing the introductory labs in Tier 0 of the Hack The Box (HTB) Starting Point track. The objective of these labs is to build foundational skills in network reconnaissance, configuration analysis, and the exploitation of unauthenticated protocols.

---
## 🐱 1. Machine: Meow (Topic: Unauthenticated Telnet Access)
* **Objective:** Understand basic port scanning and exploit insecure remote administration configurations.
* **Tools Used:** `nmap`, `telnet`
### Methodology:
1. **Reconnaissance:** Performed a service version scan on the target IP using Nmap:
   ```bash
   sudo nmap -sV <TARGET_IP>
   ```
   *The scan revealed that port **23/tcp (Telnet)** was open.*
2. **Vulnerability Analysis:** The Telnet protocol transmits data in plaintext and is notorious for weak or default credential issues. I initiated a connection to check the login prompt:
   ```bash
   telnet <TARGET_IP>
   ```
3. **Exploitation:** The target server suffered from a severe misconfiguration: the administrative root account (`root`) had a blank password. I bypassed authentication successfully by typing `root` and hitting Enter without providing a password.
4. **Post-Exploitation:** Located (`ls`)and grabbed the flag from the home directory (`cat flag.txt`).
   ---
##  🦌 2. Machine: Fawn (Topic: Understanding FTP(File Tranfer Protocol))
**Objective:** Audit an FTP server and exploit anonymous access permition
**Tools used:** `nmap`, `ftp`
### Methology:
1. **Reconnaissance:** performed a version service scan on the target IP bypassing ping detection using Nmap:
   ```bash
   sudo nmap -Pn -sV <TARGET_IP>
   ```
   *the scan revealed that port **21/ftp** was open*
 2. **Vulnerability:** The target FTP daemon was enabled with **Anonymous FTP feature enabled**. This misconfigured protocol allowed me (**Public Unauthenticated User**)to get access to directories
 3. **Exploitation:** I logged in the server as an anonymous user
    ```bash
    ftp <TARGET_IP>
    ```
4. **Post Exploitation:** I used `ls` command to list directories and find the flag and got the flag using `get flag.txt`

# Hack The Box Crocodile
* **Date:** October 2026
* **Vulnerability Class:** Information Disclosure & Web Directory Enumeration (chained attack)
* **Tools Used:** `nmap`, `ftp`,<span style="color:red;"> `gobuster` </span> 
  ---
  ## 🎯 Objective
  The goal of this lab is to compromise the administrative control panel of a target web server by locating hidden pages and credential leaks through service chaining.
  ## 🛠️ Methodology
  ### 1. Reconnaissance (Port Scanning)
  I initiated the assessment by executing a targeted service-version scan using Nmap while bypassing ping discovery (`-Pn`):
  ```bash
  sudo nmap -Pn -sV <TARGET_IP>
  ```
  The scan successfully mapped the attack surface, revealing two active doors:
* **Port 21/tcp** - File Transfer Protocol (FTP) running `vsftpd 3.0.3`
* **Port 80/tcp** - Apache Web Server (HTTP) hosting a webpage
  ### 2. Information Disclosure (FTP Enumeration)
Leveraging past knowledge, I attempted an unauthenticated **anonymous login** against the active FTP service:
```bash
ftp <TARGET_IP>
```
* **Username:** `anonymous`
* **Password:** ```(Enter)```
  The connection was granted, exposing **two**  critical administrative logs. I extracted them locally using the 'get'command:
* `allowed.userlist` — Contained a structural list of valid system usernames.
* `allowed.userlist.passwd` — Contained developer task notes leaking valid plain-text password strings.

  ---
    ### 3. Web Directory Brute-Forcing (Gobuster)
Navigating directly to `http://<TARGET_IP>/` in the browser displayed a **static promotional page** with no interactive login interfaces.

To map out hidden entry points, I performed a directory brute-force attack using **Gobuster** and a common routing dictionary, specifically filtering for `.php` and `.html` extensions:
```bash
gobuster dir -u http://<TARGET_IP>/ -w /usr/share/wordlists/dirb/common.txt -x php,html
```
  **Gobuster** discovered an insulated authentication gateway at:
* `Status: 200` -> `http://<TARGET_IP>/login.php`
  ### 4. Exploitation & Post-Exploitation
I crossed-referenced the credential assets harvested from the FTP leak against the newly discovered `/login.php` endpoint.

By inputting the administrative username and its associated plain-text password, I bypassed the login barrier. Once inside the secure administrator dashboard, the system flag was fully disclosed on the interface.

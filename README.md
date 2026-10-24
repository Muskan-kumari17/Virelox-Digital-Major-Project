# Full-Chain Enterprise Perimeter Breach & Host Takeover

**Enterprise:** Virelox Digital
**Group:** G10
**Project:** Simple CTF — TryHackMe
**Student Name:** Muskan Kumari
**Kali Linux Hostname:** `virelox-digital`
**Kali Linux Username:** `muskan`
**Target IP:** `10.48.140.200`
**Platform:** TryHackMe

---

## 1. Project Overview

This project documents a cybersecurity assessment of the Simple CTF lab on the TryHackMe platform, presented under the Virelox Digital enterprise identity.

The assessment focuses on identifying exposed services, investigating a web application vulnerability, obtaining authorized lab access, and demonstrating the security impact of a Linux privilege-escalation misconfiguration.

All testing was conducted within the authorized TryHackMe laboratory environment.

## 2. Project Objectives

* Identify open ports and services on the target machine.
* Perform network reconnaissance and web directory enumeration.
* Identify the CMS Made Simple application and investigate its known SQL injection vulnerability.
* Document credential recovery and authenticated remote access.
* Analyze Linux sudo permissions and privilege-escalation risks.
* Map the observed attack path to the Cyber Kill Chain.
* Recommend defensive controls to prevent similar security incidents.

## 3. Tools and Technologies

* Kali Linux
* Nmap
* Gobuster
* CMS Made Simple
* Python 3
* Exploit-DB
* SSH
* Metasploit Framework
* Linux privilege enumeration
* GTFOBins

## 4. Target Information

| Parameter        | Details           |
| ---------------- | ----------------- |
| Enterprise       | Virelox Digital   |
| Group            | G10               |
| Lab              | Simple CTF        |
| Platform         | TryHackMe         |
| Target IP        | `10.48.140.200`   |
| Attacker Machine | Kali Linux        |
| Hostname         | `virelox-digital` |
| Web Application  | CMS Made Simple   |

**Note:** The target IP belongs to the lab instance documented in the report. TryHackMe target IP addresses may change when a new machine instance is started.

## 5. Technical Execution Lifecycle

### Phase 1: Network Reconnaissance

Nmap was used to identify open ports, running services, and service versions. Gobuster was used for web directory enumeration.

**Target:** `10.48.140.200`

The documented scan identified the following services:

| Port     | Service | Version / Observation        |
| -------- | ------- | ---------------------------- |
| 21/tcp   | FTP     | vsftpd 3.0.3                 |
| 80/tcp   | HTTP    | Apache httpd 2.4.18 (Ubuntu) |
| 2222/tcp | SSH     | OpenSSH 7.2p2 (Ubuntu)       |

Anonymous FTP access was also observed during the assessment.

**Security concern:** Exposed services and unnecessary anonymous access can increase the attack surface.

### Phase 2: Web Application Analysis and SQL Injection

The web application was identified as CMS Made Simple, accessible at:

`http://10.48.140.200/simple/`

The assessment investigated the publicly documented unauthenticated SQL injection vulnerability affecting CMS Made Simple versions earlier than 2.2.10.

* **CVE:** CVE-2019-9053
* **Exploit-DB ID:** 46635
* **Finding:** The documented vulnerability can expose database information, including administrator credential data.

The report documents the recovery of a salted password hash and offline password cracking.

**Security impact:** An unauthenticated SQL injection vulnerability may expose sensitive database information and enable further compromise.

### Phase 3: Authenticated Remote Access

The report documents SSH access to the target using the recovered lab credentials.

* **SSH port:** `2222`
* **Lab username:** `mitch`
* **User flag output:** `G00d j0b, keep up!`

The authenticated SSH session provided command-line access as a low-privileged user.

### Phase 4: Metasploit and Meterpreter

The project includes a Metasploit/Meterpreter session-upgrade methodology.

The report describes importing scan results, configuring a handler or suitable module, and the need to capture an established Meterpreter session as proof of concept.

**Status:** A successfully established Meterpreter session is marked as pending in the report. It is not claimed as a completed result.

### Phase 5: Linux Privilege Escalation

Post-access enumeration identified a sudo permission misconfiguration involving Vim.

The report documents that user `mitch` could run `/usr/bin/vim` as root without a password.

The resulting privilege level was verified as root.

**Documented evidence:**

* Root identity confirmed with `id`.
* Root flag output: `W3ll d0n3. You made it!`

**Security impact:** Allowing unrestricted privileged execution of certain programs can enable a low-privileged user to obtain root access.

## 6. Cyber Kill Chain Mapping

| Kill Chain Phase      | Observed Activity                                                          | Project Phase |
| --------------------- | -------------------------------------------------------------------------- | ------------- |
| Reconnaissance        | Nmap service scanning and Gobuster directory enumeration                   | Phase 1       |
| Weaponization         | Selection of the publicly documented CMS Made Simple SQL injection exploit | Phase 2       |
| Delivery              | SQL injection requests to the CMS application                              | Phase 2       |
| Exploitation          | Database credential information recovery and offline password cracking     | Phase 2       |
| Installation          | Authenticated SSH access using recovered lab credentials                   | Phase 3       |
| Command and Control   | Interactive command-line access and host enumeration                       | Phase 3–4     |
| Actions on Objectives | Privilege escalation through the documented sudo/Vim misconfiguration      | Phase 5       |

## 7. Security Findings

The assessment documented the following security concerns:

1. Exposed FTP, HTTP, and SSH services.
2. Anonymous FTP access.
3. A vulnerable CMS Made Simple installation.
4. Exposure of credential information through SQL injection.
5. Risk associated with weak or reused passwords.
6. Excessive sudo privileges involving Vim.
7. The potential for escalation from a low-privileged account to root.

## 8. Defensive Remediation

### 8.1 Patch Management

* Upgrade CMS Made Simple to a secure, supported version.
* Apply security patches to the operating system and exposed services.
* Remove or disable unsupported and unnecessary services.

### 8.2 SQL Injection Prevention

* Use parameterized queries and prepared statements.
* Validate user input on the server side.
* Avoid building SQL statements through unsafe string concatenation.
* Use least-privileged database accounts.
* Log and monitor suspicious database requests.

### 8.3 FTP and Network Security

* Disable anonymous FTP access unless explicitly required.
* Restrict access to exposed services using firewall rules.
* Limit SSH access to trusted users and networks.
* Monitor authentication attempts and unusual network activity.

### 8.4 Password Security

* Enforce strong, unique passwords.
* Store passwords using an appropriate modern password-hashing algorithm.
* Change credentials that may have been exposed.
* Review authentication logs for suspicious activity.

### 8.5 Linux Privilege Hardening

* Remove unnecessary `NOPASSWD` sudo permissions.
* Review sudoers configuration regularly.
* Audit SUID/SGID binaries and file permissions.
* Apply the principle of least privilege.
* Monitor privileged command execution.

### 8.6 Web Application Hardening

* Validate file types and MIME types on the server.
* Store uploaded files outside executable web directories.
* Prevent uploaded content from being executed as server-side code.
* Apply secure configuration and regular vulnerability testing.

## 9. Project Deliverables

The project documentation includes:

* Technical reconnaissance findings.
* Service and version identification.
* Web application and vulnerability analysis.
* Credential recovery and SSH access findings.
* Linux privilege-escalation evidence.
* Cyber Kill Chain mapping.
* Defensive remediation recommendations.
* Technical proof-of-concept archive requirements.

**Technical PoC archive:** Add the shareable Google Sheets link here before submission.

**TryHackMe Profile:**
https://tryhackme.com/p/missmuskan9142

## 10. Limitations

The assessment was performed against the assigned TryHackMe lab target. The Meterpreter session evidence remains pending, as documented in the report. No successful Meterpreter session is claimed in this README.

Target IP addresses and lab conditions may change between sessions.

## 11. Conclusion

This project demonstrates how exposed services, a vulnerable web application, credential weaknesses, and excessive Linux sudo permissions can combine to create a serious security risk.

The documented assessment connects network reconnaissance and web application analysis with authenticated remote access and local privilege escalation. The recommended remediation measures focus on patch management, secure database coding, access control, password security, and Linux privilege hardening.

The findings emphasize the importance of layered security controls, continuous monitoring, and the principle of least privilege.

## 12. References

* TryHackMe — Simple CTF lab
* Exploit-DB — CMS Made Simple SQL Injection, EDB-ID 46635
* CVE-2019-9053
* GTFOBins — Vim
* OWASP — SQL Injection Prevention Cheat Sheet

---

**Project:** Cybersecurity Major Project
**Enterprise:** Virelox Digital
**Group:** G10
**Student:** Muskan Kumari

**Disclaimer:** This documentation describes an authorized educational lab assessment. Testing must remain within the assigned TryHackMe environment and other explicitly authorized systems.

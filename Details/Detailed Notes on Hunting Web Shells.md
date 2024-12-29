### **Hunting Web Shells – Detailed Guide**

#### **What is a Web Shell?**

A web shell is a script uploaded to a web server that provides attackers with remote administration capabilities. These scripts are used to gain unauthorized access, escalate privileges, and pivot to other systems in a network.

- **Targets**: Both internet-facing and internal web servers.
- **Languages**: Commonly PHP and ASP, but any language supported by the server (e.g., JSP, Python, Ruby) can be used.

**US-CERT Definition**:  
"A web shell is a script that can be uploaded to a web server to enable remote administration of the machine."

#### **Common Attack Vectors**

1. **Cross-Site Scripting (XSS)**: Injecting malicious scripts into a trusted website.
2. **SQL Injection (SQLi)**: Exploiting vulnerabilities in database queries.
3. **Remote/Local File Inclusion (RFI/LFI)**: Forcing the server to execute malicious files remotely or locally.
4. **Server Misconfigurations**: Exploiting improperly secured directories, permissions, or services.

---

#### **Examples of Web Shells**

- **C99**: A popular PHP-based web shell with numerous features for attackers.
- **B374K**: Another advanced PHP web shell offering file management, SQL database interaction, and more.
- **R57**: Older but still-used PHP shell with basic features.

---

#### **Challenges in Detection**

1. **Obfuscation**: Attackers modify code to bypass antivirus (AV) detection.
    - Example: Encoding payloads, variable renaming, or using encryption.
2. **Signatures**: Static AV tools rely on known signatures, making detection difficult for modified shells.

#### **Detection Methods**

- Search for suspicious keywords like `cmd.exe`, `eval()`, `base64_decode()`, etc.
- Monitor unusual server behaviors, such as unexpected file uploads or process activity.

---

### **Tools for Hunting Web Shells**

| **Tool**                  | **Description**                                                              |
| ------------------------- | ---------------------------------------------------------------------------- |
| **LOKI**                  | Free IOC scanner with support for YARA rules and hash matching.              |
| **NeoPI**                 | Detects obfuscated/encrypted code using statistical analysis.                |
| **BackdoorMan**           | Finds malicious PHP scripts and web shells using a signature database.       |
| **PHP-malware-finder**    | Uses YARA rules to find obfuscated PHP code and suspicious functions.        |
| **unPHP**                 | Online tool for decoding obfuscated PHP scripts.                             |
| **Web Shell Detector**    | Detects web shells (PHP, ASP, Perl) with a high accuracy signature database. |
| **Linux Malware Detect**  | Designed for detecting malware in Linux-based shared hosting environments.   |
| **Invoke-ExchangeHunter** | PowerShell script to detect web shells on Microsoft Exchange servers.        |
| **NPROCWATCH**            | Monitors and logs new processes spawned on a system for suspicious activity. |

---

### **How to Use These Tools**

#### **LOKI**

- Features:
    - Detects known IOC hashes (MD5, SHA1, SHA256).
    - Matches YARA rules on files and process memory.
    - Flags filenames matching hard/soft indicators via regex.
- Usage: Run LOKI on target directories to detect web shells or suspicious files.

#### **NeoPI**

- Features:
    - Performs recursive scans from a base directory.
    - Applies statistical tests to rank files by likelihood of obfuscation.
- Usage: Use NeoPI to identify encrypted or obfuscated shell code.

#### **BackdoorMan**

- Features:
    - Detects malicious PHP scripts, suspicious functions, and shells by filename or activity.
    - Integrates external APIs like VirusTotal and UnPHP for enhanced detection.
- Usage: Scan directories for potential backdoors and validate findings using APIs.

#### **PHP-malware-finder**

- Features:
    - Crawls filesystems for PHP malware based on YARA rules.
    - Identifies malicious PHP functions commonly used in shells.
- Usage: Run against the filesystem to detect obfuscated PHP web shells.

#### **unPHP**

- Features: Online decoder to reverse PHP obfuscation.
- Usage: Submit obfuscated scripts to decode and analyze their intent.

#### **Web Shell Detector**

- Features: High accuracy shell detection using a robust signature database.
- Usage: Deploy on web servers to identify known shell patterns.

#### **Linux Malware Detect (LMD)**

- Features: Combines edge intrusion detection with active malware signature generation.
- Usage: Ideal for shared hosting environments where malware is prevalent.

#### **Invoke-ExchangeHunter**

- Features: Scans Exchange servers for indicators of web shell activity.
- Usage: Specifically for Microsoft Exchange environments.

#### **NPROCWATCH**

- Features: Monitors and displays new system processes in real time.
- Usage: Use on critical servers to track anomalous activity during investigations.

---

### **Preventive Measures**

1. Implement secure coding practices to avoid injection vulnerabilities.
2. Regularly patch and update software to close security loopholes.
3. Use firewalls and WAFs to block suspicious traffic.
4. Conduct periodic scans using the above tools to identify potential threats.

---

### **Resources for Further Study**

- **US-CERT Alert on Web Shells**: [TA15-314A](https://us-cert.cisa.gov/ncas/alerts/TA15-314A)
- **OWASP Top 10 Vulnerabilities**: [OWASP Web Security](https://owasp.org/www-project-top-ten/)

This comprehensive guide should serve as a detailed resource for hunting web shells and securing web servers effectively.
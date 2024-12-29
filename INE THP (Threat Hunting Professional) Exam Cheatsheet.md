### **1. Threat Hunting Frameworks**

|**Framework**|**Purpose**|**Key Features**|
|---|---|---|
|**MITRE ATT&CK**|Identify TTPs in an attack|Tactics, Techniques, and Procedures|
|**Diamond Model**|Visualize threats|Adversary, Infrastructure, Capability, Victim|
|**Cyber Kill Chain**|Attack lifecycle identification|7 Steps: Recon -> Weaponize -> Deliver -> Exploit -> Install -> C2 -> Action|

---

### **2. Key Threat Hunting Steps**

|**Step**|**Description**|
|---|---|
|**1. Hypothesis**|Form a question (e.g., Is there lateral movement?)|
|**2. Data Collection**|Gather logs: EDR, network data, SIEM|
|**3. Data Analysis**|Correlate logs, spot anomalies|
|**4. Investigate**|Verify suspicious activities|
|**5. Action**|Mitigate, document, and report|

---

### **3. Common Data Sources**

|**Source**|**Purpose**|**Tools**|
|---|---|---|
|**Endpoint Logs**|Process, File, Registry analysis|Sysmon, EDR, Windows Logs|
|**Network Data**|Monitor traffic patterns|PCAP, Zeek, Suricata, NetFlow|
|**Authentication Logs**|Detect lateral movement|AD Logs, Kerberos, Event 4624|
|**SIEM Logs**|Aggregate & analyze logs|Splunk, ELK, Qradar|

---

### **4. Key Tools and Commands**

|**Tool**|**Purpose**|**Common Commands**|
|---|---|---|
|**Sysmon**|Process/file monitoring|`Get-SysmonLog`|
|**Volatility**|Memory forensics|`vol.py -f memdump pslist`|
|**Wireshark**|Network packet analysis|`Follow TCP Stream`|
|**ELK Stack**|Log management/analysis|Queries in Kibana|

---

### **5. MITRE ATT&CK Tactics & Techniques**

|**Tactic**|**Example Technique**|
|---|---|
|**Initial Access**|Phishing (T1566), Exploit Public-Facing Apps (T1190)|
|**Execution**|PowerShell (T1059.001), Scheduled Task (T1053)|
|**Persistence**|Registry Run Keys (T1547), Services (T1543)|
|**Privilege Escalation**|Exploit SUDO/ SUID (T1548)|
|**Defense Evasion**|Obfuscation (T1027), Masquerading (T1036)|
|**Lateral Movement**|SMB/WinRM (T1021), RDP (T1076)|
|**Exfiltration**|Encrypted Channel (T1048)|

---

### **6. Query Examples**

|**Tool**|**Query**|
|---|---|
|**Splunk**|`index=windows EventCode=4624`|
|**KQL (Kibana)**|`process.name: "powershell.exe"`|
|**Wireshark**|`http contains "cmd.exe"`|

---

### **7. Indicators of Compromise (IoCs)**

|**Category**|**Examples**|
|---|---|
|**File**|Malicious hashes (MD5, SHA1)|
|**Network**|C2 IPs, Domains, Ports|
|**Behavioral**|Suspicious processes, scripts|

---

### **8. Key Exam Tips**

1. **Understand TTPs** (MITRE ATT&CK).
2. Use **common hunting tools** effectively (Splunk, Sysmon, Wireshark).
3. Know how to **analyze logs and spot anomalies**.
4. Master **hypothesis-driven hunting** methods.
5. Time management: Prioritize questions.

---

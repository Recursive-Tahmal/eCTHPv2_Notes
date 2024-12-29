## **Definition**

A **web shell** is a malicious script uploaded to a web server that provides attackers with remote access and control. It allows adversaries to execute arbitrary commands, exfiltrate data, manipulate files, and maintain persistent access.

---

## **Common Web Shell Types**

|**Language**|**Examples**|**Characteristics**|
|---|---|---|
|**PHP**|`C99`, `B374K`, `R57`|Most common; runs on Apache, supports various commands.|
|**ASP**|Classic ASP scripts|Runs on IIS servers; easy to manipulate.|
|**JSP**|JSP-based shells|Targets Java-based servers (e.g., Tomcat).|
|**Other**|Python, Perl, Ruby scripts|Server-dependent; less common but still effective.|

---

## **Common Upload Methods**

1. **XSS (Cross-Site Scripting)**: Exploiting input validation flaws to upload or execute scripts.
2. **SQL Injection (SQLi)**: Injecting malicious SQL queries to write web shell code to files on the server.
3. **Remote/Local File Inclusion (RFI/LFI)**: Exploiting vulnerabilities to include web shells from external or local directories.
4. **Misconfigured Web Servers**: Exploiting weak security settings, allowing unrestricted uploads.

---

## **Baselines**

### **What is a Baseline?**

A baseline is a known-good reference file for configurations, file structures, or system states. By comparing current states to the baseline, anomalies can be identified, such as new or modified files.

### **Creating a Baseline with PowerShell**

Use PowerShell to recursively hash all files in a directory and save results to a CSV file:

```powershell
Get-ChildItem -Path .\FooCompany -File -Recurse | Get-FileHash -Algorithm MD5 | Export-Csv C:\Baseline-WWW-FooCompany.csv
```

- `Get-ChildItem`: Lists all files recursively from the specified path.
- `Get-FileHash`: Generates the **MD5** hash for each file.
- `Export-Csv`: Exports the results to a CSV file.

### **Comparing Baselines**

To detect changes since the baseline was created, use **Compare-FileHashesList.ps1**:

```powershell
Compare-FileHashesList.ps1 -Old C:\Baseline-WWW-FooCompany.csv -New C:\07192017-WWW-FooCompany.csv
```

#### **Outputs**:

- **File Status**:
    - `new` - New file detected.
    - `changed` - File modified.
    - `missing` - File no longer exists.
- **Summary**: Aggregates changes without displaying file paths.

---

## **Detection Techniques for Web Shells**

### **1. File Content Analysis**

Look for suspicious PHP functions that indicate malicious activity:

- `eval()`
- `exec()`
- `system()`
- `base64_decode()`
- `fsockopen()`

#### **Linux Commands**

- Find recently modified or new PHP files:
    
    ```bash
    find . -type f -name '*.php' -mtime -1
    ```
    
- Search for malicious functions:
    
    ```bash
    find . -type f -name '*.php' | xargs grep -i "eval("
    find . -type f -name '*.php' | xargs egrep -i "(exec|system|eval|base64_decode)\("
    ```
    

#### **Windows PowerShell Commands**

- Identify suspicious PHP files:
    
    ```powershell
    get-childitem -recurse -include "*.php" | select-string "mail|fsockopen|exec\b|system\b|eval\b|base64_decode" | % {"$($_.filename):$($_.line)"} | out-gridview
    ```
    
- Check for recently modified files:
    
    ```powershell
    Get-ChildItem -Recurse -File | Select-Object FullName, Length, LastWriteTime | Out-GridView
    ```
    

---

### **2. Statistical Analysis**

Statistical analysis of **web logs** helps uncover unusual file activity based on:

- **Execution Count**: Files accessed far more/less than expected.
- **Load Times**: Longer execution times may indicate malicious code.

#### **Tool: Log Parser Studio**

- Parses IIS, EXADB, and event logs.
- SQL-based queries identify abnormal file behaviors.

Example: Detect `WebShell.asp` accessed fewer times but with high execution time, raising suspicion.

---

### **3. Detecting Web Shells Hidden in Image Files**

Attackers can embed web shell code in **JPEG Exif headers**.

#### **How It Works**

Exif metadata is appended to images, which can contain malicious PHP code:

- Example suspicious headers:
    - `Copyright: /.*/e`
    - `Document Name: eval(base64_decode(...))`

#### **Tools**

- **exiftool**: Inspect metadata of images:
    
    ```bash
    exiftool image.jpg
    ```
    
- Example PHP trigger code:
    
    ```php
    <?php  
    $exif = exif_read_data('image.jpg');  
    preg_replace($exif['Copyright'], $exif['Document Name']);  
    ?>  
    ```
    
- Use a **PHP script** to recursively scan for Exif data containing `/e` modifiers and Base64 decoding.
    

---

## **Process Monitoring for Web Shell Activity**

### **What is w3wp.exe?**

`w3wp.exe` is an **IIS worker process** responsible for executing web applications and responding to requests.

### **How Web Shells Trigger w3wp.exe**

When web shells execute commands like `ping`, they spawn:

- **w3wp.exe** → **cmd.exe** → **conhost.exe** → **ping.exe**

---

### **Monitoring w3wp.exe Processes with PowerShell**

The following script monitors `w3wp.exe` for child processes:

```powershell
While($true) {  
   If ($(Get-WmiObject -Class Win32_Process -Filter "Name='w3wp.exe'") -ne $null) {  
       $p = Get-WmiObject -Class Win32_Process -Filter "Name='w3wp.exe'" | Select ProcessID, CreationDate  
       $pp = $p.ProcessId  
       Get-WmiObject -Class Win32_Process -Filter "ParentProcessId=$pp" | ? { $_.Name -eq "cmd.exe" } | & C:\hunting\scripts\Get-W3WPChildren.ps1  
   }  
}
```

**Output**: Displays any child processes spawned by `w3wp.exe`.

---

## **Hunting Tools**

|**Tool**|**Purpose**|
|---|---|
|**LOKI**|Scans for IOCs (hashes, YARA rules, filenames).|
|**NeoPI**|Identifies obfuscated/encrypted PHP code statistically.|
|**BackdoorMan**|Detects malicious PHP scripts using APIs like VirusTotal.|
|**Web Shell Detector**|Signature-based web shell detection for multiple languages.|
|**Linux Malware Detect**|Scans Linux servers for malware and web shells.|
|**exiftool**|Analyzes Exif metadata for embedded PHP code.|
|**Log Parser Studio**|Parses logs for unusual execution counts and load times.|
|**NPROCWATCH**|Monitors new processes and their parent-child relationships.|

---

## **Conclusion**

Hunting web shells requires a combination of techniques:

1. **Baselines**: Compare current file states to a trusted baseline.
2. **File Analysis**: Search for suspicious functions and modifications.
3. **Statistical Analysis**: Detect anomalies in execution and load times.
4. **Metadata Analysis**: Inspect Exif headers for hidden PHP code.
5. **Process Monitoring**: Track `w3wp.exe` for unexpected child processes.

By leveraging tools like **Log Parser Studio**, **exiftool**, and **NPROCWATCH**, attackers’ web shells can be detected and mitigated efficiently.

---

Let me know if further refinement is needed!
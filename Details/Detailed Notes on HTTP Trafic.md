### HTTP & HTTPS Traffic - Detailed Notes

**HTTP (HyperText Transfer Protocol)** and **HTTPS (HTTP Secure)** are the fundamental protocols used to transfer data over the web. HTTP is the unencrypted version, while HTTPS uses encryption (SSL/TLS) to protect data in transit. Both protocols follow a request-response model between clients (typically browsers) and servers.

---

#### **Normal HTTP Traffic**:

- **Ports**:
    - **Port 80 (TCP)**: Standard port for HTTP traffic.
    - **Port 8080, 8088 (TCP)**: Alternate ports often used for HTTP traffic.
- **Plaintext**: HTTP traffic is transmitted as plaintext, meaning data such as URLs and headers are visible.
- **Request-Response Model**: HTTP traffic consists of client requests and server responses.
    - **HTTP Methods**: These methods, such as GET, POST, PUT, DELETE, are defined in **RFC 2616**. The server may not support all methods, depending on configuration.
    - **Response Codes**: HTTP responses include a 3-digit status code (e.g., 200 OK, 404 Not Found).
- **FQDN Format**: Web servers are typically accessed by their **Fully Qualified Domain Name (FQDN)** (e.g., `example.com`), rather than an IP address.

**Example Normal HTTP Behavior**:

- **Packet Flow**:
    - The TCP handshake (packets 3-6) occurs before the HTTP request is made (packet 7 with `GET` method).
    - The server responds with a `200 OK` status code (packet 9).
- **Traffic Inspection**: By analyzing the HTTP section, it’s evident the traffic is using port 80, and the host is in **FQDN format** (e.g., `example.com`).

**Wireshark Inspection**:

- **TCP Streams**: In Wireshark, HTTP traffic is part of different TCP streams. Identifying these helps analyze the flow of requests and responses.
    - For example, inspecting stream details can reveal HTTP methods (`GET` or `POST`), headers, and response codes.
    - If you are tracking logins or sensitive information, inspect the traffic for **cleartext credentials**.

---

#### **Suspicious HTTP Traffic**:

1. **Traffic to IP Address Instead of FQDN**:
    
    - **Suspicious**: Direct connections to an IP address (instead of an FQDN) can indicate abnormal or potentially malicious traffic. However, this can be normal in internal environments when accessing internal servers.
2. **Cleartext Traffic**:
    
    - While HTTP traffic is typically cleartext, sensitive data like login credentials should ideally be transmitted over **HTTPS** (secure HTTP). Cleartext credentials over HTTP are a major security risk.
3. **Malicious Content**:
    
    - Malicious binaries, **backdoors**, or **scripts** (like web shells) can use HTTP on port 80 to evade detection, especially in corporate networks where port 80 is typically open.
4. **SQL Injection (SQLi) Attempts**:
    
    - **Example**: In a packet capture (PCAP), requests such as `newsdetails.php?id=26%27` (where `%27` decodes to a single quote `'`) might indicate **SQL Injection** (SQLi) attempts.
    - Further inspection reveals repeated SQLi patterns, especially when the server responds with errors, indicating a successful attempt to exploit a vulnerability.
    - **Tool Usage**: If the traffic continues with similar patterns, it may indicate automated attack tools like **sqlmap**, which are used to test for SQLi vulnerabilities.
    
    **Example SQLi Detection**:
    
    - Initial packets may show typical web traffic requests.
    - Subsequent requests may attempt to exploit the SQL injection vulnerability (e.g., through altered URLs with special characters like `%27` for a single quote).
    - A change in the **User-Agent** header, such as `Sqlmap`, signals that an automated tool is now being used for exploitation.

---

#### **Detecting SQL Injection (SQLi)**:

1. **Manual Testing**:
    
    - The attacker may manually inspect the server's response to crafted input (e.g., adding a single quote or semicolon in the URL).
    - **Error Messages**: If the server returns MySQL error messages in response to the query, it’s an indicator that SQL injection is being attempted.
2. **Automated Tools**:
    
    - Once enough information is gathered, the attacker may switch to automated tools (e.g., **sqlmap**) to perform more refined SQLi tests.
    - In Wireshark, you can identify these tools by inspecting the **User-Agent** field (e.g., `Sqlmap`).

**PCAP Inspection Example**:

- **Initial Manual Attempts**: The attacker uses Firefox on Linux (shown in the `User-Agent`), trying different inputs in URLs to test for vulnerabilities.
- **Tool Escalation**: After manual probing, the **Sqlmap** tool is detected in subsequent HTTP requests, signaling a shift from manual testing to automated exploitation.

---

#### **Wireshark Techniques for Analyzing HTTP Traffic**:

1. **TCP Streams**:
    
    - Use **Follow TCP Stream** to isolate HTTP requests and responses within a single stream.
    - This helps in analyzing the flow of traffic and identifying any anomalies (e.g., unexpected cleartext credentials).
2. **Protocol Hierarchy**:
    
    - By selecting **Statistics > Protocol Hierarchy**, you can inspect the frequency of protocols within the packet capture.
    - This can help identify if HTTP traffic is being used for other, unexpected purposes.
3. **Export HTTP Objects**:
    
    - The **File > Export Objects > HTTP** option allows you to save files (such as scripts, images, or binaries) transferred via HTTP for later inspection.

---

#### **Conclusion**:

**Normal HTTP Traffic**:

- Occurs over **port 80 (TCP)** or alternate ports like **8080/8088**.
- Uses **plaintext** communication.
- Accesses web servers using **FQDNs**.
- Should not contain sensitive information like credentials in cleartext.

**Suspicious HTTP Traffic**:

- **Traffic to an IP** instead of FQDNs.
- **SQLi attempts** or other common web-based attacks.
- Use of **malicious tools** like `sqlmap` seen in **User-Agent** headers.
- **Malicious payloads** like web shells or backdoors transmitted via HTTP.

To effectively identify suspicious HTTP traffic, it is important to analyze packet details using Wireshark, focusing on TCP streams, request-response patterns, and unusual or malicious content such as SQLi attempts or malware.
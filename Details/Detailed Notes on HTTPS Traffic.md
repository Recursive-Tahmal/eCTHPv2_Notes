### HTTPS Traffic: Detailed Notes

---

### **Overview of HTTPS**

HTTPS (Hypertext Transfer Protocol Secure) is the secure version of HTTP, designed to ensure secure communication between a client and a server. It employs encryption protocols like **SSL (Secure Sockets Layer)** or **TLS (Transport Layer Security)** to protect data.

#### **Key Points About HTTPS**:

1. HTTPS handshake is similar to a TCP handshake but more complex.
2. **Protocol Agreement**: The client and server agree on the protocol version.
3. **Cryptographic Algorithms**: Both sides select algorithms for encryption.
4. **Authentication**: Optionally, client and server authenticate each other.
5. **Encryption**: Public key encryption is used to establish a secure channel.

---

### **Characteristics of HTTPS Traffic**

#### **Normal HTTPS Traffic**:

- **Ports**:
    - Standard: 443 (TCP)
    - Alternate: 8443 (TCP)
- **Traffic**: Encrypted, ensuring privacy and integrity.
- **Destination**: Web servers are typically in Fully Qualified Domain Name (FQDN) format.

#### **Suspicious HTTPS Traffic**:

- Malicious binaries, backdoors, or web shells may use port 443 since it's often open in corporate networks.
- **Encryption Issues**: Traffic that is not encrypted or contains empty Secure Sockets Layer fields in packet details is suspicious.
- **Destination**: Servers using IP addresses instead of FQDNs.

---

### **HTTPS Handshake**

HTTPS handshake establishes secure communication. Key steps include:

1. **Client Hello**:
    - Specifies the **protocol version** (e.g., TLS 1.2).
    - Lists supported **cipher suites** (e.g., AES, RSA).
    - Includes random values for encryption.
2. **Server Hello**:
    - Confirms the protocol version and cipher suite.
    - Provides the server's digital certificate for authentication.
3. **Key Exchange**:
    - The server sends a **Server Key Exchange**.
    - The client replies with a **Client Key Exchange** to establish a shared secret.
4. **Finished Message**:
    - Both sides verify the handshake, after which communication is encrypted.

#### **Packet Analysis Using Wireshark**:

- **Client Hello Packet**:
    - **Content Type**: Handshake.
    - **TLS Version**: Indicates protocol (e.g., TLS 1.2).
    - **Cipher Suites**: Lists supported encryption algorithms.
- **Server Hello Packet**:
    - Confirms encryption methods and provides the server certificate.
- **Encrypted Data**:
    - Post-handshake, all data packets are encrypted and unreadable without decryption.

---

### **Decrypting HTTPS Traffic**

- **Decryption with Private Keys**:  
    Traffic can be decrypted using the server's private key in environments where HTTPS traffic needs monitoring.  
    Steps in Wireshark:
    
    - Go to `Edit > Preferences > Protocols > SSL`.
    - Import the private key (`+` symbol) with the server's IP, port, protocol, and key file path.
- **Decrypted Data**: Once decrypted, the HTTPS packet reveals HTTP data (e.g., GET/POST methods, status codes).
    

---

### **Suspicious HTTPS Example: CrypMIC Ransomware**

1. **Behavior**:
    - CrypMIC used port 443 for communication with its Command-and-Control (C2) server.
    - Traffic included unencrypted ransom notes over port 443.
2. **Packet Inspection**:
    - **Statistics > Protocol Hierarchy**: Identified suspicious encrypted protocols.
    - **Statistics > Endpoints**: Found traffic to high-risk countries.
    - **Statistics > Conversations**: Highlighted heavy port 443 communication to specific IPs.
    - **TCP Stream Analysis**:
        - No SSL/TLS handshake packets found.
        - Plaintext ransom note visible in packets 560 & 561.
3. **Indicators of Suspicion**:
    - Empty Secure Sockets Layer field.
    - Plaintext data on port 443.

---

### **Attack Vectors and Monitoring**

- HTTP and HTTPS ports (80, 443) are common targets due to their widespread use and often limited monitoring.
- Monitoring these ports and detecting anomalies (e.g., cleartext traffic, unusual handshake behavior, suspicious IP addresses) is critical in threat hunting.

### **Conclusion**

Understanding HTTPS behavior is essential for distinguishing between legitimate encrypted traffic and suspicious or malicious activity. Tools like Wireshark enable detailed inspection and decryption, helping to detect anomalies such as unencrypted ransomware traffic or unauthorized server communications.
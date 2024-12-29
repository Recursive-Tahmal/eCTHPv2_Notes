### OSI Model Overview

The OSI (Open Systems Interconnection) model consists of **7 layers**, each with a specific role in network communication. Understanding these layers helps analyze how data travels and where issues might occur.

|**Layer**|**Description**|**Examples**|
|---|---|---|
|**7. Application**|Interfaces directly with user applications for network communication.|FTP, HTTP, SMTP, DNS|
|**6. Presentation**|Translates, encrypts, or compresses data for the application layer.|SSL/TLS, JPEG, ASCII|
|**5. Session**|Establishes, manages, and terminates connections between devices.|NetBIOS, RPC|
|**4. Transport**|Ensures reliable data delivery via error checking and retransmission.|TCP (reliable), UDP (fast)|
|**3. Network**|Routes data between devices on different networks using logical addressing (IP).|IP, ICMP, ARP|
|**2. Data Link**|Handles data transfer between devices on the same network using MAC addresses.|Ethernet, PPP, Switches|
|**1. Physical**|Manages the transmission of raw bitstreams over physical media.|Cables, Hubs, Fiber Optics|

---

#### Key Points:

- Each layer performs a **specific function** and communicates with the layers directly above and below it.
- **Encapsulation**: Each layer adds its own header during data transmission.
- At the receiving end, headers are removed in the reverse order.

---

#### OSI Model in Threat Hunting:

1. **Physical Layer**: Detect hardware-related issues like cable failures.
2. **Data Link Layer**: Identify suspicious MAC address behavior.
3. **Network Layer**: Monitor IP-level anomalies like spoofing or unexpected routes.
4. **Transport Layer**: Analyze TCP/UDP traffic for abnormal patterns.
5. **Application Layer**: Spot malicious applications or protocols in use.

This layered approach ensures systematic analysis and troubleshooting during threat-hunting activities.
### TCP Traffic (Detailed Notes)

**TCP (Transmission Control Protocol)** is a connection-oriented protocol ensuring reliable data delivery between source and destination nodes. It handles **packet sequencing**, **error recovery**, and uses a **3-way handshake** to establish connections.

---

#### **Key TCP Concepts**:

1. **3-Way Handshake**:
    
    - **SYN**: Client initiates the connection.
    - **SYN/ACK**: Server acknowledges the request.
    - **ACK**: Client confirms the server's response.
2. **Relative SEQ and ACK Numbers**:
    
    - Wireshark displays **relative sequence numbers** for readability, starting from 0 in each session.
    - Absolute SEQ and ACK numbers are much larger and randomly initialized during the SYN phase.

---

#### **Normal TCP Behavior**:

- Sequential SYN, SYN/ACK, and ACK packets during connection establishment.
- Smooth data flow without excessive retransmissions or resets.

---

#### **Suspicious TCP Traffic**:

- **Excessive SYN packets**: Indicative of scanning or DoS attacks.
- **Unusual TCP flags**: Crafted packets with unexpected combinations of flags (e.g., FIN+URG).
- **Scanning Behavior**:
    - Single host attempting connections on multiple ports.
    - Multiple nodes targeted by a single host.
- **RST Flooding**: Abnormal reset (RST) packets may signal malicious activity.

---

#### **Examples in Wireshark**:

1. **SYN Flood Attack**:
    
    - Numerous SYN packets without corresponding SYN/ACK responses.
    - Indicates possible port scanning or denial-of-service attempts.
2. **SYN Scan**:
    
    - Sequence of SYN → SYN/ACK → RST packets.
    - Common in tools like Nmap for determining open ports.
3. **Ping Sweep with TCP**:
    
    - Combines ICMP Echo Requests and TCP SYN packets to ports like 80 and 443.
    - Timestamp Requests may follow, raising a red flag.

---


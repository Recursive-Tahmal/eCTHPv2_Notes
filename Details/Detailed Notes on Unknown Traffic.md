#### **Overview**

In network hunting, encountering unknown or custom protocols is not uncommon. These can include proprietary protocols used maliciously to bypass traditional monitoring mechanisms. Understanding and decoding unknown traffic is critical in identifying potential threats. For example, CrypMIC ransomware used a custom protocol over port 443 for communication with command-and-control (C2) servers.

---

#### **Example: Custom Protocol Analysis Using PCAP**

A proprietary protocol example is explained using a packet capture (PCAP) file titled _"Ann's Bad Aim"_ from a forensic contest. The PCAP contains AOL Instant Messenger (AIM) traffic, which can be analyzed to understand unknown traffic.

##### **Steps in Analysis:**

1. **Filtering for Specific Traffic**:
    
    - Focus on port 443 traffic. Note that traffic on port 443 should ideally be encrypted.
    - If Secure Sockets Layer (SSL) fields in the packet details are empty, it indicates non-SSL traffic.
2. **Identifying the Protocol**:
    
    - In this case, the AIM file transfer protocol (OSCAR) is identified. The traffic shows the "OFT2" signature at the start of the TCP stream, indicating OSCAR.
3. **Decoding the Protocol**:
    
    - Use **Wireshark’s "Decode As"** feature to interpret unknown traffic:
        - Navigate to `Analyze > Enable Protocols` to enable AIM-related dissectors.
        - Right-click on a packet, select **Decode As**, and assign the appropriate dissector (e.g., AIM dissector for OSCAR).
4. **Results of Decoding**:
    
    - Once decoded, packet details reveal information about the OSCAR protocol used on port 443.
    - This process helps understand how the proprietary protocol operates and uncovers valuable insights.

---

#### **Wireshark’s "Decode As" Feature**

Wireshark contains a vast library of protocol dissectors, each designed to interpret specific protocols. The "Decode As" feature allows temporary reassignment of dissectors to interpret custom or unknown traffic.

**Steps to Use "Decode As":**

- Right-click a packet and select **Decode As**.
- In the dialog, assign the appropriate dissector for the traffic.
- For AIM in the given PCAP, set parameters to enable AIM protocol interpretation.
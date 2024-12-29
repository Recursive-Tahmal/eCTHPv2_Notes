### ARP Traffic

**ARP (Address Resolution Protocol)** is a Layer 2 protocol that maps IP addresses to MAC addresses. It operates with two key message types:

1. **Request (Opcode 1)**: Device asks for the MAC address of a specific IP.
2. **Reply (Opcode 2)**: Device responds with the MAC address.

---

#### **Normal ARP Traffic**:

- ARP broadcasts from clients/servers and devices at reasonable flow levels.
- ARP Requests are usually followed by Responses.
- Legitimate **Gratuitous ARP** packets may occur (e.g., for network updates).

#### **Suspicious ARP Traffic**:

- A high volume of ARP broadcasts in a short time (e.g., scans).
- Duplicate MAC addresses with different IPs on the network.
- Gratuitous ARP packets crafted by attackers (used for ARP Poisoning).

---

#### **Gratuitous ARP in Attacks**:

- **Unsolicited ARP Replies**: Attackers send ARP Replies without requests to poison ARP tables.
- Prevent entry expiration by sending frequent replies (e.g., every 30 seconds).

---

#### **Examples in Packet Analysis**:

- **Normal ARP Traffic**:
    - Request: Device asks for the MAC of an IP it doesn’t know.
    - Reply: Responding device sends its MAC.
- **Suspicious ARP Traffic**:
    - Frequent ARP broadcasts or scanning behavior (e.g., sequential IP increments with short intervals).
    - Incremental packets from a rogue device may signal ARP scans or reconnaissance.

---

#### **Attack Implications**:

- ARP can be exploited for **Man-in-the-Middle Attacks** via ARP Poisoning.
- Attackers can conduct ARP scans to bypass restrictions like blocked pings or identify internal network structure.
### Detailed Notes on DHCP Traffic

**Dynamic Host Configuration Protocol (DHCP)** enables the automatic assignment of IP addresses and other network configurations to devices (clients) on a network. This simplifies network administration and ensures efficient allocation of IP addresses.

---

#### **How DHCP Works**

**DORA Process**  
The DHCP communication process follows four steps, collectively known as the **DORA process**:

1. **Discover**
    - The client broadcasts a DHCP Discover message to locate available DHCP servers on the network.
    - This message is sent to **UDP Port 67** on the server from **UDP Port 68** on the client.
2. **Offer**
    - The DHCP server replies with a DHCP Offer message, containing an available IP address and additional configuration parameters such as:
        - Subnet mask
        - Default gateway
        - Lease time
        - DNS server addresses
3. **Request**
    - The client responds with a DHCP Request message, confirming acceptance of the offered IP address and requesting additional configuration details if necessary.
4. **Acknowledgment (ACK)**
    - The DHCP server sends an Acknowledgment (ACK) to finalize the process. This message contains all configuration parameters assigned to the client, including the IP address and lease duration.

---

#### **Packet Breakdown**
- **DHCP Discover**:
    - **Purpose**: The client is requesting an IP address.
    - **Details**: The message is broadcast because the client doesn’t know the DHCP server's address.
- **DHCP Offer**:
    - **Purpose**: The server offers an IP address to the client.
    - **Details**: Contains additional info such as netmask, lease time, and the server’s IP address.
- **DHCP Request**:
    - **Purpose**: The client accepts the offered IP.
    - **Details**: The transaction ID ensures the response is linked to the original Discover request.
- **DHCP ACK**:
    - **Purpose**: The server confirms the assignment.
    - **Details**: Indicates successful completion of the DORA process.

---

#### **Normal DHCP Traffic**
- Sequential DORA process without interruptions.
- Proper use of **UDP Ports 67 and 68**.
- The DHCP server's IP address matches known or configured addresses.
- Lease times and network configurations align with organizational policies.

---

#### **Suspicious DHCP Traffic**
1. **Rogue DHCP Servers**
    - Unauthorized devices acting as DHCP servers.
    - Can assign IPs outside valid ranges or launch man-in-the-middle attacks by manipulating gateway/DNS configurations.
2. **Unexpected Offers**
    - Multiple DHCP Offers from unknown servers.
    - Offers with unexpected IP ranges or subnet masks.
3. **DHCP Starvation Attacks**
    - Flooding the network with DHCP Discover messages to exhaust available IP addresses.
4. **Anomalous Lease Times**
    - Extremely short or excessively long lease durations.

---

#### **Identifying Suspicious Traffic in Wireshark**

- Use **BOOTP filters** to analyze DHCP traffic:
    - `bootp.type == 1` for Discover
    - `bootp.type == 2` for Offer
    - `bootp.type == 3` for Request
    - `bootp.type == 5` for Acknowledgment
- **Look for Patterns**:
    - Multiple DHCP Offers from unknown servers.
    - Inconsistent configurations (e.g., incorrect subnet masks).
    - Repeated Discover messages indicating possible DHCP starvation.

---

#### **Summary**
Understanding normal and suspicious DHCP behavior is essential for network monitoring. Detecting rogue servers or unusual traffic patterns can prevent configuration issues and potential attacks. Implement DHCP logging and monitoring tools to gain visibility into these activities.
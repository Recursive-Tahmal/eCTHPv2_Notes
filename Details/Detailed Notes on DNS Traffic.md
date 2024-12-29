### DNS Traffic - Detailed Notes

**DNS (Domain Name System)** is a protocol used to resolve domain names to IP addresses. It works through a query-response mechanism, primarily using **UDP on port 53**. DNS traffic is critical for network operations, but malicious actors can exploit it for various attacks. Here's how to recognize both normal and suspicious DNS traffic.

---

#### **Normal DNS Traffic**:

- **Port**: DNS typically uses **UDP** on **port 53** for most queries.
- **Query-Response Flow**: DNS requests (queries) are sent from clients to DNS servers, and responses (answers) are sent back to the clients.
    - **Example**: A client queries a DNS server for `example.com`, and the server responds with the corresponding IP address.
- **Transaction ID**: Each query and its response share the same **transaction ID**. This helps the client correlate the response with the request.
- **Traffic Direction**: Traffic should flow from clients to DNS servers only. DNS servers should not receive traffic from unauthorized or non-DNS clients.

**Example**: A DNS query from a client for a domain like `example.com` would generate a response containing the domain's IP address, all using UDP on port 53.

---

#### **Suspicious DNS Traffic**:

1. **TCP/53**:
    
    - **Suspicious if TCP is used instead of UDP**: DNS typically uses UDP for efficiency. TCP is only used when the response exceeds the UDP packet size limit (512 bytes) or for **DNS zone transfers** (AXFR).
    - **Risk**: Attackers may use TCP to bypass detection or to transmit larger amounts of data covertly, such as in **DNS tunneling** attacks.
2. **Non-DNS Destinations**:
    
    - **Traffic going to non-DNS servers**: If DNS traffic is directed to servers not part of the DNS infrastructure, this could indicate **misuse or redirection**.
    - **Risk**: Attackers may redirect DNS queries to malicious servers to intercept, redirect, or manipulate traffic.
3. **Missing Responses or Queries**:
    
    - **Unanswered queries** or **queries without responses** could indicate network issues, DNS misconfigurations, or **DNS amplification attacks**.
    - **Risk**: An attacker could flood the network with DNS queries, hoping to overwhelm the server or extract information.
4. **Zone Transfers (AXFR)**:
    
    - **AXFR (DNS Zone Transfer)** is a legitimate method for copying DNS records from one server to another, but it’s typically restricted to authorized servers.
    - **Suspicious Behavior**: If an unauthorized server attempts an AXFR request, it may indicate an attacker trying to obtain a list of all domain names within the DNS server, which can be used for further attacks.
    - **Risk**: Unauthorized zone transfers can expose sensitive internal network information.

---

#### **DNS Zone Transfer (AXFR)**:

- **AXFR** (Authorized Zone Transfer) is used for replication between DNS servers. This should only happen between trusted servers.
- **Suspicious AXFR Traffic**:
    - If AXFR queries are detected from untrusted IPs, this could indicate an attempt to gather internal DNS records, revealing domain names, IP ranges, and potentially exploitable information.

**Example**: A DNS query for a zone transfer (AXFR) will typically result in a full list of domain records being returned from the DNS server. If this happens over TCP/53, and it is not part of the organization’s normal behavior, it should be considered suspicious.

---

#### **Threats**:

- **DNS Zone Transfers (AXFR)**: Attackers may exploit zone transfers to map out all DNS records within a domain. Once they have a list of domain names, they could launch targeted attacks, such as **phishing** or **domain spoofing**.
    
- **DNS Tunneling**: Attackers may use DNS queries and responses to send or receive malicious payloads covertly. This is known as **DNS tunneling**, where DNS packets are used to transfer data (even large amounts) in an encrypted or obfuscated form. It is difficult to detect since DNS traffic is typically allowed on firewalls.
    
- **Exfiltration via DNS**: Malicious actors may use DNS queries to exfiltrate sensitive data, encoding it within the domain name in DNS requests.
    

---

### **Summary Comparison**:

|**Normal DNS Traffic**|**Suspicious DNS Traffic**|
|---|---|
|UDP on port 53|TCP on port 53|
|Queries and responses with matching IDs|No responses or no queries|
|Traffic directed to DNS servers|Traffic directed to non-DNS servers|
|Queries followed by DNS responses|Missing responses or unexpected queries|
|No AXFR (zone transfers) from unauthorized servers|AXFR requests from unauthorized IPs|

### **Conclusion**:

It’s essential to monitor DNS traffic closely, as it can be abused for malicious purposes, such as DNS tunneling or unauthorized zone transfers. Regular network behavior should only show UDP traffic on port 53 directed to DNS servers, with a matching query-response flow. Anything outside this pattern, such as TCP on port 53 or AXFR requests, should be investigated promptly.
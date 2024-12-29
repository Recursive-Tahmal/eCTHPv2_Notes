The **Pyramid of Pain** is a concept introduced by David Bianco that illustrates the relative difficulty attackers face when defenders remove specific indicators of compromise (IOCs) or disrupt their operations. It has six levels, ranked from easiest to most challenging for adversaries.

---

1. **Hash Values:**
    - **Description:** Unique identifiers for malicious files (e.g., MD5, SHA256).
    - **Impact on Attackers:** Low.
    - **Why It’s Easy to Change:** Attackers can alter file contents slightly to generate a new hash with minimal effort.
    - **Example:** Blocking a known malware hash.

2. **IP Addresses:**
    - **Description:** Specific network locations attackers use for command and control (C2).
    - **Impact on Attackers:** Low.
    - **Why It’s Easy to Change:** IP addresses can be reassigned or attackers can use proxies or dynamic addresses.
    - **Example:** Blocking a malicious IP address.

3. **Domain Names:**
    - **Description:** Domains used by attackers for phishing or C2.
    - **Impact on Attackers:** Moderate.
    - **Why It’s Easy to Change:** While cheap and quick to register new domains, retooling to use them requires more effort than changing IPs.
    - **Example:** Blocking domains like "badsite.com".

4. **Network/Host Artifacts:**
    - **Description:** Evidence of attacker activity, such as registry keys, file paths, or network traffic patterns.
    - **Impact on Attackers:** High.
    - **Why It’s Hard to Change:** Attackers must alter their methods and tools to avoid detection, which can require significant retooling.
    - **Example:** Detecting malicious traffic patterns unique to an attacker’s C2 framework.

5. **Tools:**
    - **Description:** Software attackers use for exploitation (e.g., Mimikatz, Cobalt Strike).
    - **Impact on Attackers:** Higher.
    - **Why It’s Hard to Change:** Developing or acquiring new tools takes time, expertise, and resources.
    - **Example:** Detecting and blocking a known exploitation framework like Metasploit.

6. **Tactics, Techniques, and Procedures (TTPs):**
    - **Description:** The methods attackers use to achieve objectives (e.g., lateral movement, privilege escalation).
    - **Impact on Attackers:** Severe.
    - **Why It’s Hard to Change:** Altering TTPs requires attackers to rethink and redesign their overall strategy, which is time-consuming and resource-intensive.
    - **Example:** Detecting Pass-the-Hash attacks or unusual lateral movement techniques.

---

The higher the defender operates on the pyramid, the more effort it takes for attackers to adapt, making it increasingly disruptive to their operations.
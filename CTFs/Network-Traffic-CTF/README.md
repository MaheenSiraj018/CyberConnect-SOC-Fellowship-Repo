# Network Traffic CTF Writeup

This document contains the detailed writeup for two network traffic–based CTF challenges completed as part of the CyberConnect SOC Fellowship.

---

# CTF 1: Malicious PS Download

## Scenario

A user on a workstation clicked a phishing link, triggering an HTTP request that downloaded a malicious PowerShell script. The objective was to determine:

- Where to place the network tap to efficiently capture the web traffic.
- Identify the HTTP response containing the malicious PowerShell file.
- Extract the flag from the captured traffic.

---

## Tap Placement Analysis

To capture all HTTP/HTTPS traffic efficiently, the tap must be placed where all outbound web traffic passes.

Correct placement: **Web Proxy**

Reason:
- All internal devices forward their HTTP(S) requests to the web proxy.
- The proxy then initiates connections to external destinations.
- Placing the tap here ensures visibility of all inbound and outbound web traffic.

This is the most efficient location for monitoring web-based attacks.

---

## Traffic Analysis

After placing the tap on the Web Proxy:

1. Inspected captured HTTP traffic.
2. Filtered HTTP responses.
3. Identified the response containing the malicious PowerShell script.
4. Extracted the embedded flag from the response body.

---

## Flag

THM{FoundTheMalware}


---

## Key Learning

- Web proxies are strategic monitoring points for HTTP-based threats.
- Phishing attacks often deliver malware through direct HTTP responses.
- Packet inspection helps identify malicious payload delivery.

---

# CTF 2: DNS Infiltration

## Scenario

A compromised workstation was receiving malicious C2 (Command and Control) instructions via DNS TXT records.

Objective:

- Determine the optimal tap placement for DNS traffic.
- Identify the malicious DNS TXT record.
- Extract the flag from the DNS response.

---

## Tap Placement Analysis

Correct placement: **DNS Server**

Reason:
- The DNS server handles all external DNS queries and responses.
- All DNS traffic flows through this central point.
- Placing the tap here allows monitoring of all DNS communications.

This ensures visibility of potential DNS-based exfiltration or C2 activity.

---

## Traffic Analysis

After placing the tap on the DNS server:

1. Inspected DNS traffic.
2. Located DNS responses containing TXT records.
3. Identified suspicious DNS response data.

Packet Details:

- Source IP: 10.10.10.11  
- Destination IP: 192.168.0.2  
- Protocol: UDP  
- Source Port: 53  
- Application: DNS (Response)  
- Record Type: TXT  

Query:

c2.tryhackrne.thn (TXT/IN)

Response:

c2.tryhackrne.sthn (TXT/IN)
TTL: 2413
Data: THM{C2CommandFound}


The malicious C2 instruction was embedded inside the DNS TXT record.

---

## Flag

THM{C2CommandFound}


---

# Overall Key Takeaways

- Strategic tap placement is critical for effective network monitoring.
- Web proxies provide full visibility of HTTP-based attacks.
- DNS servers are ideal monitoring points for DNS tunneling and C2 activity.
- DNS TXT records are commonly abused for data exfiltration and command delivery.
- Network traffic analysis is essential for SOC analysts in detecting stealthy attacks.

---

These challenges reinforced the importance of understanding network architecture and identifying optimal monitoring points in a SOC environment.

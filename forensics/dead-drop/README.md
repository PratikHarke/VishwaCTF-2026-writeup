# 🧩 DNS Exfiltration Analysis — Meridian Biotech

## 📌 Challenge Overview
An insider at Meridian Biotech was suspected of exfiltrating sensitive research data over an 11-day period. The Security Operations Center (SOC) identified unusual DNS traffic originating from:

- Workstation: WS-114  
- IP Address: 10.0.1.114  

A 4-hour PCAP capture was provided for analysis. The attacker is believed to have used a covert DNS tunneling technique to bypass Data Loss Prevention (DLP) systems.

---

## 🎯 Objectives
- Analyze the provided PCAP file  
- Reconstruct the exfiltrated payload from DNS traffic  
- Identify the attacker’s signaling mechanism in the final UDP packet  

---

## 🔍 Step 1 – Initial Traffic Analysis
The PCAP was opened in Wireshark.

### Observations:
- High volume of DNS queries from 10.0.1.114  
- Repeated queries to a suspicious domain: `*.r3s.io`  
- Queries contain long, random-looking subdomains  
- No corresponding legitimate browsing behavior  

👉 This strongly indicates **DNS tunneling / covert channel usage**

---

## 🔍 Step 2 – Isolating DNS Traffic
Apply a display filter:

dns

Focus on:
- Query names (QNAME)  
- Repeated patterns  
- Subdomain length and structure  

---

## 🔍 Step 3 – Identifying Suspicious Patterns
Example DNS queries:

KZUX.r3s.io  
G2DX.r3s.io  
MFBV.r3s.io  
IRT3.r3s.io  

### Key Insight:
- Subdomains are uniform in structure  
- Use uppercase letters and digits (A–Z, 2–7)  
- No natural language meaning  

👉 This strongly matches **Base32 encoding characteristics**

---

## 🔍 Step 4 – Extracting the Encoded Data
Extract the leftmost labels from DNS queries:

KZUX G2DX MFBV IRT3 MRXH GX3U OVXG 4ZLM L5ZD G5RT GRWD GZC7 MJ4V 65DU NRPW C3TEL52GS3LJNZTX2  

⚠️ Note:
- One query (`C3X8EMJQ`) does not follow the pattern  
- Including it breaks decoding → identified as noise/decoy  

---

## 🔍 Step 5 – Decoding the Payload
Concatenate the extracted values and decode using Base32.

### Result:
VishwaCTF{dns_tunnel_r3v34l3d_by_ttl_and_timing}

---

## 🔍 Step 6 – Validating the Covert Channel
Why DNS?

- DNS traffic is rarely blocked  
- Often not deeply inspected  
- Subdomains can carry encoded data  
- Each query acts as a data packet  

👉 This confirms a **classic DNS tunneling attack**

---

## 🔍 Step 7 – Final UDP Packet Analysis
Challenge hint:
> “The data isn't hidden in the packet body.”

Inspection of the final packet:

- Protocol: UDP  
- Payload: `\x00\x00OKOKOKOK`  

👉 Clearly not meaningful → likely a decoy

---

## 🔍 Step 8 – Identifying the Signaling Mechanism
Focus shifts to packet metadata.

### Key Observation:
- Normal traffic from WS-114: TTL = 64  
- Final UDP packet: TTL = 79  

---

## 🚨 Conclusion on Signaling
The attacker used:

✅ **IP Header Field Manipulation (TTL-based signaling)**  
- TTL value intentionally altered  
- Acts as a covert signal  
- Not visible in payload inspection  
- Bypasses content-based detection systems  

---

## 🏁 Final Flag
VishwaCTF{dns_tunnel_r3v34l3d_by_ttl_and_timing}

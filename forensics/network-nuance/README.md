# 🧩 Network Nuance — Forensics Challenge

## 📌 Challenge Overview
The challenge provided a list of ICMP sequence numbers with the hint:

“The sequence numbers in the ICMP echo requests don't match the replies.”

This suggests that the **ICMP sequence number field** is being used as a covert channel to hide data.

---

## 🔍 Step 1 — Initial Analysis
We are given a list of sequence numbers:

186, 205, 215, 204, 219, 197, 167, 184, 170, 223, ...

### Observations:
- Values do not directly map to readable ASCII  
- Numbers fall within a consistent range  
- Pattern suggests transformation rather than randomness  

👉 Indicates encoded data within packet fields  

---

## 🧠 Step 2 — Identifying the Pattern
Testing simple transformations reveals:

186 → 'V' (ASCII 86)

Difference:
186 - 86 = 100  

👉 Pattern identified:

encoded_value = ascii_value + 100  
ascii_value = seq_num - 100  

---

## 🔍 Step 3 — Decoding the Data
Applying the transformation (seq_num - 100) to all sequence numbers reconstructs the hidden message.

---

## 🛠️ Decoding Script
seq_nums = [
    186, 205, 215, 204, 219, 197, 167, 184, 170, 223,
    178, 151, 216, 219, 148, 214, 207, 195, 180, 152,
    199, 207, 151, 216, 195, 172, 149, 200, 200, 151,
    210, 195, 186, 172, 225
]

decoded = ''.join(chr(x - 100) for x in seq_nums)
print(decoded)

---

## 🎯 Final Output
VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}

---

## 🏁 Final Flag
VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}

---

## 🧠 Key Concepts
- Covert channels using network protocol fields  
- ICMP sequence numbers as data carriers  
- Simple encoding via numeric offsets  

---

## 🔐 Key Takeaways
- Network protocol fields can be abused for hidden communication  
- Always analyze numeric patterns in packet captures  
- Simple transformations (like offsets) are common in CTF challenges  
- ICMP is frequently used for covert data exfiltration  

---

## 🔧 Tools Used
- Python  
- Wireshark (for context analysis)  

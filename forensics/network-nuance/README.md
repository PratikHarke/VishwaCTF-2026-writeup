# 🧩 Network Nuance — ICMP Covert Channel Analysis

## 📌 Challenge Overview

The challenge hinted at hidden data within ICMP traffic:

> *“The sequence numbers in the ICMP echo requests don't match the replies.”*

This suggested that the **ICMP sequence field was being used as a covert communication channel** instead of its intended purpose.

---

## 🎯 Objective

* Analyze ICMP packets
* Identify hidden encoding mechanism
* Extract and reconstruct the flag

---

## 🔍 Initial Analysis

From the packet capture, the ICMP sequence numbers were:

```
186, 205, 215, 204, 219, 197, 167, 184, 170, 223,
178, 151, 216, 219, 148, 214, 207, 195, ...
```

### Key Observation:

* Values were **too large for standard ASCII**
* But appeared **close to readable ASCII values**

This hinted at a **simple transformation or offset encoding**

---

## 🧠 Hypothesis

Testing differences:

```
186 → 'V' (ASCII 86)
Difference = +100
```

This suggested:

```
encoded_value = ascii_value + 100
ascii_value = seq_num - 100
```

---

## 🛠️ Decoding Process

We subtract **100** from each sequence number:

```python
seq_nums = [
    186, 205, 215, 204, 219, 197, 167, 184, 170, 223,
    178, 151, 216, 219, 148, 214, 207, 195
]

decoded = ''.join(chr(n - 100) for n in seq_nums)
print(decoded)
```

---

## 🧾 Output

```
VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}
```

---

## 🏁 Final Flag

```
VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}
```

---

## 🔬 Key Takeaways

* ICMP fields (like sequence numbers) can be abused for **covert data exfiltration**
* Always check for:

  * Unusual numeric patterns
  * Offsets or transformations
* Simple encoding schemes are often used to **evade detection**

---

## 🧠 Skills Demonstrated

* Network traffic analysis
* Covert channel identification
* Pattern recognition
* Python-based decoding

---

## 🛠️ Tools Used

* Wireshark
* Python

---

## 🚨 Real-World Relevance

Attackers frequently use **covert channels** to bypass:

* Firewalls
* IDS/IPS systems
* Data Loss Prevention (DLP) tools

ICMP, DNS, and HTTP headers are common vectors.

---

## ✨ Conclusion

This challenge demonstrates how **seemingly harmless protocol fields** can be weaponized for stealthy data transmission.

Understanding such techniques is crucial for both:

* Offensive security (red teaming)
* Defensive detection (SOC analysis)

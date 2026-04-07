# 🧩 Network Nuance

## 📌 Challenge Overview
The challenge provided a list of ICMP sequence numbers with the hint:

“The sequence numbers in the ICMP echo requests don't match the replies.”

This suggested the sequence number field was being used as a covert channel.

---

## 🔍 Analysis
The sequence numbers (186, 205, 215, …) did not directly map to ASCII but were close to readable values.

---

## 🧠 Approach
Observation:
186 → ASCII 'V' (86) → difference = +100

So:
encoded_value = ascii_value + 100  
ascii_value = seq_num - 100  

---

## 🛠️ Decoding
Applying (seq_num - 100) to all values reconstructs:

VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}

---

## 🧪 Verification Script
```python
seq_nums = [
    186, 205, 215, 204, 219, 197, 167, 184, 170, 223,
    178, 151, 216, 219, 148, 214, 207, 195, 180, 152,
    199, 207, 151, 216, 195, 172, 149, 200, 200, 151,
    210, 195, 186, 172, 225
]

decoded = ''.join(chr(x - 100) for x in seq_nums)
print(decoded)

✅ Final Flag

VishwaCTF{N3tw0rk_P4ck3t_H1dd3n_VH}

🧠 Key Takeaways
ICMP fields can be used as covert data channels
Look for numeric patterns in packet data
Simple transformations like offsets are common in CTFs

# 🧩 The Curator

## 📌 Challenge Overview
We are given:
- RSA parameters (n, e, c1, c2)
- 8 LCG outputs
- Encrypted flag
- A MAC (unused)
- Partial server code

Hint:
“The seed is hiding inside the key. The key is hiding inside the noise.”

---

## 🔍 Analysis

### RSA (Red Herring)
c1 = pow(session_key, e, n)  
c2 = pow(session_key ^ MAGIC, e, n)  

MAGIC = SHA256(seed)[:4]

The prime structure suggests a hidden weakness (shared prefix, embedded seed), but this is a **decoy**.

---

### LCG (Actual Weakness)
xₙ₊₁ = (A·xₙ + C) mod 2³²  

- First 8 outputs are known  
- Remaining outputs form keystream  
- Encryption uses XOR  

---

## 🧠 Approach

Recover LCG parameters using consecutive outputs:

A = (x₃ - x₂) · (x₂ - x₁)⁻¹ mod 2³²  
C = x₂ - A·x₁ mod 2³²  
seed = (x₁ - C) · A⁻¹ mod 2³²  

Recovered:
- A = 2072692183  
- C = 1916465311  
- seed = 651701731  

---

## ⚙️ Keystream Recovery

Rebuild LCG:

x = seed  
x = (A·x + C) mod 2³²  

Skip first 8 outputs.

---

## 🔑 Critical Insight

Only the least significant byte is used:

keystream_byte = output & 0xff  

---

## 🔓 Decryption

plaintext = ciphertext ⊕ keystream  

---

## 🧪 Solve Script

```python
M = 2**32
outs = [980887876, 699919547, 2058135724, 3888182547, 3474875028, 107192043, 215891708, 1328661059]

enc = bytes.fromhex("b2723f1b432adff7c2009fe0e7cf4f5c10a9bf6c1a38aa50b6645fa7725823f20ae8fd9787946c5135965f200b1fd2e7fb353c8287b821")

def inv(a, m):
    return pow(a, -1, m)

x1, x2, x3 = outs[0], outs[1], outs[2]

A = ((x3 - x2) * inv((x2 - x1) % M, M)) % M
C = (x2 - A * x1) % M
seed = ((x1 - C) * inv(A, M)) % M

x = seed
stream = []
for _ in range(100):
    x = (A * x + C) % M
    stream.append(x)

keystream = bytes(v & 0xff for v in stream[8:8+len(enc)])
pt = bytes(a ^ b for a, b in zip(enc, keystream))

print(pt.decode())


✅ Final Flag

VishwaCTF{s33ds_4r3_n3v3r_s4f3_1ns1d3_pr1m3s_4nd_n01s3}

🧠 Key Takeaways
LCG is predictable with few outputs
Even partial leakage (1 byte) is enough to break encryption
RSA complexity was a deliberate distraction
Always target the simplest vulnerability first
🧾 Note

A Base64 string in the challenge decodes to a fake flag:

VishwaCTF{y0u_f0und_th3_r34l_fl4g_n0t!}

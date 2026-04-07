# 🧩 The Lost Client

## 📌 Challenge Overview
The challenge involved extracting hidden clues from an apparently blank image and using OSINT to identify a reputed Indian institute.

---

## 🔍 Step 1: Initial Analysis
The provided image appeared blank, suggesting possible hidden data or embedded metadata.

Using:
strings Challenge\ 1.png

We extracted the following clues:
- Creator: “Tata and ………”
- Comment: “Its a generational business family”
- Title: “V!”

---

## 🧠 Step 2: Interpreting Clues
- “Tata and ………” → Points to the **Tata Group**
- “Generational business family” → Confirms a legacy business house

This leads to a prominent figure:
➡️ **Ratan Tata**

---

## 🔎 Step 3: Identifying the Institute
The challenge mentions:

> “ex-chairman of a reputed institute in India”

Research shows that Ratan Tata served as Chairman of the Board of Governors of:
➡️ **Indian Institute of Management Ahmedabad**

---

## 🎯 Step 4: Deriving the Flag
The flag requires the **initials of the institute (in capitals)**:

Indian Institute of Management Ahmedabad → **IIMA**

---

## ✅ Final Answer

VishwaCTF{IIMA}

---

## 🧠 Key Takeaways
- Always check **metadata** in image-based challenges
- Combine **technical analysis + OSINT reasoning**
- Use contextual clues to pivot toward real-world entities

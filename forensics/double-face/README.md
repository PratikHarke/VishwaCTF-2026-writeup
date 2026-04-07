# 🧩 Double Face — Forensics Challenge

## 📌 Challenge Overview
We are provided with a PNG file and the following hint:

"Two faces of a single soul. One shows the beauty, the other hides the truth behind a wall of secrets. The key lies in the past, or in a list of common memories. Password is in rockyou.txt."

The objective is to uncover the hidden flag in the format:

VishwaCTF{}

---

## 🔍 Step 1 — Initial Inspection
We start by examining the file:

file challenge.png

Output:
PNG image data, 10 x 10, 8-bit/color RGB

### Observation:
- Extremely small image (10x10 pixels)  
- Appears visually insignificant  
- Likely used as a carrier for hidden data  

---

## 🔍 Step 2 — Raw String Analysis
Next, we extract readable strings:

strings challenge.png

### Findings:
- Standard PNG chunks:
  - IHDR  
  - IDAT  
  - IEND  
- After IEND, unexpected data appears  
- Presence of:
  PK → ZIP magic signature  

👉 Confirms the file is not just an image

---

## 🧠 Key Observation
- Data exists beyond the PNG IEND marker  
- Indicates appended content  
- ZIP signature confirms embedded archive  

👉 This confirms a **PNG + ZIP polyglot file**

---

## 🔍 Step 3 — Inspecting Embedded Data
Within the appended section, readable content is visible:

secret.txt  
The flag is: VishwaCTF{D0ubl3_F4c3_P0lygl0t_S3cr3t}

👉 The flag is directly exposed in raw bytes without requiring extraction

---

## 🔍 Step 4 — Optional Extraction
The embedded ZIP can also be extracted:

unzip challenge.png

This would reveal:
- secret.txt containing the flag  

---

## 🎯 Final Flag
VishwaCTF{D0ubl3_F4c3_P0lygl0t_S3cr3t}

---

## 🧠 Key Concepts
- Polyglot files (multiple valid formats in one file)  
- Hidden data after file end markers  
- ZIP magic bytes (PK) for embedded archive detection  

---

## 🔐 Key Takeaways
- Always inspect raw data using tools like strings or hex dump  
- Look beyond file format boundaries (e.g., after IEND in PNG)  
- Polyglot files are common in CTF for hiding data  
- Wordlists like rockyou.txt may hint at extraction paths, even if not required here  

---

## 🔧 Tools Used
- file  
- strings  
- unzip (optional)  

---

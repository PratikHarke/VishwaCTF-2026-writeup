# 🧩 Deep in the Delve

## 📌 Challenge Overview
The challenge required identifying:
- A YC-backed compliance startup
- The Substack investigator who exposed it
- The COO’s Gmail address

Flag format:
VishwaCTF{StartupName_substackhandle_cooGmail}

---

## 🔍 Step 1: Identifying the Startup
The clue mentioned:

> YC-backed compliance startup exposed on Substack

Using targeted searches, we identified:
➡️ **Delve**

- Y Combinator-backed startup
- Focused on compliance automation
- Recently exposed in an online investigation

---

## 🔎 Step 2: Finding the Substack Investigator
Searching for the exposure led to a Substack article authored by:

➡️ **deepdelver**

- Anonymous investigator
- Published analysis of Delve’s practices

---

## 📧 Step 3: Extracting the COO’s Gmail
Within the Substack article:

- COO identified as **Selin Kocalar**
- Gmail address found in the article:

➡️ sskocalar@gmail.com

---

## 🎯 Step 4: Constructing the Flag

- Startup → Delve  
- Substack → deepdelver  
- Gmail → sskocalar@gmail.com  

Final flag:

VishwaCTF{Delve_deepdelver_sskocalar@gmail.com}

---

## ✅ Final Answer

VishwaCTF{Delve_deepdelver_sskocalar@gmail.com}

---

## 🧠 Key Takeaways
- OSINT often involves **multi-source correlation**
- Blogs/Substack can contain **valuable investigative data**
- Always pivot across:
  - Startup → Media → People → Contact info

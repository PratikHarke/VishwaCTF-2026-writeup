# 🧩 Palette Index Steganography

## 📌 Challenge Overview
The challenge hinted that the data is hidden in the **palette indices**, not in the visible image.

> “Sometimes the colors aren't the colors, but the indices that lead to them.”

---

## 🔍 Analysis
Using `exiftool`, the image was identified as an **indexed PNG** with a palette (PLTE chunk).

Instead of analyzing pixel data, we inspected the **palette table**.

---

## 🛠️ Approach
- Each palette entry is stored as `(R, G, B)`
- Observed that:
  - Green and Blue values were `0`
  - Only the **Red channel contained data**
- These Red values directly mapped to **ASCII characters**

---

## 🎯 Result
Extracting the Red values from the palette revealed:

VishwaCTF{P4l3tt3_1nd3x_S3cr3t}

---

## ✅ Flag

VishwaCTF{P4l3tt3_1nd3x_S3cr3t}

---

## 🧠 Key Takeaway
Indexed PNG images can hide data in the **palette (PLTE)** instead of pixel values.

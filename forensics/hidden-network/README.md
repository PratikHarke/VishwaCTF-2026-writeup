📌 Challenge Overview

We are given a .pcap file and asked to extract a flag in the format:

VishwaCTF{}

No direct hints were provided, so the challenge requires traffic analysis + hidden data detection.

🔍 Step 1 — Initial Analysis

We start by opening the PCAP file in Wireshark:

wireshark challenge.pcap
Observations:
The capture contains HTTP traffic
Several requests and responses are visible
No obvious flag in plain text

👉 This suggests the flag is hidden or obfuscated

🔍 Step 2 — Filtering Relevant Traffic

Apply a filter to isolate HTTP responses:

http

Then inspect:

HTTP response bodies
Unusual payloads
Long text responses
🔍 Step 3 — Export HTTP Objects

To analyze content more easily:

File → Export Objects → HTTP

Save all files and inspect them.

👉 At first glance, the content looks normal — no visible flag

🔍 Step 4 — Suspicious Behavior

While inspecting one of the HTTP responses:

The text appears normal
But copying the content into a text editor shows:
Odd spacing
Invisible characters

👉 This indicates possible steganography using invisible Unicode characters

🔍 Step 5 — Identifying Zero-Width Characters

Using tools like:

xxd
hexdump
Python script

We detect the presence of:

U+200B → Zero-width space
U+200C → Zero-width non-joiner

👉 These characters are invisible but can encode data

🔍 Step 6 — Extracting Hidden Data

We extract only the zero-width characters from the text.

Mapping:
U+200B → 0
U+200C → 1

This converts the hidden sequence into a binary string.

🔍 Step 7 — Binary to Text Conversion

After extracting the binary:

Group into 8-bit chunks
Convert each byte to ASCII

Example script:

data = open("extracted.txt", "r", encoding="utf-8").read()

binary = ""
for ch in data:
    if ch == "\u200b":
        binary += "0"
    elif ch == "\u200c":
        binary += "1"

# Convert binary to text
flag = ""
for i in range(0, len(binary), 8):
    byte = binary[i:i+8]
    flag += chr(int(byte, 2))

print(flag)
🎯 Final Output

After decoding:

VishwaCTF{H1DDN3TWRKK}
🏁 Final Flag
VishwaCTF{H1DDN3TWRKK}
💡 Key Takeaways
Hidden data may not always be visible — check for Unicode tricks
Zero-width characters are commonly used for:
Steganography
Data exfiltration
Always inspect:
Raw bytes
Encodings
Invisible characters
🔥 Tools Used
Wireshark
Python (for decoding)
Hex/encoding inspection tools

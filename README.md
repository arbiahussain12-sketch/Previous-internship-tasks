# cybersecurity-internship-tasks
This repository contains five cybersecurity tasks completed during my Prodigy Infotech internship. The projects demonstrate concepts such as network security, encryption, secure coding, and vulnerability analysis, showcasing practical knowledge and hands-on implementation.


⸻

✅ Task 01 — Caesar Cipher
Goal: Encrypt & decrypt a message using a fixed shift.
Predefined: message="HELLO PRODIGY", shift=3.
Output: Prints original, encrypted, and decrypted text.
# task01_caesar.py
def caesar_encrypt(text, shift):
    out = []
    for ch in text:
        if ch.isalpha():
            base = 65 if ch.isupper() else 97
            out.append(chr((ord(ch) - base + shift) % 26 + base))
        else:
            out.append(ch)
    return "".join(out)

def caesar_decrypt(text, shift):
    return caesar_encrypt(text, -shift)

# Predefined values
message = "HELLO PRODIGY"
shift = 3

enc = caesar_encrypt(message, shift)
dec = caesar_decrypt(enc, shift)

print("Original :", message)
print("Encrypted:", enc)
print("Decrypted:", dec)


🖼️ Task 02 — Image Encryption (Pixel Manipulation)
Goal: Encrypt & decrypt an image by shifting pixel values.
Predefined: input photo.jpg, key 100.
Files produced: encrypted_photo.jpg, decrypted_photo.jpg

Put a test image named photo.jpg in the same folder as the script.
# task02_image_encrypt.py
from PIL import Image

def encrypt_image(inp, outp, key=100):
    img = Image.open(inp).convert("RGB")
    px = img.load()
    for x in range(img.width):
        for y in range(img.height):
            r, g, b = px[x, y]
            px[x, y] = ((r + key) % 256, (g + key) % 256, (b + key) % 256)
    img.save(outp)
    print(f"Encrypted -> {outp}")

def decrypt_image(inp, outp, key=100):
    img = Image.open(inp).convert("RGB")
    px = img.load()
    for x in range(img.width):
        for y in range(img.height):
            r, g, b = px[x, y]
            px[x, y] = ((r - key) % 256, (g - key) % 256, (b - key) % 256)
    img.save(outp)
    print(f"Decrypted -> {outp}")

# Predefined flow
encrypt_image("photo.jpg", "encrypted_photo.jpg", key=100)
decrypt_image("encrypted_photo.jpg", "decrypted_photo.jpg", key=100)


📡 Task 03 — Network Packet Analyzer
Goal: Capture live packets and print source, destination & protocol.
Predefined: capture exactly 10 packets.
If it prints nothing, generate some traffic (open a website) while it runs.
python3 -m pip install scapy
sudo python3 task03_packet_analyzer.py
# enter your Mac password (no characters will show while typing—normal)
# task03_packet_analyzer.py
from scapy.all import sniff, IP

def pretty_proto(num):
    return {6:"TCP", 17:"UDP", 1:"ICMP"}.get(num, str(num))

def handle(packet):
    if IP in packet:
        ip = packet[IP]
        print(f"Src: {ip.src:>15}  ->  Dst: {ip.dst:>15} | Proto: {pretty_proto(ip.proto)}")

print("Starting capture (10 packets)...")
sniff(prn=handle, count=10, store=False)
print("Done.")



⸻

🎹 Task 04 — Keylogger (Demo, limited)

README (Task 04)

Goal: Capture a limited number of keystrokes and save to keylog.txt.
Predefined: stops after 50 key presses.
Ethics: For learning, on your own machine only.
Deps: pynput
python3 -m pip install pynput
python3 task04_keylogger.py
# task04_keylogger.py
from pynput.keyboard import Listener
from datetime import datetime

MAX_KEYS = 50
count = 0

def on_press(key):
    global count
    count += 1
    with open("keylog.txt", "a", encoding="utf-8") as f:
        f.write(f"{datetime.now().isoformat()}  {key}\n")
    if count >= MAX_KEYS:
        return False  # stop listener

print(f"Keylogger started (will stop after {MAX_KEYS} keys).")
with Listener(on_press=on_press) as listener:
    listener.join()
print("Keylogger stopped. Saved to keylog.txt")


🔑 Task 05 — Password Strength Checker

README (Task 05)

Goal: Rate password strength using simple rules.
Predefined: a list of sample passwords.
python3 task05_password_strength.py
# task05_password_strength.py
import re

def strength_label(pw):
    score = 0
    score += 1 if len(pw) >= 8 else 0
    score += 1 if re.search(r"[A-Z]", pw) else 0
    score += 1 if re.search(r"[a-z]", pw) else 0
    score += 1 if re.search(r"[0-9]", pw) else 0
    score += 1 if re.search(r"[@$!%*#?&]", pw) else 0

    if score <= 2:
        return "Weak ❌"
    elif score == 3:
        return "Medium ⚠️"
    else:
        return "Strong ✅"

passwords = ["hello123", "HelloWorld", "Hussain@123", "Arbia_2025!", "ProdigyInfotech1"]

for pw in passwords:
    print(f"{pw:20} -> {strength_label(pw)}")


prodigy-cybersecurity-tasks/
├── task01_caesar.py
├── task02_image_encrypt.py
├── task03_packet_analyzer.py
├── task04_keylogger.py
├── task05_password_strength.py
└── README.md

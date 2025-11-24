---
title: "Space Pirates"
author: "PlayerX"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough

TARGET = [
    0x5A,0x3D,0x5B,0x9C,0x98,0x73,0xAE,0x32,0x25,0x47,
    0x48,0x51,0x6C,0x71,0x3A,0x62,0xB8,0x7B,0x63,0x57,
    0x25,0x89,0x58,0xBF,0x78,0x34,0x98,0x71,0x68,0x59
]

XOR_KEY = [0x42, 0x73, 0x21, 0x69, 0x37]
MAGIC_ADD = 0x2A
FLAG_LEN = 30

### Step 1 (reverse): XOR with rotating key
for i in range(FLAG_LEN):
    buffer[i] ^= XOR_KEY[i % 5]

### Step 2 (reverse): swap adjacent bytes
for i in range(0, FLAG_LEN, 2):
    buffer[i], buffer[i+1] = buffer[i+1], buffer[i]

### Step 3 (reverse): subtract MAGIC_ADD modulo 256
buffer = [(b - MAGIC_ADD) % 256 for b in buffer]

### Step 4 (reverse): XOR with position
buffer = [(b ^ i) for i, b in enumerate(TARGET)]


flag = ''.join(chr(b) for b in buffer)
print(flag)
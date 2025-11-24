---
title: "Space Pirates 2"
author: "PlayerX"
layout: "single"
github: "https://pointerpointer.com/"
---


TARGET = [
    0x15, 0x5A, 0xAC, 0xF6, 0x36, 0x22, 0x3B, 0x52,
    0x6C, 0x4F, 0x90, 0xD9, 0x35, 0x63, 0xF8, 0x0E,
    0x02, 0x33, 0xB0, 0xF1, 0xB7, 0x69, 0x42, 0x67,
    0x25, 0xEA, 0x96, 0x63, 0x1B, 0xA7, 0x03, 0x0B
]

XOR_KEY = [0x7E, 0x33, 0x91, 0x4C, 0xA5]
ROTATION_PATTERN = [1, 3, 5, 7, 2, 4, 6]
MAGIC_SUB = 0x5D
CHUNK_SIZE = 5
LENGTH = 32

def rotate_right(byte, n):
    return ((byte >> n) | ((byte << (8 - n)) & 0xFF)) & 0xFF

### Step 1 reverse: XOR with XOR_KEY
buffer = [buffer[i] ^ XOR_KEY[i % 5] for i in range(LENGTH)]

### Step 2 reverse: rotate right by ROTATION_PATTERN[i % 7]
buffer = [rotate_right(buffer[i], ROTATION_PATTERN[i % 7]) for i in range(LENGTH)]

### Step 3 reverse: swap adjacent bytes
for i in range(0, LENGTH, 2):
    buffer[i], buffer[i+1] = buffer[i+1], buffer[i]

### Step 4 reverse: add MAGIC_SUB
buffer = [(b + MAGIC_SUB) % 256 for b in buffer]

### Step 5 reverse: reverse chunks of 5
for start in range(0, LENGTH, CHUNK_SIZE):
    end = start + CHUNK_SIZE
    buffer[start:end] = buffer[start:end][::-1]

### Step 6 reverse: XOR with position squared
buffer = [b ^ ((i*i) % 256) for i, b in enumerate(TARGET)]







Convert to string
flag = ''.join(chr(b) for b in buffer)
print("Treasure Map Input:", flag)
print("Length:", len(flag))
---
title: "Vibe Bank Vault"
author: "Phil"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough


### Core bug
vibe_hash only hashes payload[: len(payload) % 256] and then bcrypt truncates the input to 72 bytes. With the static salt, any two inputs that share the same first min(len(payload) % 256, 72) bytes collide. Lengths that are multiples of 256 hash the empty string.


### Level-by-level
- Level 1: The secret is 140 chars, but bcrypt only sees the first 72. The first 70 are leaked. Brute-forced the remaining 2 base62 chars (62^2 = 3844 options) with multiprocessing until `bcrypt.checkpw` matched the provided hash.

- Level 2: Pick two different strings that start with the given prefix and have lengths that are multiples of 256/512. The hash portion is empty in both cases, so the hashes match.

- Level 3: Target length 300–500 means the hash depends on `remainder = target_len % 256`, truncated to 72. Sent `"B"*min(remainder, 72)` (length different from the target) to collide.

- Level 4: Target is `C... + fire emojis`. When the total byte length exceeds 72, bcrypt only sees the first 72 bytes. Sent the target truncated to 72 bytes (all Cs plus as many full fire emojis as fit) so the hash matched while staying under the 72-byte input limit.

- Level 5: Let `r = (len(prefix) + admin_pw_len) % 256`. The hashed portion is `(prefix + "X"... )[:r]` (truncated again to 72 if needed). If `r < len(prefix)`, sent any string of length `r + 239` so the portion is just the first r bytes of the prefix. Otherwise sent `"X" * (r - len(prefix))` so the portion matches the admin’s prefix + X padding.


### Automation

`solve.py` implements the above logic and uses a parallel brute-force for level 1:
```python solve.py --workers 16```

Successful run returns the flag after clearing all five layers.


### Flag
```
PCTF{g00d_v1b3s_b4d_3ntropy_sync72_b4ck1ng}
```
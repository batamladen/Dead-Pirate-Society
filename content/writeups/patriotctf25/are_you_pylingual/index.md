---
title: "Are You Pylingual?"
author: "weed"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough

```
with open("output.txt") as f:
    data = f.read().strip()

start = data.find('[')
end = data.rfind(']') + 1
list_str = data[start:end]

nums = list(map(int, list_str[1:-1].split(", ")))
unsigned = [(x + 256) if x < 0 else x for x in nums]

# Try only keys 249 and 250
for key in [249, 250]:
    decoded = "".join(chr(b ^ key) for b in unsigned)

    print(f"Key {key} (0x{key:02x}):")
    print("=" * 80)

# Save to file
    with open(f"decodedkey{key}.txt", "w") as f:
        f.write(decoded)

# Show first 20 lines
    lines = decoded.split('\n')
    for i, line in enumerate(lines[:20]):
        print(f"{i:2}: {line}")

    print(f"\nSaved to: decodedkey{key}.txt")
    print("=" * 80)
    print()
```



```Copy
pctf{obFusc4ti0n_i5n't_EncRypt1oN}
```
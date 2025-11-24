---
title: "Matrix Reconstruction"
author: "Qi._"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough

Had to go different direction to get this one, so i had to run a python script. Where i had to use the cipher.txt n keystream_leaks.txt. The keystream_leak are the states, need to use some math.


### Step 1 - Open the cipher.txt

```
with open(r"File directory\cipher.txt", "rb") as f:
    ciphertext = f.read()
```

### Step 2 - Leaked states
```
states = [  2694419740,2430555337,3055882924,228605358,4055459295,676741477,1030306057,1320993926,2317712498,3680836913,1922319333,1836782265,1490734773,218490631,4065897775,3125259028,189241330,1710684784,2355890305,95797196,813001417,1021781706,3522243094,1603928614,
1122416469,4125638785,2423341845,3666529189,61609182,2391267942,
148130332,4246509548,3552866507,1487751530,1895017353,327726050,
4251037246,22647618,3958787364,227107204]
```


### Step 3 - Build keystream from lowest byte of each state

```
keystream_bytes = bytes([s & 0xff for s in states])
```

### Step 4 - Decrypt by XOR

```
plaintext = bytes([c ^ keystream_bytes[i % len(keystream_bytes)] for i, c in enumerate(ciphertext)])
```

### Step 5 - Print decrypted text
```
try:
    print("Decrypted flag:", plaintext.decode('ascii'))
except UnicodeDecodeError:
    print("Decrypted bytes contain non-ASCII characters. Raw bytes:")
    print(plaintext)
```
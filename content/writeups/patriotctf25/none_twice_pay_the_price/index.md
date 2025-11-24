---
title: "Nonce Twice, Pay the Price"
author: "Phil"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough


### Approach
- Parsed two ECDSA signatures that reused the same nonce k on secp256k1. With the formula k = (z1 - z2)/(s1 - s2) mod n and d = (s1*k - z1)/r mod n, recovered the private key.
- Confirmed the derived public key matched pub.pem, proving the key was correct.
- The ciphertext secret_blob.bin was a stream XOR: keystream blocks are SHA256(priv || counter) with a 4-byte big-endian counter starting at 0. XORing that keystream with the blob produced the flag.

### Reproduction
Run the provided solve script from the challenge directory:

```
from hashlib import sha256

def modinv(a: int, n: int) -> int:
    return pow(a, -1, n)


def derive_priv_from_reused_nonce():
    # Values taken from sig1.txt and sig2.txt
    r = int("288b415d6703ba7a2487681b10da092d991a2ef7d10de016daea4444523dc792", 16)
    s1 = int("fc00f6d1c8e93beb4c983104f1991e6d1951aa729004b7a1e841f29d12797f4", 16)
    s2 = int("693ee365dd7307a44fddbdd81c0059b5b5f7ef419beee7aaada3c37798e270c5", 16)
    z1 = int("9f9b697baa97445b19c6552e13b3a796ec9b76d6d95190a0c7fab01cce59b7fd", 16)
    z2 = int("465e2cf6b15b701b2d40cac239ab4d50388cd3e0ca54621cff58308f7c9a226b", 16)

    # secp256k1 group order
    n = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141

    k = (z1 - z2) * modinv(s1 - s2, n) % n
    d = (s1 * k - z1) * modinv(r, n) % n
    return k, d


def decrypt_blob(priv_key: int, blob_path: str) -> bytes:
    blob = bytearray(open(blob_path, "rb").read())

    # XOR stream derived from SHA256(priv || counter)
    key_bytes = priv_key.to_bytes(32, "big")
    keystream = bytearray()
    counter = 0
    while len(keystream) < len(blob):
        keystream.extend(sha256(key_bytes + counter.to_bytes(4, "big")).digest())
        counter += 1

    return bytes(b ^ keystream[i] for i, b in enumerate(blob))


if __name__ == "__main__":
    k, priv = derive_priv_from_reused_nonce()
    print(f"Recovered k = {k:#x}")
    print(f"Recovered priv = {priv:#x}")

    plaintext = decrypt_blob(priv, "secret_blob.bin")
    print("Plaintext:", plaintext.decode(errors="replace"))
```

### Output:

```
Recovered k = 0xbcaa6b25e44bf83b5fc38873820cc423c720160e3e3a9ad7f4f4dc2a371051fe
Recovered priv = 0x3d5d238dfd8ccd1472cd22f80e22ae57e9ad79d779f4630930efb5cc21977ce7
Plaintext: pctf{ecdsa_n0nc3_r3us7e_get!s_y0u8_0wn1ed}
```
---
title: "This Craft Is Mine"
author: "Skye"
layout: "single"
github: "https://pointerpointer.com/"
---

# This Craft Is Mine

1. I looked at the level.dat 

```bash
python3 -c "from nbt.nbt import NBTFile; f=NBTFile('level.dat'); print(f.pretty_tree())"
```

→ Didnt find anything relevant 

1. Looked through the stats and found a .json file 

![thiscraft1.webp](/images/material/thiscraft1.webp)

Here we see that blocks were placed, maybe we could find where they were placed. The suspicious blocks were white concrete and black concrete. To do that I used uNmINed, go to the Highlighters tab and click the + button, then entered the id: minecraft:black_concrete

![thiscraft2.webp](/images/material/thiscraft2.webp)

Now we need to extract the coordinates, they are written at the bottom on the b 

/tp 1613 77 1978

We found morse code 

![thiscraft3.webp](/images/material/thiscraft3.webp)

After looking at it more closely, since the dash are not the same size, this might be binary code 

![thiscraft4.webp](/images/material/thiscraft4.webp)

Whites are 1 and blacks are 0, I also had to reverse the binary code to get a readable answer (so reading from the end to start)

The sequence: 0100011000110001010101100011001101100101011011100011010101101001011000110111001100100001

Flag: `F1V3en5ics!`
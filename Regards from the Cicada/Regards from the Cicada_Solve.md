# 🔐 CTF Challenge — Organiser Guide

> **Internal use only. Do not distribute to participants.**

---

## Challenge Overview

| Field | Details |
|---|---|
| **Challenge Name** | The Cicada Shell |
| **Category** | Steganography |
| **Difficulty** | medium |
| **Flag** | `SECURINETSEPS{n0t_h3r3_but_4lw4ys_w4s}` |
| **Author** | 0XParanoia |
| **Files given to player** | `cicada_song.wav` |

---

## Challenge Description (shown to players)

> *Some secrets hide in plain sight. Others hide in sound, in rot, in hex, and in shells. Follow the trail.*

---

## Full Solve Chain

```
🎵 cicada_song.wav
    → Open in Audacity/Sonic Visualiser (spectrogram view)
        → ROT47 decode the visible string
            → https://imgur.com/gallery/red-is-way-d-FGIrvaw
                → Download cicada.png
                    → Decode ALL hex bytes visible in image
                        → "not here look deeper alot deeper"
                            → stegano red channel extract
                                → SECURINETSEPS{n0t_h3r3_but_4lw4ys_w4s} 🎉
```

---

## Stage 1 — Audio Spectrogram

**File:** `cicada_song.wav`

**What's hidden:**
The ROT47 encoded Imgur URL is embedded visually in the spectrogram.

**How to view:**
- Open in **Audacity** → View → Spectrogram
- Or use **Sonic Visualiser**
- Or use online tool: **spectrogram.online**

**Encoded string visible in spectrogram:**
```
9EEADi^^:>8FC]4@>^82==6CJ^C65\:D\H2J\5\uv=CG2H
```

**ROT47 decode:**
```
9EEADi^^:>8FC]4@>^82==6CJ^C65\:D\H2J\5\uv=CG2H
→ https://imgur.com/gallery/red-is-way-d-FGIrvaw
```

**Tools to decode ROT47:**
- CyberChef → ROT47
- [rot47.net](https://rot47.net)
- Python:
```python
text = "9EEADi^^:>8FC]4@>^82==6CJ^C65\:D\H2J\5\uv=CG2H"
print(''.join(chr(33 + (ord(c) - 33 + 47) % 94) if 33 <= ord(c) <= 126 else c for c in text))
```

---

## Stage 2 — The Cicada Image

**URL:** `https://imgur.com/gallery/red-is-way-d-FGIrvaw`

**What players see:**
A wooden cicada sculpture with visible text:
```
WOOPS
So close, yet you overlooked a hidden clue.
The patterns here are a cloak.
Some layers conceal, but this time,
the shell is just a shell.
```

And hex bytes visible in the bottom-right corner:
```
6e 6f 74 20 68 65 72 65 20
6c 6f 6f 6b 20 64 65 65 70
65 72 20 61 6f 66 74 20 64
65 65 70 65 72
```

**Step 1 — Decode the hex (common mistake: stopping early!)**
```
6e 6f 74 20 68 65 72 65 20 6c 6f 6f 6b 20 64 65 65 70 65 72 20 61 6f 66 74 20 64 65 65 70 65 72
→ "not here look deeper alot deeper"
```

> ⚠️ Red herring: players may stop at "not here look deeper" and dismiss it as flavor text. The full phrase is the key to the next step.

**Step 2 — Extract the hidden flag**

Download the image then run:
```bash
pip install stegano --break-system-packages
python3 -c "from stegano import red; print(red.reveal('cicada.png'))"
```

**Output:**
```
SECURINETSEPS{n0t_h3r3_but_4lw4ys_w4s}
```

---

## How the Flag Was Embedded

```bash
# Install stegano
pip install stegano --break-system-packages

# Embed flag in red channel
python3 -c "
from stegano import red
red.hide('cicada.png', 'SECURINETSEPS{n0t_h3r3_but_4lw4ys_w4s}').save('cicada.png')
"

# Verify
python3 -c "from stegano import red; print(red.reveal('cicada.png'))"
```

---
Player usual solve :
# Try standard LSB first (fails)
python3 -c "from stegano import lsb; print(lsb.reveal('cicada.png'))"

# Try red channel (works!)
python3 -c "from stegano import red; print(red.reveal('cicada.png'))"

## Red Herrings

| Misdirection | Reality |
|---|---|
| *"the shell is just a shell"* | Implies no steg — but there is |
| `"not here look deeper"` partial hex | Looks like flavor text — but it's the start of the key |
| The crumpled paper note in image | Purely decorative |
| Standard LSB steg tools (zsteg, steghide) | Won't find anything — red channel only |

---

## Difficulty Breakdown

| Stage | Trick | Why it's hard |
|---|---|---|
| Audio | ROT47 encoded URL in spectrogram | ROT47 less common than ROT13, encodes special chars too |
| Hex decode | Partial decode is a red herring | Players dismiss "not here" as flavor text |
| Stegano | Red channel instead of standard LSB | Common tools fail, need to know specific module |

---

## Hints (release progressively)

| Hint # | Cost | Text |
|---|---|---|
| 1 | 50pts | *"Not all rotations are equal."* |
| 2 | 100pts | *"Every byte in that image means something. Every. Single. One."* |
| 3 | 150pts | *"What color stands out in the image?"* |

---

## Flag

```
SECURINETSEPS{n0t_h3r3_but_4lw4ys_w4s}
```

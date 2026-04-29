
***

# Challenge Name: The Nine Days of Odin
**Category:** OSINT / Steganography / Cryptography  
**Difficulty:** Medium  
**Author:** 0XParanoia

## 📜 Challenge Description
Deep within the whispers of the Yggdrasil, a secret was buried. A traveler left behind an image of three giant swords and a cryptic poem detailing the Allfather's sacrifice. To claim the runes, you must follow his path: from a physical landmark in the mortal realm to the digital echoes of the void.

*"Where the 3 swords of the giants meet, count the realms... count the days... press them together through Yggdrasil's haze."*

## 📁 Provided Files
* `Unknown_Artifact.jpg`: An image of a famous landmark.
* `The Nine Days of Odin.txt`: A thematic poem containing lore and numerical hints.
* `SOLVE.txt`: A file containing a long string of Morse-like pulses (`.` and `-`) and a list of operations.
* `flags_wordlist.txt`: A custom wordlist for the final cracking stage.

---

## 🛠️ Solution Path

### Step 1: OSINT - Locating the Artifact
The image `Unknown_Artifact.jpg` shows three massive bronze swords. 
* **Search:** A reverse image search or a query for "three swords monument" identifies this as **Sverd i fjell** (Swords in Rock), however this almost cetaintly doesnt bring up ant results because i cropped the image in such a way its undetectable however i have provided both a hexadicimal hint in the metadata and also a hint in the user comment "social media might be the answer"
* **Location:** Hafrsfjord, near **Stavanger, Norway**.
* **Significance:** While the location provides context, the poem hints at specific numbers: **9** realms and **9** days.
**with all these hints now the player can search on insta for sverdifjell99 and find a post with encoded morse code this is the flag now the player would pass to the decoding step.**
**### Step 2: Decoding the Transmission
The instagram post ` contains a string of `.` and `-`. The player must decode this to binary first:

1.  **Binary Conversion:** Replace `.` with `0` and `-` with `1`.
2.  **ASCII Translation:** Convert the binary string into text. 
    * *Resulting String:* `k360m716177m4l794n3k8119144m2096nf7ob983291kn61p918o3572mfo474of03ok9095647m7kb065m6kknn4p4196nm8n9k422p8n84ok10k625k683n4n55201`

### Step 3: The ROT Cipher
The poem mentions "Nine realms... Nine days... Press them together." This is a hint for a **ROT10** shift (often derived from the "next step" after 9 or a combination of the numbers).
* **Action:** Apply **ROT10** (or `tr 'a-z' 'q-za-p'`) to the string from Step 2.
* **Result (Whirlpool Hash):** `a360c716177c4b794d3a8119144c2096df7eb983291ad61f918e3572cfe474ef03ea9095647c7ab065c6aadd4f4196dc8d9a422f8d84ea10a625a683d4d55201`

### Step 4: Cracking the Hash
The 128-character string is a **Whirlpool hash**. Using the provided `flags_wordlist.txt`, the player must perform a dictionary attack.

**Command:**
```bash
john --format=whirlpool --wordlist=flags_wordlist.txt hash.txt
```

---

## 🚩 Final Flag
The hash resolves to the first entry in the wordlist logic:
**`SECURINETSEPS{0din_And_1he_R8vens}`**

---

## 📝 Design Notes
* **Red Herring:** The Stavanger location is a classic OSINT hook but serves mainly to confirm the "Three Swords" (representing the 3 swords in the monument and the 3 steps of the puzzle).
* **Numerical Key:** The number 9 is repeated throughout the poem (9 realms, 9 days). The shift to 10 (ROT10) represents the "falling from the tree" (the 10th step/state).
* **Complexity:** The multi-layered encoding (Morse -> Binary -> ASCII -> ROT -> Hash) ensures that players cannot skip steps without understanding the logic.
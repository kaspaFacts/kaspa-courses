# Module 2: Hexadecimal Number System
<br>

## Learning Objective
Understand the **hexadecimal (base-16) number system**, how it relates to binary, and why computers use hex to represent data compactly.

---
<br>
<br>

## What is Hexadecimal?

### A Shortcut for Binary

**Hexadecimal** (often shortened to "hex") is a base-16 number system used as a human-friendly shorthand for binary.

| System | Base | Digits Used |
|--------|------|-------------|
| Decimal | 10 | `0, 1, 2, 3, 4, 5, 6, 7, 8, 9` |
| Binary | 2 | `0, 1` |
| **Hexadecimal** | **16** | `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F` |

<br>
<br>

### The 16 Hexadecimal Digits

Hex needs sixteen symbols. We use:
- **0–9** for values zero through nine (same as decimal)
- **A–F** for values ten through fifteen

| Hex Digit | Decimal Value |
|-----------|---------------|
| `0` | 0 |
| `1` | 1 |
| `2` | 2 |
| `3` | 3 |
| `4` | 4 |
| `5` | 5 |
| `6` | 6 |
| `7` | 7 |
| `8` | 8 |
| `9` | 9 |
| `A` | **10** |
| `B` | **11** |
| `C` | **12** |
| `D` | **13** |
| `E` | **14** |
| `F` | **15** |

<br>
<br>

### Why Letters for Numbers?

In any positional number system with base N, you need exactly **N distinct symbols**.

| Base | Symbols Needed | What We Use |
|------|----------------|-------------|
| Binary (base-2) | 2 symbols | `0`, `1` |
| Decimal (base-10) | 10 symbols | `0` through `9` |
| Hexadecimal (base-16) | **16 symbols** | `0`–`9` plus...? |

We already use all ten digits for values 0–9. To represent values 10–15 with single characters, we extend into the alphabet: A through F.

This is consistent with how humans naturally order symbols—we already sort letters alphabetically (A before B before C), so they work as ordered numeric values.

---
<br>
<br>

## Positional Notation in Hexadecimal

### Each Position is a Power of 16

Just like decimal uses powers of 10 and binary uses powers of 2:

**Example: Hex `1A3` to decimal**

```
    1   A   3
    │   │   └── 3 × 16⁰ = 3 × 1     = 3
    │   └────── 10 × 16¹ = 10 × 16  = 160  (A = 10)
    └────────── 1 × 16² = 1 × 256   = 256
                   
               Total: 256 + 160 + 3 = **419**
```

<br>
<br>

### Hex Place Values Table

| Position (from right) | Power of 16 | Value |
|-----------------------|-------------|-------|
| 1st | 16⁰ | 1 |
| 2nd | 16¹ | 16 |
| 3rd | 16² | 256 |
| 4th | 16³ | 4,096 |

<br>
<br>

## The Key Insight: Hex ↔ Binary Connection

### One Hex Digit = Exactly Four Bits

The connection between hexadecimal and binary is mathematical:

| Representation | Number of Values |
|----------------|------------------|
| 4 bits (binary) | 2⁴ = **16 values** (0–15) |
| 1 hex digit | **16 values** (0–F) |

Because both represent exactly 16 distinct values, there is a perfect one-to-one correspondence:
- Every group of 4 bits maps to exactly one hex digit
- Every hex digit expands to exactly 4 bits

<br>
<br>

### The Complete Mapping

| Binary | Decimal | Hex |
|--------|---------|-----|
| `0000` | 0 | `0` |
| `0001` | 1 | `1` |
| `0010` | 2 | `2` |
| `0011` | 3 | `3` |
| `0100` | 4 | `4` |
| `0101` | 5 | `5` |
| `0110` | 6 | `6` |
| `0111` | 7 | `7` |
| `1000` | 8 | `8` |
| `1001` | 9 | `9` |
| `1010` | 10 | `A` |
| `1011` | 11 | `B` |
| `1100` | 12 | `C` |
| `1101` | 13 | `D` |
| `1110` | 14 | `E` |
| `1111` | 15 | `F` |

<br>
<br>

## Converting Binary to Hexadecimal

### Group by Four Bits

**Example: Convert binary `10101101011` to hex**

```
Step 1: Pad with leading zeros to make groups of 4

     101 0110 1011 <- not groups of 4
    0101 0110 1011 <- leading zero added does not change value
    
Now: 010101101011 has 12 bits ✓ (divisible by 4)

Step 2: Group from right to left

    0101 0110 1011 <-

Step 3: Convert each group

    1011 → B (11)
    0110 → 6 (6)
    0101 → 5 (5)

Answer: Binary `010101101011` = Hex **56B**
```

<br>
<br>

### More Examples

| Binary | Groups of 4 | Hexadecimal |
|--------|-------------|-------------|
| `1010` | `1010` | `A` |
| `11110000` | `1111` `0000` | `F0` |
| `11111111` | `1111` `1111` | `FF` |
| `100000000` | `0001` `0000` `0000` | `100` |

<br>
<br>

## Converting Hexadecimal to Binary

### Expand Each Digit to Four Bits

**Example: Convert hex `2F` to binary**

```
Step 1: Convert each hex digit to its 4-bit binary equivalent

    For digit "2":
        2 in decimal = 0×8 + 0×4 + 1×2 + 0×1 = **0010**
    
    For digit "F" (which is 15):
        15 in decimal = 1×8 + 1×4 + 1×2 + 1×1 = **1111**

Step 2: Concatenate the 4-bit groups

    0010 1111 = 00101111

Answer: Hex `2F` = Binary **00101111** (or just `101111`)
```
**Note:** To **concatenate** means to join things together end-to-end. In computing, we concatenate strings or sequences by placing them side by side.

<br>
<br>

## Converting Hexadecimal to Decimal

### Use Powers of 16

**Example: Convert hex `2A` to decimal**

```
    2   A
    │   └── A × 16⁰ = 10 × 1 = 10
    └────── 2 × 16¹ = 2 × 16 = 32
            
            Total: 32 + 10 = **42**
```

<br>
<br>

### More Examples

| Hex | Calculation | Decimal |
|-----|-------------|----------|
| `10` | 1×16 + 0×1 | **16** |
| `1F` | 1×16 + 15×1 | **31** |
| `FF` | 15×16 + 15×1 | **255** |
| `100` | 1×256 + 0×16 + 0×1 | **256** |

<br>
<br>

## Why Hexadecimal is Useful

### Compact Representation of Binary Data

Compare how we write the same value:

| Format | Example Value | Characters Needed |
|--------|---------------|------------------|
| Decimal | `4,294,967,295` | 10 characters |
| Binary | `11111111111111111111111111111111` | 32 characters |
| **Hexadecimal** | `FFFFFFFF` | **8 characters** |

<br>
<br>

## Hexadecimal Notation in Code

### The `0x` Prefix

In programming, hexadecimal numbers are prefixed with **`0x`**:

| Written as | Value | Type |
|------------|-------|------|
| `42` | forty-two | Decimal |
| `0x42` | sixty-six (4×16 + 2) | Hexadecimal |
| `0xFF` | two hundred fifty-five | Hexadecimal |
| `0x10` | sixteen | Hexadecimal |

<br>
<br>

### Examples

```
0x0000000000B3F0B88B53ED975E28B8286741CE2AEE474267606BC7FE4D98A79A
0x814C2BAAE389B55FAAB19A6D687BCC5EDF40B13448A97BD4105EAC4356567866
0xDEADBEEF
```

The `0x` prefix tells you: "read the following digits as hexadecimal, not decimal."

---
<br>
<br>

## Summary

| Concept | Explanation |
|---------|-------------|
| **Hexadecimal** | Base-16 number system using digits 0-9 and letters A-F |
| **A-F values** | A=10, B=11, C=12, D=13, E=14, F=15 |
| **Positional notation** | Each position is a power of 16 (16⁰, 16¹, 16²...) |
| **Hex ↔ Binary** | One hex digit = exactly four bits (one nibble) |
| **Why use hex?** | Compact representation: 8 hex chars = 32 bits |
| **0x prefix** | Indicates a hexadecimal number in code |

---
<br>
<br>

## Check Your Understanding

**Question:** Convert the hexadecimal value `0xB4` to binary, then convert the hexadecimal value `0xB4` to decimal.

<details>
<summary>Click to see the answer.</summary>

**Answer:** 
<br><br>
**Hex to Binary**
```
B = 11 = 1011
4 = 4  = 0100

Result: 10110100
```
<br>

**Hex to Decimal**

From hex `B4`:
```
B × 16¹ + 4 × 16⁰ = 11 × 16 + 4 × 1 = 176 + 4 = **180**
```

</details>

---
<br>
<br>

## Practice Questions & Answers

**Q1: What does the hex digit `A` represent in decimal?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 10 (ten)

</details>

---

**Q2: What does the hex digit `F` represent in decimal?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 15 (fifteen)

</details>

---

**Q3: How many bits does one hexadecimal digit represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Four bits (also called a "nibble")

</details>

---

**Q4: What is the binary representation of hex digit `C`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `1100` (C = 12 = 8 + 4)

</details>

---

**Q5: What is the binary representation of hex digit `F`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `1111` (F = 15 = 8 + 4 + 2 + 1)

</details>

---

**Q6: Convert hex `0x10` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 16 (one × 16¹ + zero × 16⁰ = 16)

</details>

---

**Q7: Convert hex `0xFF` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 255 (fifteen × 16 + fifteen = 240 + 15 = 255)

</details>

---

**Q8: Convert hex `0x100` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 256 (one × 16² = 256)

</details>

---

**Q9: What is the maximum value representable with one hex digit?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** F (fifteen in decimal)

</details>

---

**Q10: What is the maximum value representable with two hex digits?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** FF (two hundred fifty-five in decimal) — same as one byte!

</details>

---

**Q11: Convert binary `1010` to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A (ten)

</details>

---

**Q12: Convert binary `1111` to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** F (fifteen)

</details>

---

**Q13: Convert binary `10101010` to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** AA (group as 1010 1010 → A A)

</details>

---

**Q14: Convert binary `11110000` to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** F0 (group as 1111 0000 → F 0)

</details>

---

**Q15: Convert hex `0x20` to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `00100000` or `100000` (2 = 0010, 0 = 0000)

</details>

---

**Q16: Convert hex `0xABC` to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `101010111100` (A=1010, B=1011, C=1100)

</details>

---

**Q17: What does the `0x` prefix indicate?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The following digits are in hexadecimal format

</details>

---

**Q18: How many hex digits are needed to represent one byte (8 bits)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Two hex digits (each represents 4 bits, so 2 × 4 = 8)

</details>

---

**Q19: How many hex digits are needed to represent 256 bits?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Sixty-four hex digits (256 ÷ 4 = 64)

</details>

---

**Q20: Convert decimal 255 to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** FF (255 = 15×16 + 15)

</details>

---

**Q21: Convert decimal 256 to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 100 (256 = 1×16² + 0×16¹ + 0×16⁰)

</details>

---

**Q22: Convert decimal 16 to hexadecimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 10 (sixteen in base-16 is written as "one-zero")

</details>

---

**Q23: What is hex `0xG`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Invalid! G is not a valid hexadecimal digit. Only 0-9 and A-F are allowed.

</details>

---

**Q24: Which is larger: hex `0x10` or hex `0xA`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 0x10 (sixteen) is larger than 0xA (ten)

</details>

---

**Q25: In hex, what comes after `0x9`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 0xA (just like in decimal, 10 comes after 9)

</details>

---

*Next: Module 3 will cover **undetermined**.*

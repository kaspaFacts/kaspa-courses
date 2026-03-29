# Module 1: What is a Bit and How Binary Works
<br>

## Learning Objective
Understand what a **bit** is, how the **binary number system** represents values using only 0s and 1s, and why computers use this system.

---
<br>
<br>

## What is a Bit?

### The Smallest Unit of Information

A **bit** (short for "binary digit") is the smallest unit of information a computer can store. It has exactly two possible values:

| Value | Meaning |
|-------|----------|
| `0` | Off, False, Low voltage |
| `1` | On, True, High voltage |

<br>
<br>

### Physical Representation

In a computer's hardware, bits are represented by physical states:

```
Voltage Level    Bit Value
───────────────────────────
High (e.g., 5V)     →      1
Low (e.g., 0V)      →      0
```

**Analogy:** Think of a light switch:
- Switch ON = `1`
- Switch OFF = `0`

<br>
<br>

### Why Only Two Values?

Computers use binary because it's **reliable** and **simple** to distinguish between two states (on/off) rather than ten (like our decimal system).

---
<br>
<br>

## The Binary Number System

### Decimal vs Binary

Our everyday number system is **decimal** (base-10), using ten digits:

| System | Base | Digits Used |
|--------|------|-------------|
| Decimal | 10 | `0, 1, 2, 3, 4, 5, 6, 7, 8, 9` |
| Binary | 2 | `0, 1` |

<br>
<br>

### Positional Notation in Decimal

In decimal, each position represents a **power of 10**:

```
    3456
    │││└─ 6 × 10⁰ = 6 × 1 = 6        (ones place)
    ││└── 5 × 10¹ = 5 × 10 = 50      (tens place)
    │└─── 4 × 10² = 4 × 100 = 400    (hundreds place)
    └──── 3 × 10³ = 3 × 1,000 = 3,000 (thousands place)
           
          Total: 3,000 + 400 + 50 + 6 = 3,456
```

<br>
<br>

### Positional Notation in Binary

In binary, each position represents a **power of 2**:

```
    10110
    ││││└─ 0 × 2⁰ = 0 × 1 = 0
    │││└── 1 × 2¹ = 1 × 2 = 2
    ││└─── 1 × 2² = 1 × 4 = 4
    │└──── 0 × 2³ = 0 × 8 = 0
    └───── 1 × 2⁴ = 1 × 16 = 16
           
          Total: 16 + 0 + 4 + 2 + 0 = 22 (in decimal)
```

<br>
<br>

### Binary Place Values Table

| Position (from right) | Power of 2 | Value |
|-----------------------|------------|-------|
| 1st | 2⁰ | 1 |
| 2nd | 2¹ | 2 |
| 3rd | 2² | 4 |
| 4th | 2³ | 8 |
| 5th | 2⁴ | 16 |
| 6th | 2⁵ | 32 |
| 7th | 2⁶ | 64 |
| 8th | 2⁷ | 128 |

<br>
<br>

## Converting Binary to Decimal

### Step-by-Step Method

**Example: Convert `1011` to decimal**

```
Step 1: Write the binary number with place values

    1   0   1   1    ← Binary digits
    │   │   │   └─ 2⁰ = 1
    │   │   └───── 2¹ = 2
    │   └───────── 2² = 4
    └───────────── 2³ = 8

Step 2: Multiply each bit by its place value

    1 × 8 = 8
    0 × 4 = 0
    1 × 2 = 2
    1 × 1 = 1

Step 3: Add the results

    8 + 0 + 2 + 1 = 11
```

**Answer:** Binary `1011` = Decimal **11**

<br>
<br>

### More Examples

| Binary | Calculation | Decimal |
|--------|-------------|----------|
| `1` | 1 × 2⁰ | **1** |
| `10` | 1×2¹ + 0×2⁰ = 2 + 0 | **2** |
| `11` | 1×2¹ + 1×2⁰ = 2 + 1 | **3** |
| `100` | 1×2² + 0×2¹ + 0×2⁰ = 4 + 0 + 0 | **4** |
| `1010` | 1×8 + 0×4 + 1×2 + 0×1 | **10** |
| `1111` | 8+4+2+1 | **15** |

<br>
<br>

## Converting Decimal to Binary

### Method: Repeated Division by 2

**Example: Convert decimal 13 to binary**

```
Divide by 2, track remainders:

    13 ÷ 2 = 6 remainder 1 ← Least significant bit (rightmost)
     6 ÷ 2 = 3 remainder 0
     3 ÷ 2 = 1 remainder 1
     1 ÷ 2 = 0 remainder 1 ← Most significant bit (leftmost)
    
    Read remainders bottom-to-top: 1101
```

**Answer:** Decimal **13** = Binary `1101`

<br>
<br>

### Verification

```
    1   1   0   1
    │   │   │   └─ 1 × 1 = 1
    │   │   └───── 0 × 2 = 0
    │   └───────── 1 × 4 = 4
    └───────────── 1 × 8 = 8
                
                Total: 8 + 4 + 0 + 1 = 13 ✓
```

<br>
<br>

## Visualizing Binary Numbers

### All 4-Bit Combinations (0-15)

| Decimal | Binary |
|---------|--------|
| 0 | `0000` |
| 1 | `0001` |
| 2 | `0010` |
| 3 | `0011` |
| 4 | `0100` |
| 5 | `0101` |
| 6 | `0110` |
| 7 | `0111` |
| 8 | `1000` |
| 9 | `1001` |
| 10 | `1010` |
| 11 | `1011` |
| 12 | `1100` |
| 13 | `1101` |
| 14 | `1110` |
| 15 | `1111` |

<br>
<br>

### Pattern Recognition

Notice the pattern when counting in binary:

```
    0000  (0)
    0001  (1)   ← Rightmost bit flips
    0010  (2)   ← Second bit flips, rightmost resets
    0011  (3)   ← Rightmost bit flips
    0100  (4)   ← Third bit flips, others reset
    0101  (5)
    0110  (6)
    0111  (7)
    1000  (8)   ← Fourth bit flips, all others reset
```

**Rule:** Each position toggles at half the frequency of the position to its right.

---
<br>
<br>

## Bytes and Larger Units

### What is a Byte?

A **byte** consists of exactly **8 bits**:

```
    1 byte = 8 bits

    [bit 7][bit 6][bit 5][bit 4][bit 3][bit 2][bit 1][bit 0]
    └─────── high nibble ──────┘└─────── low nibble ───────┘
```

<br>
<br>

### Byte Range

| Binary | Decimal |
|--------|----------|
| `00000000` | 0 (minimum) |
| `11111111` | 255 (maximum) |

**Calculation for maximum:**
```
1×128 + 1×64 + 1×32 + 1×16 + 1×8 + 1×4 + 1×2 + 1×1 = 255
```

<br>
<br>

### Common Units

| Unit | Bits | Bytes |
|------|------|-------|
| Bit | 1 | — |
| Byte | 8 | 1 |
| Kilobyte (KB) | 8,192 | 1,024 |
| Megabyte (MB) | 8,388,608 | 1,048,576 |

---
<br>
<br>

## Bit Positions and Counting

### How We Number Bit Positions

There are two common conventions for numbering bit positions:

**Convention A: Right-to-left (starting at 0)** — Most common in computing
```
    Binary:   1 0 1 1 0 0 0 1
    Position: 7 6 5 4 3 2 1 0  ← Zero-indexed from right
```

**Convention B: Right-to-left (starting at 1)** — Used in some documentation
```
    Binary:   1 0 1 1 0 0 0 1
    Position: 8 7 6 5 4 3 2 1  ← One-indexed from right
```

<br>
<br>

### Leading Zeros in Binary

**Leading zeros** are zeros at the left side of a binary number:

```
    With leading zeros:  00001011  (= 11)
    Without:             1011      (= 11, same value!)
```

---
<br>
<br>

## Summary

| Concept | Explanation |
|---------|-------------|
| **Bit** | Smallest unit of data; either `0` or `1` |
| **Binary System** | Base-2 number system using only digits 0 and 1 |
| **Positional Notation** | Each position represents a power (2ⁿ in binary, 10ⁿ in decimal) |
| **Byte** | 8 bits grouped together; range 0-255 |
| **Leading Zeros** | Zeros at the start of a binary number; don't change value |

---
<br>
<br>

## Check Your Understanding

**Question:** Convert the binary number `10110` to decimal, and identify the position of its highest '1' bit (using one-indexed counting from the right).

<details>
<summary>Click to see the answer.</summary>

**Answer:** 
<br><br>
**Converting to decimal:**
```
    1   0   1   1   0
    │   │   │   │   └─ 0 × 2⁰ = 0 × 1 = 0
    │   │   │   └───── 1 × 2¹ = 1 × 2 = 2
    │   │   └───────── 1 × 2² = 1 × 4 = 4
    │   └───────────── 0 × 2³ = 0 × 8 = 0
    └───────────────── 1 × 2⁴ = 1 × 16 = 16
                    
                    Total: 16 + 0 + 4 + 2 + 0 = **22**
```
<br>
**Highest '1' bit position:** The leftmost '1' is at the 5th position from the right, so the answer is **position 5**.

</details>

---
<br>
<br>

## Practice Questions & Answers

**Q1: What does "bit" stand for?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** "Binary digit"

</details>

---

**Q2: How many values can a single bit represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Two values: `0` and `1`

</details>

---

**Q3: What is the decimal value of binary `1`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 1 (one × 2⁰ = 1)

</details>

---

**Q4: What is the decimal value of binary `10`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 2 (one × 2¹ + zero × 2⁰ = 2 + 0 = 2)

</details>

---

**Q5: What is the decimal value of binary `11`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 3 (one × 2¹ + one × 2⁰ = 2 + 1 = 3)

</details>

---

**Q6: What is the decimal value of binary `100`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 4 (one × 2² + zero × 2¹ + zero × 2⁰ = 4 + 0 + 0 = 4)

</details>

---

**Q7: What is the decimal value of binary `101`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 5 (one × 2² + zero × 2¹ + one × 2⁰ = 4 + 0 + 1 = 5)

</details>

---

**Q8: What is the decimal value of binary `111`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 7 (one × 2² + one × 2¹ + one × 2⁰ = 4 + 2 + 1 = 7)

</details>

---

**Q9: What is the decimal value of binary `1000`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 8 (one × 2³ = 8)

</details>

---

**Q10: What is the maximum decimal value representable with 4 bits?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 15 (binary `1111` = 8+4+2+1 = 15)

</details>

---

**Q11: What is the maximum decimal value representable with 8 bits (one byte)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 255 (binary `11111111` = 128+64+32+16+8+4+2+1 = 255)

</details>

---

**Q12: How many bits are in one byte?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 8 bits

</details>

---

**Q13: What is the position of the highest '1' bit in binary `0001` (one-indexed from right)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Position 1 (the rightmost bit)

</details>

---

**Q14: What is the position of the highest '1' bit in binary `0100` (one-indexed from right)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Position 3

</details>

---

**Q15: What is the position of the highest '1' bit in binary `1000` (one-indexed from right)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Position 4

</details>

---

**Q16: Convert decimal 5 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `101` (4 + 1 = 5)

</details>

---

**Q17: Convert decimal 6 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `110` (4 + 2 = 6)

</details>

---

**Q18: Convert decimal 7 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `111` (4 + 2 + 1 = 7)

</details>

---

**Q19: Convert decimal 8 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `1000` (8 = 2³)

</details>

---

**Q20: Convert decimal 15 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `1111` (8 + 4 + 2 + 1 = 15)

</details>

---

**Q21: Convert decimal 16 to binary.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `10000` (16 = 2⁴)

</details>

---

**Q22: Does adding leading zeros change a binary number's value?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** No. Binary `00101` equals binary `101`, both equal decimal 5.

</details>

---

**Q23: Which has a higher value: binary `1000` or binary `0111`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Binary `1000` (8) is greater than binary `0111` (7).

</details>

---

**Q24: What is the position of the highest '1' bit in binary `00100000` (one-indexed from right)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Position 6

</details>

---

**Q25: Why do computers use binary instead of decimal?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** It's more reliable and simpler for hardware to distinguish between two states (on/off, high/low voltage) than ten different states.

</details>

---

**Q26: What is 2⁰ equal to?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 1 (any number raised to power 0 equals 1)

</details>

---

**Q27: If a binary number has its highest '1' bit at position 3 (one-indexed), what is the maximum possible value it could represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 7 (`111` = 4+2+1). With highest '1' at position 3, we can have at most bits set in positions 1, 2, and 3.

</details>

---

**Q28: If you have a binary number with exactly one '1' bit at position 5 (one-indexed), what is its decimal value?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 2⁴ = 16 (position 5 means 2^(5-1) = 2⁴)

</details>

---

**Q29: How many different values can be represented with n bits?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 2ⁿ different values (from 0 to 2ⁿ - 1)

</details>

---

**Q30: What is the binary representation of zero?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `0` or any number of zeros like `0000`

</details>

---

*Next: Module 02 will introduce hexadecimal notation and how it relates to binary.*

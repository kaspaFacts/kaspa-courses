# Module 3: Unsigned vs Signed Integers
<br>

## Learning Objective
Understand the difference between **unsigned** and **signed** integers, how computers represent negative numbers using two's complement, and why this matters for types like `Uint256`.

---
<br>
<br>

## What is an Integer?

### Whole Numbers Only

An **integer** is a whole number with no fractional part:

| Type | Examples |
|------|----------|
| Integers | `0`, `1`, `-5`, `42`, `-1000` |
| Not integers | `3.14`, `½`, `-0.5` |

<br>
<br>

### Two Categories of Integers

Integers fall into two categories:

```
                    ┌─────────────┬───────────────┐
                    │  Unsigned   │    Signed     │
                    │ (No sign)   │  (With sign)  │
                    ├─────────────┼───────────────┤
                    │ 0, 1, 2...  │ -2, -1, 0, +1 │
                    └─────────────┴───────────────┘
```

---
<br>
<br>

## Unsigned Integers

### Definition

**Unsigned integers** can only represent **non-negative values** (zero and positive numbers).

| Property | Value |
|----------|-------|
| Minimum value | `0` |
| Maximum value | Depends on bit width |
| Can store negatives? | ❌ No |

<br>
<br>

### All Bits Used for Magnitude

In an unsigned integer, **every bit contributes to the magnitude** (size) of the number:

```
8-bit Unsigned Integer:

     [bit 7][bit 6][bit 5][bit 4][bit 3][bit 2][bit 1][bit 0]
     └─────────── All 8 bits represent magnitude ───────────┘

Example: 01001101 = 64 + 8 + 4 + 1 = 77
```

<br>
<br>

### Unsigned Range by Bit Width

| Bits | Minimum | Maximum | Formula |
|------|---------|---------|----------|
| 8 | 0 | 255 | 2⁸ - 1 |
| 16 | 0 | 65,535 | 2¹⁶ - 1 |
| 32 | 0 | 4,294,967,295 | 2³² - 1 |
| 64 | 0 | 18,446,744,073,709,551,615 | 2⁶⁴ - 1 |
| 256 | 0 | Huge number! | 2²⁵⁶ - 1 |

<br>
<br>

### Common Unsigned Types in Programming

| Type Name | Bits | Range |
|-----------|------|-------|
| `u8` | 8 | 0 to 255 |
| `u16` | 16 | 0 to 65,535 |
| `u32` | 32 | 0 to ~4.3 billion |
| `u64` | 64 | 0 to ~18 quintillion |
| `Uint256` | 256 | 0 to an enormous number |

---
<br>
<br>

## Signed Integers

### Definition

**Signed integers** can represent **both positive and negative values**, plus zero.

| Property | Value |
|----------|-------|
| Minimum value | Negative (depends on bit width) |
| Maximum value | Positive (depends on bit width) |
| Can store negatives? | ✅ Yes |

<br>
<br>

### The Sign Bit Problem

If we want to represent negative numbers, we need a way to indicate the sign. One simple approach: use the **leftmost bit** as a sign indicator.

```
8-bit Signed Integer (Simple Approach):

    [sign][bit 6][bit 5][bit 4][bit 3][bit 2][bit 1][bit 0]
     │
     └─ Sign bit: 0 = positive, 1 = negative
```

<br>
<br>

### Two's Complement: The Standard Solution

Computers use **two's complement** to represent signed integers. This is the universal standard.

#### How Two's Complement Works

| Leftmost Bit | Meaning |
|--------------|----------|
| `0` | Positive number (or zero) |
| `1` | Negative number |

<br>
<br>

### Reading Positive Numbers in Two's Complement

If the leftmost bit is **0**, read it like a normal binary number:

```
8-bit Two's Complement Examples (Positive):

Binary      Left Bit    Decimal
────────────────────────────────
00000001     0          +1
00000100     0          +4
01000000     0          +64
01111111     0          +127 (maximum positive)
```

<br>
<br>

### Reading Negative Numbers in Two's Complement

If the leftmost bit is **1**, it represents a negative number.

#### Method to Find the Value:

**Step 1:** Invert all bits (0→1, 1→0)  
**Step 2:** Add 1  
**Step 3:** The result is the positive magnitude; add a minus sign

<br>
<br>

### Example: Converting `11111000` to Decimal

```
Binary: 11111000 (leftmost bit is 1, so it's negative)

Step 1: Invert all bits
    11111000 → 00000111

Step 2: Add 1
    00000111 + 1 = 00001000

Step 3: Read as positive, then add minus sign
    00001000 = 8
    Answer: **-8**
```

<br>
<br>

### Verification: Adding a Number and Its Negative

In two's complement, adding a number to its negative gives zero:

```
    +8:  00001000
   -8:  11111000
         ────────
        100000000

Result: 0 (with overflow bit discarded) ✓
```

---
<br>
<br>

## Signed Range by Bit Width

### The Asymmetric Range

In two's complement, the negative range is **one value larger** than the positive range:

| Bits | Minimum (Negative) | Maximum (Positive) |
|------|-------------------|--------------------|
| 8 | -128 | +127 |
| 16 | -32,768 | +32,767 |
| 32 | -2,147,483,648 | +2,147,483,647 |
| 64 | -9,223,372,036,854,775,808 | +9,223,372,036,854,775,807 |

<br>
<br>

### Why Is the Range Asymmetric?

Two's complement has **one more negative value** than positive values:

```
8-bit Two's Complement Range Visualization:

    -128  -127  ...   -1     0    +1  ...  +126  +127
     │      │          │     │    │          │    │
     └──────┴──────────┴─────┴────┴──────────┴────┘
                  ← 256 total values (2⁸) →

Negative side: 128 values (-128 to -1)
Zero:          1 value  (0)
Positive side: 127 values (+1 to +127)
```

<br>
<br>

### Why Does This Happen?

The leftmost bit in two's complement has a **negative weight**:

```
8-bit Two's Complement Weights:

    [bit 7][bit 6][bit 5][bit 4][bit 3][bit 2][bit 1][bit 0]
      -128    +64   +32   +16    +8     +4     +2     +1
      (Note: bit 7 has NEGATIVE weight!)
```

<br>
<br>

### Examples Using Negative Weight Method

**Example 1: `10000000` (minimum value)**
```
1 × (-128) + 0×64 + 0×32 + ... = **-128**
```

**Example 2: `11111111` (equals -1)**
```
1×(-128) + 1×64 + 1×32 + 1×16 + 1×8 + 1×4 + 1×2 + 1×1
= -128 + 64 + 32 + 16 + 8 + 4 + 2 + 1
= -128 + 127
= **-1**
```

<br>
<br>

## Common Signed Types in Programming

| Type Name | Bits | Range |
|-----------|------|-------|
| `i8` | 8 | -128 to +127 |
| `i16` | 16 | -32,768 to +32,767 |
| `i32` | 32 | ~-2.1 billion to ~+2.1 billion |
| `i64` | 64 | ~-9 quintillion to ~+9 quintillion |

---
<br>
<br>

## Comparison: Unsigned vs Signed

### Side-by-Side Comparison (8-bit)

```
UNSIGNED 8-BIT:
┌───────────────────────────────────────────────┐
│ 0                                            255 │
│ └──────────────── All values positive ─────────┘

SIGNED 8-BIT (Two's Complement):
┌────────────────────────────────────────────────┐
│ -128 ... -1 | 0 | +1 ... +127                  │
│   ← negative → │← positive →                    │
                └─ zero
```

<br>
<br>

### Key Differences Table

| Aspect | Unsigned | Signed (Two's Complement) |
|--------|----------|---------------------------|
| **Can store negatives?** | ❌ No | ✅ Yes |
| **Minimum value** | 0 | Negative number |
| **Uses leftmost bit for** | Magnitude | Sign AND magnitude |
| **8-bit max** | 255 | 127 |
| **Better for** | Counts, sizes, hashes | Temperatures, balances, differences |

<br>
<br>

### Same Binary Pattern, Different Interpretation

The same 8 bits can represent different values depending on interpretation:

| Binary | As Unsigned (u8) | As Signed (i8) |
|--------|------------------|----------------|
| `00000000` | 0 | 0 |
| `00000001` | 1 | 1 |
| `00001111` | 15 | 15 |
| `01111111` | 127 | 127 |
| `10000000` | **128** | **-128** |
| `10001000` | **136** | **-120** |
| `11111111` | **255** | **-1** |

---
<br>
<br>

## Why Two's Complement?

### Advantages Over Other Methods

Two's complement has several advantages:

| Advantage | Explanation |
|-----------|-------------|
| **Single representation of zero** | Only one way to write 0 (unlike sign-magnitude) |
| **Simple hardware** | Addition and subtraction use the same circuit |
| **No special cases** | Adding positive + negative works naturally |

<br>
<br>

### Example: Addition Works Naturally

```
In two's complement, these all work with the same addition:

  (+5) + (+3):    (-5) + (+3):
   00000101         11111011
 + 00000011       + 00000011
   ────────          ────────
   00001000          11111110
     = +8              = -2 ✓
```

---
<br>
<br>

## Uint256: The Blockchain Standard

### What is Uint256?

`Uint256` is an **unsigned 256-bit integer** type used extensively in blockchain and cryptography.

| Property | Value |
|----------|-------|
| Type | Unsigned integer |
| Bit width | 256 bits |
| Minimum value | 0 |
| Maximum value | 2²⁵⁶ - 1 |

<br>
<br>

### How Big is Uint256's Maximum?

```
2²⁵⁶ - 1 ≈ 1.16 × 10⁷⁷

That's a 1 followed by 77 zeros!

Written out in hexadecimal:
FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF
```

<br>
<br>

### Why Use Uint256 in Blockchain?

| Reason | Explanation |
|--------|-------------|
| **Hash outputs** | SHA-256 produces 256-bit values |
| **Large numbers** | Block counts, transaction IDs need huge ranges |
| **No negatives needed** | Counts and hashes are always non-negative |
| **Cryptographic security** | Large enough to prevent brute-force attacks |

<br>
<br>

### Uint256 in Rust Code

```rust
use rusty_kaspa::Uint256;

// Creating a Uint256 from hexadecimal string
let hash = Uint256::from_hex_str("0x814C2BAA...")?;

// The .bits() method returns how many bits are needed
// to represent the value (related to difficulty calculation)
let bit_count = hash.bits();
```

---
<br>
<br>

## Preview: Why This Matters for `parents_by_level`

The `.bits()` method on `Uint256` returns an **unsigned integer** representing the position of the highest '1' bit. Understanding unsigned vs signed is crucial because:

- The formula uses `max_block_level as i64 - pow.bits() as i64`
- This casts unsigned values to **signed** integers for subtraction
- The result can be negative (requiring clamping with `max(..., 0)`)

---
<br>
<br>

## Summary

| Concept | Explanation |
|---------|-------------|
| **Unsigned integer** | Non-negative only; all bits represent magnitude |
| **Signed integer** | Can be positive or negative; uses sign bit |
| **Two's complement** | Standard method for signed integers; leftmost bit has negative weight |
| **8-bit unsigned range** | 0 to 255 |
| **8-bit signed range** | -128 to +127 |
| **Uint256** | Unsigned 256-bit integer used in blockchain for hashes and large numbers |

---
<br>
<br>

## Check Your Understanding

**Question:** An 8-bit value has the binary representation `11100100`. What is its decimal value when interpreted as (a) unsigned, and (b) signed (two's complement)?

<details>
<summary>Click to see the answer.</summary>

**Answer:** 
<br><br>
**(a) As Unsigned:**
```
1×128 + 1×64 + 1×32 + 0×16 + 0×8 + 1×4 + 0×2 + 0×1
= 128 + 64 + 32 + 4
= **228**
```
<br>
**(b) As Signed (Two's Complement):**

Method 1 (invert and add):
```
Original: 11100100 (leftmost is 1, so negative)
Invert:   00011011
Add 1:    00011100 = 28
Answer: **-28**
```

Method 2 (negative weight):
```
1×(-128) + 1×64 + 1×32 + 0×16 + 0×8 + 1×4 + 0×2 + 0×1
= -128 + 64 + 32 + 4
= **-28**
```
</details>

---
<br>
<br>

## Practice Questions & Answers

**Q1: What does "unsigned" mean for an integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Unsigned means the integer can only represent non-negative values (zero and positive numbers). It cannot store negative numbers.

</details>

---

**Q2: What does "signed" mean for an integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Signed means the integer can represent both positive and negative values, plus zero.

</details>

---

**Q3: What is the maximum value of an 8-bit unsigned integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 255 (binary `11111111` = 2⁸ - 1)

</details>

---

**Q4: What is the maximum value of an 8-bit signed integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** +127 (binary `01111111`)

</details>

---

**Q5: What is the minimum value of an 8-bit signed integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** -128 (binary `10000000`)

</details>

---

**Q6: What is two's complement?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Two's complement is the standard method computers use to represent signed integers, where the leftmost bit indicates sign (0=positive, 1=negative) and has a negative weight.

</details>

---

**Q7: In two's complement, what does a leftmost bit of 0 indicate?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A positive number (or zero)

</details>

---

**Q8: In two's complement, what does a leftmost bit of 1 indicate?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A negative number

</details>

---

**Q9: Convert the signed 8-bit value `01000000` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** +64 (leftmost bit is 0, so it's positive; 2⁶ = 64)

</details>

---

**Q10: Convert the signed 8-bit value `10000000` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** -128 (this is the minimum representable value in 8-bit two's complement)

</details>

---

**Q11: Convert the signed 8-bit value `11111111` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** -1 (invert: 00000000, add 1: 00000001 = 1, so answer is -1)

</details>

---

**Q12: Convert the signed 8-bit value `10000001` to decimal.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** -127 (invert: 01111110, add 1: 01111111 = 127, so answer is -127)

</details>

---

**Q13: Why does an 8-bit signed integer have range -128 to +127 instead of -127 to +127?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** In two's complement, there is one more negative value than positive values because the leftmost bit has a negative weight of -2^(n-1). This allows representation of -128 but not +128 in 8-bit, creating the asymmetric range.

</details>

---

**Q14: What is the binary representation of -5 in 8-bit two's complement?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `11111011`

To find it:
- Start with +5: `00000101`
- Invert all bits: `11111010`
- Add 1: `11111011`

</details>

---

**Q15: What is the binary representation of -1 in any width two's complement?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** All 1s (e.g., `11111111` for 8-bit, `11111111111111111111111111111111` for 32-bit)

</details>

---

**Q16: What is the binary representation of 0 in two's complement?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** All 0s (e.g., `00000000` for 8-bit)

</details>

---

**Q17: What does the Rust type `u8` represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** An unsigned 8-bit integer with range 0 to 255.

</details>

---

**Q18: What does the Rust type `i8` represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A signed 8-bit integer with range -128 to +127.

</details>

---

**Q19: What does `Uint256` represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** An unsigned 256-bit integer, used extensively in blockchain for hashes and large numbers. Range is 0 to 2²⁵⁶ - 1.

</details>

---

**Q20: Why would you use an unsigned type instead of a signed type?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** When you only need non-negative values (like counts, sizes, memory addresses, or cryptographic hashes), unsigned types give you twice the positive range and make it clear that negative values don't make sense.

</details>

---

**Q21: Why would you use a signed type instead of an unsigned type?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** When you need to represent both positive and negative values (like temperature, financial balances, or differences between numbers).

</details>

---

**Q22: What is the maximum value of a 32-bit unsigned integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 4,294,967,295 (or 2³² - 1)

</details>

---

**Q23: What is the maximum value of a 32-bit signed integer?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 2,147,483,647 (or 2³¹ - 1)

</details>

---

**Q24: If you have binary `0111` interpreted as a 4-bit value, what is it as unsigned and as signed?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Both are +7. When the leftmost bit is 0, unsigned and signed interpretations give the same positive result.

</details>

---

**Q25: If you have binary `1000` interpreted as a 4-bit value, what is it as unsigned and as signed?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** As unsigned: +8. As signed (two's complement): -8.

</details>

---

**Q26: What happens when you add a number to its two's complement negative?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The result is zero (with possible overflow bit discarded).

</details>

---

**Q27: In the formula `max_block_level as i64 - pow.bits() as i64`, why cast to `i64`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The subtraction might produce a negative result, so we need signed integers (`i64`) rather than unsigned. This allows the formula to work correctly when `.bits()` is larger than `max_block_level`.

</details>

---

**Q28: How many different values can an 8-bit unsigned integer represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 256 different values (0 through 255)

</details>

---

**Q29: How many different values can an 8-bit signed integer represent?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Also 256 different values (-128 through +127)

</details>

---

**Q30: Which is larger: unsigned `u8` value 200 or signed `i8` value -57, when both have binary representation `11001000`?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** They're the same bits interpreted differently. As unsigned: 200. As signed: -56 (invert 11001000 → 00110111, add 1 → 00111000 = 56, so -56). The question is about interpretation, not comparison.

</details>

---

*Next: Module 4 will cover [next topic].*

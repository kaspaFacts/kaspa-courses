# Module 1: Understanding Block Level
<br>

## Learning Objective
Understand what **block level** is in Kaspa and how it relates to proof-of-work difficulty. This is essential to understanding `parents_by_level`, the 3rd field of the `block.header`.

---
<br>
<br>

## What is a Block Level?

**Block level** is derived from proof-of-work difficulty.

<br>
<br>

### The Formula

```rust
block_level = max(max_block_level - pow.bits(), 0)
```

Where:
- `max_block_level` = 250 (on mainnet)
- `pow.bits()` = **the minimum number of bits needed to represent the PoW hash as an integer**  
*(equivalently: the position of the highest '1' bit, counting from right starting at 1)*
<br>
<br>

### Connection to Mining

Miners search for nonces that satisfy a difficulty target, this produces hashes with **leading zeros**. The more leading zeros achieved, the higher the block level.

> 🔗 *See [Undetermined Title](../broken/link/mining-basics.md) for undetermined info.*
<br>
<br>

### What Does `pow.bits()` Actually Return?

The `.bits()` method returns **the position of the highest '1' bit in the hash, counting from the right** (where the rightmost bit = position 1).

| PoW Hash Value | Hex Example | Highest '1' Bit Position | `.bits()` Result |
|----------------|-------------|-------------------------|------------------|
| Very low value | `0x00...001` | Rightmost bit only | 1 |
| Low value | `0x00...0FF` | 8th bit from right | 8 |
| Medium value | `0x00...ABC` | ~100-200th bit | ~100-200 |
| High value | `0xFF...FFF` | Leftmost bit (full range) | 256 |
<br>
<br>

### Visualizing `pow.bits()` with Binary + Hex Examples

| Hash Representation | Description | Highest '1' Bit Position | `.bits()` Result | Block Level |
|---------------------|-------------|-------------------------|------------------|-------------|
| Binary: `00...001`<br>Hex: `0x00...001` | Only rightmost bit set | 1st from right | **1** | 250 - 1 = **249** |
| Binary: `00...00FF`<br>Hex: `0x00...0FF` | First 8 bits used | 8th from right | **8** | 250 - 8 = **242** |
| Binary: `00...ABCDEF`<br>Hex: `0x00...ABCDEF` | ~16 bytes of data | ~130th bit | **~130** | 250 - 130 = **~120** |
| Binary: `FF...FFFF`<br>Hex: `0xFF...FFFF` | All bits used (no leading zeros) | 256th bit | **256** | max(250-256,0) = **0** |

**Notice the pattern:** More leading zeros in binary → smaller `.bits()` value → higher block level

<br>
<br>

### The Inverse Relationship in One Flow

```
Miner finds hash with MORE leading zeros
                ↓
pow.bits() returns SMALLER number  
                ↓
(250 - smaller number) = LARGER result
                ↓
Block gets HIGHER level
```

**Example:** 
- Hash A: `0x00...0FF` → `.bits()` = 8 → level = **242** (high difficulty)
- Hash B: `0xFF...FFF` → `.bits()` = 256 → level = **0** (low difficulty)

<br>
<br>

### Key Insight: Difficulty vs Level

| Block Level | Difficulty Achieved | PoW Hash Characteristics |
|-------------|---------------------|--------------------------|
| **Level 250** | Maximum (Genesis) | Special-cased, no parents |
| **Level 180** | High | Many leading zeros, low hash value |
| **Level 125** | Middle | Moderate leading zeros |
| **Level 50** | Low-Fair | Fewer leading zeros |
| **Level 0** | Minimum/None | No leading zeros, high hash value |

**Higher level number = Higher difficulty achieved**

<br>
<br>

### Why the `max(..., 0)` Clamping?

The formula includes clamping to prevent negative block levels:

```rust
pub fn calc_level_from_pow(pow: Uint256, max_block_level: BlockLevel) -> BlockLevel {
    let signed_block_level = max_block_level as i64 - pow.bits() as i64;
    max(signed_block_level, 0) as BlockLevel  // Clamp to minimum of 0
}
```

**Example without clamping:**
- If `pow.bits()` returns 256 (hash with no leading zeros)
- Calculation: `250 - 256 = -6` ❌ Invalid!
- With clamping: `max(-6, 0) = 0` ✓ Valid minimum level

**Why this happens:** A PoW hash can use up to 256 bits (full range), but `max_block_level` is only 250. The subtraction can go negative, so we clamp to the valid range [0, 250].

<br>
<br>

### Why Subtract from Max?

The subtraction `max_block_level - pow.bits()` **inverts the relationship**:

| Without Subtraction | With Subtraction (`250 - bits`) |
|---------------------|---------------------------------|
| High difficulty → small `.bits()` (e.g., 10) | High difficulty → high level (250 - 10 = **240**) |
| Low difficulty → large `.bits()` (e.g., 250) | Low difficulty → low level (250 - 250 = **0**) |

**Design goal:** Genesis (maximum difficulty) sits at the **top** of the scale (level 250), not the bottom.

---
<br>
<br>

## Visualizing Levels as Difficulty Tiers

It might be helpful to visualize blocks achieving higher difficulty as taller than blocks achieving lower difficulty:

```
Level 250  ┌───────┐← Maximum difficulty (Genesis)
           │Genesis│
Level 180  │       │  ┌───────┐← High difficulty blocks
           │       │  │Block A│
Level 125  │       │  │       │  ┌───────┐← Medium difficulty blocks
           │       │  │       │  │Block B│
Level  50  │       │  │       │  │       │  ┌───────┐← Lower difficulty blocks
           │       │  │       │  │       │  │Block C│
Level   0  └───────┘  └───────┘  └───────┘  └───────┘← Lowest possible difficulty

```

**Key insight:** Higher level number = higher difficulty achieved. Genesis at level 250 is the maximum.

---
<br>
<br>

## The Genesis Block Special Case

The genesis block is unique:

```rust
if header.parents_by_level.is_empty() {
    return (max_block_level, true);  // Level 250, PoW always passes
}
```

**Properties:**
- **Level**: 250 (maximum level — origin/root of DAG)
- **Parents**: None (`parents_by_level` is empty)
- **PoW Check**: Skipped (always valid, special-cased with full difficulty)
- **Role**: Foundation of the entire DAG

---
<br>
<br>

## Preview: Why This Matters for `parents_by_level`
**Block Level** determines **where** each parent appears in the `parents_by_level` array. 

---
<br>
<br>

## Summary

| Concept | Explanation |
|---------|-------------|
| **Block Level** | Derived from PoW: `level = max(250 - pow.bits(), 0)` |
| **Level Range** | **0** (lowest difficulty) to **250** (maximum difficulty/Genesis) |
| **Genesis Block** | Always at level **250** with no parents (special-cased) |
| **Key Insight** | Higher level number = higher difficulty achieved |

---
<br>
<br>

## Check Your Understanding

**Question:** If Block X has `pow.bits() = 200` and Block Y has `pow.bits() = 50`, which block is at a higher level number?

<details>
<summary>Click to see the answer.</summary>

**Answer:** 
<br>Block X → `level = max(250 - 200, 0) = 50`.
<br>Block Y → `level = max(250 - 50, 0) = 200`. 
<br>Block Y achieved higher difficulty (smaller `.bits()`), so it has the higher level number (200 > 50).

</details>

---

## Practice Questions & Answers

**Q1: What is the formula for calculating block level?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `block_level = max(max_block_level - pow.bits(), 0)` where max_block_level = 250 on mainnet.

</details>

---

**Q2: If a block has `pow.bits() = 200`, what is its block level?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** `level = 250 - 200 = 50`

</details>

---

**Q3: What block level does the genesis block have?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Level 250 (special-cased as maximum difficulty/origin of DAG)

</details>

---

**Q4: Which has a higher level number: a high-difficulty block or a low-difficulty block?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A high-difficulty block has a higher level number (closer to 250).

</details>

---

**Q5: What is the range of possible block levels on mainnet?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 0 to 250 (inclusive)

</details>

---

**Q6: Does block level represent time or chronological order?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** No — block level represents difficulty tier, not time. Concurrent blocks can have different levels.

</details>

---

**Q7: Can two blocks exist at the same level simultaneously?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Yes — multiple blocks can be mined concurrently at the same level.

</details>

---

**Q8: What does `pow.bits()` measure?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The minimum number of bits needed to represent the PoW hash as an integer. A smaller `.bits()` value means more leading zeros and higher difficulty achieved.

</details>

---

**Q9: If Block A has level 10 and Block B has level 200, which achieved higher difficulty?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Block B (level 200) — higher level number = higher difficulty achieved.

</details>

---

**Q10: What is the maximum value of `max_block_level` on mainnet?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 250

</details>

---

**Q11: What does a higher block level number indicate?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A higher block level number indicates higher difficulty achieved — more leading zeros in the PoW hash.

</details>

---

**Q12: Where does genesis sit in terms of difficulty?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** At maximum difficulty (level 250), special-cased as the origin/root of the DAG.

</details>

---

**Q13: If you visualize levels as a spectrum, which end represents easier mining?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Level 0 (minimum difficulty) — requires fewer leading zeros in the hash.

</details>

---

**Q14: Does "higher difficulty" mean higher or lower level number?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Higher level number. Higher difficulty → higher number (closer to 250).

</details>

---

**Q15: A block at level 240 achieved more or less difficulty than a block at level 20?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** More difficulty — higher level numbers indicate greater difficulty achieved.

</details>

---

**Q16: What is the relationship between `.bits()` value and block level?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Inverse relationship. Smaller `.bits()` → higher difficulty → higher block level.

</details>

---

**Q17: If Block X is at level 5 and Block Y is at level 245, which achieved more difficult PoW?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Block Y (level 245) — higher level number means higher difficulty achieved.

</details>

---

**Q18: Why does genesis have level 250?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Genesis is special-cased with maximum difficulty. The code returns `(max_block_level, true)` directly when `parents_by_level.is_empty()`.

</details>

---

**Q19: What happens if you measure a block with no leading zeros in the hash?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The block would be at level 0 (minimum difficulty). Calculation: `max(250 - 256, 0) = 0`.

</details>

---

**Q20: What is the relationship between level number and difficulty achieved?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Direct relationship. Higher level number = higher difficulty achieved (more leading zeros).

</details>

---

**Q21: Block X has `pow.bits() = 50`, Block Y has `pow.bits() = 200`. Which achieved higher difficulty?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** 
Block Y → level = max(250 - 200, 0) = 50.
Block X → level = max(250 - 50, 0) = 200.

Block X (level 200) achieved higher difficulty than Block Y (level 50).

</details>

---

**Q22: If a block achieves the maximum possible `pow.bits()`, what level would it be at?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Level 0 (minimum difficulty — hash uses all 256 bits, no leading zeros)

</details>

---

**Q23: A block with `pow.bits() = 0` would be at what level?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Level 250 (`max(250 - 0, 0) = 250`) — maximum difficulty (theoretical minimum hash value).

</details>

---

**Q24:** Rank these blocks from highest to lowest difficulty achieved:
- Block A: level 180
- Block B: level 25
- Block C: level 100

<details>
<summary>Click to see the answer.</summary>

**Answer:** Highest → Lowest difficulty: Block A (180) > Block C (100) > Block B (25)

</details>

---

**Q25: If genesis is at level 250 and a new block is mined with minimal difficulty, approximately what level would it be?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Close to level 0 — minimal `pow.bits()` (close to 256) means low level number.

</details>

---

**Q26: True or False: A block at level 1 is "harder" (more difficult) than a block at level 10.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** False — higher level = higher difficulty achieved. Level 10 > Level 1 in difficulty.

</details>

---

**Q27: What would happen if you calculated `block_level = pow.bits()` directly (without subtracting from max)?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** The relationship would flip — high difficulty blocks (small `.bits()`) would have LOW numbers, which contradicts the current design where genesis (maximum difficulty) is at level 250.

</details>

---

**Q28: Why does the formula use `max(..., 0)` clamping?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** A PoW hash can have up to 256 bits (no leading zeros), but max_block_level is only 250. Without clamping: `250 - 256 = -6` would be invalid. Clamping ensures valid range [0, 250].

</details>

---

**Q29: What is the minimum possible `pow.bits()` value?**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Technically **0** (a hash of all zeros), resulting in level 250. The probability of achieving this through mining is astronomically low.

</details>

---

**Q30: Explain in one sentence why the formula uses subtraction (`max - pow`) rather than direct assignment.**

<details>
<summary>Click to see the answer.</summary>

**Answer:** Subtraction creates an inverse relationship where smaller `.bits()` (more leading zeros, higher difficulty) results in larger block level numbers, placing genesis at maximum level 250.

</details>

---

*Next: Module 2 will show *undetermined*.*

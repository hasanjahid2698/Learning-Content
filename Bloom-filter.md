![Bloom Filter Example](bloom-filters-poster.png)
# Bloom Filters – Introduction and Implementation
Suppose you are creating an account on Geekbook and want to enter a cool username. After several attempts, you get the message, “Username is already taken.” How does Geekbook check username availability so quickly from millions of registered usernames?  

### Ways to Check Username Availability:
1. **Linear Search**: Inefficient.
2. **Binary Search**: Store usernames alphabetically and compare step by step. Better but still requires multiple steps.
3. **Bloom Filter**: A space-optimized probabilistic data structure with potential false positives.

---

## What is a Bloom Filter?

A **Bloom Filter** is a space-efficient probabilistic data structure used to test set membership. For example, checking username availability is a set membership problem.  

### Key Features:
- Efficient representation of large sets.
- May produce **false positives** but never **false negatives**.
- Cannot delete elements (as it could affect other entries).

---

## Working of Bloom Filter

1. **Initial State**: A Bloom filter starts as a bit array of `m` bits, all set to zero.
2. **Adding Items**: Use `k` hash functions to set bits at specific indices.
3. **Checking Membership**: Verify if all hashed indices are set to `1`. If so, the item is "probably present"; if not, it is "definitely not present."

### Example:
- **Adding "geeks"**:
  ```
  h1("geeks") % 10 = 1
  h2("geeks") % 10 = 4
  h3("geeks") % 10 = 7
  ```
  Set bits at indices `1, 4, 7`.

- **Checking "geeks"**:
  Hash the item and confirm all corresponding bits are `1`.

---

## False Positives in Bloom Filters

A false positive occurs when:
- Bits set by other items overlap with the query item.
- Example: Checking "cat" when bits were already set by "geeks" and "nerds."

---

## Operations Supported by Bloom Filter:
1. **insert(x)**: Adds an element to the filter.
2. **lookup(x)**: Checks for element existence with a positive false probability.

---

## Calculations:
1. **Probability of False Positive (P)**:
   ```
   P = (1 - [1 - 1/m]^(kn))^k
   ```
2. **Bit Array Size (m)**:
   ```
   m = -(n * ln(P)) / (ln(2)^2)
   ```
3. **Number of Hash Functions (k)**:
   ```
   k = (m/n) * ln(2)
   ```

---

## Implementation

### Python Example
```python
# Python Bloom Filter Implementation
# Requires mmh3 and bitarray modules
import math
import mmh3
from bitarray import bitarray

class BloomFilter:
    def __init__(self, items_count, fp_prob):
        self.fp_prob = fp_prob
        self.size = self.get_size(items_count, fp_prob)
        self.hash_count = self.get_hash_count(self.size, items_count)
        self.bit_array = bitarray(self.size)
        self.bit_array.setall(0)

    def add(self, item):
        for i in range(self.hash_count):
            digest = mmh3.hash(item, i) % self.size
            self.bit_array[digest] = True

    def check(self, item):
        for i in range(self.hash_count):
            digest = mmh3.hash(item, i) % self.size
            if not self.bit_array[digest]:
                return False
        return True

    @classmethod
    def get_size(cls, n, p):
        return int(-(n * math.log(p)) / (math.log(2)**2))

    @classmethod
    def get_hash_count(cls, m, n):
        return int((m / n) * math.log(2))
```

## Applications of Bloom Filters:
1. **Medium**: Recommending posts by filtering already-seen content.
2. **Quora**: Filtering duplicate stories in feeds.
3. **Google Chrome**: Identifying malicious URLs.
4. **Databases**: Used in Google BigTable, Apache Cassandra, etc., to reduce unnecessary disk lookups.

---

## References:
- [geeksforgeeks: Bloom Filter](https://www.geeksforgeeks.org/bloom-filters-introduction-and-python-implementation)
- [Wikipedia: Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter)
- [Quora: Applications of Bloom Filters](https://www.quora.com/What-are-the-best-applications-of-Bloom-filters)

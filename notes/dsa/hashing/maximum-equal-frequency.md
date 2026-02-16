# Maximum-equal-frequency

## Concepts

Based on the video explanation, here are the formulas used to identify a valid prefix for LeetCode 1224, followed by a brute-force Python implementation.

### 1. The Formulas
To determine if a prefix of length $L$ is valid after removing exactly one element, the video identifies three scenarios. 

**Definitions:**
*   **$L$**: Current length of the prefix ($i + 1$).
*   **$max\_freq$**: The highest frequency count achieved by any number in the prefix.
*   **$F_{max}$**: The number of distinct elements that have the frequency $max\_freq$.
*   **$num\_distinct$**: The total number of distinct elements in the prefix.

---

#### Scenario 1: All Unique or Single Number
*   **All Unique:** Every number appears exactly once. Removing any one leaves the rest with frequency 1.
    *   **Formula:** $max\_freq = 1$
*   **Single Number:** Only one distinct number exists in the prefix. Removing one occurrence leaves only that number remaining.
    *   **Formula:** $num\_distinct = 1$

#### Scenario 2: The "Outlier is 1" Case
All elements have the same frequency ($M$), except for exactly one element that has a frequency of 1.
*   **Formula:** $(max\_freq \times (num\_distinct - 1)) + 1 = L$

#### Scenario 3: The "Outlier is Max" Case
All elements have the same frequency ($M$), except for exactly one element that appears $M+1$ times.
*   **Formula:** $((max\_freq - 1) \times num\_distinct) + 1 = L$

---

### 2. Brute Force Solution (Python)
The brute force approach iterates through every possible prefix length and, for each prefix, rebuilds the frequency map from scratch to check the conditions.

**Note:** As mentioned in the video, this is $O(N^2)$ and will likely result in a **Time Limit Exceeded (TLE)** on LeetCode, but it follows the basic logic explained.

```python
from collections import Counter

def maxEqualFreq_brute_force(nums):
    max_len = 0
    n = len(nums)
    
    # Check every prefix from length 2 to n
    for i in range(n):
        prefix = nums[:i+1]
        L = i + 1
        
        # 1. Count occurrences of each number in the prefix
        counts = Counter(prefix)
        num_distinct = len(counts)
        
        # 2. Find the maximum frequency in this prefix
        max_freq = max(counts.values())
        
        # 3. Find how many numbers have that max frequency
        f_max_count = sum(1 for freq in counts.values() if freq == max_freq)
        
        # Apply the 3 Scenario Formulas:
        
        # Scenario 1: All Unique or Only One Distinct Number
        if max_freq == 1 or num_distinct == 1:
            max_len = L
            
        # Scenario 2: Outlier is 1 
        # (One element has freq 1, others have max_freq)
        elif (max_freq * (num_distinct - 1)) + 1 == L:
            max_len = L
            
        # Scenario 3: Outlier is Max 
        # (One element has max_freq, others have max_freq - 1)
        elif ((max_freq - 1) * num_distinct) + 1 == L:
            max_len = L
            
    return max_len

# Example usage:
nums = [2, 2, 1, 1, 5, 3, 3, 5]
print(maxEqualFreq_brute_force(nums)) # Output: 7
```

### Why the optimized version is better:
In the optimized $O(N)$ version shown at the end of the video, you don't use `nums[:i+1]` or `Counter(prefix)` inside the loop. Instead, you update the `counts` and the `freq_of_freq` maps **incrementally** as you iterate through the array once.

## Commands

*No content*

## Notes

*No content*


---

*Created: February 16, 2026 at 02:13 PM*
*Updated: February 16, 2026 at 02:13 PM*

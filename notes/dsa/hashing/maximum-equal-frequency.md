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



To make the solution $O(N)$ and avoid the Time Limit Exceeded error, we must update the frequency counts **incrementally** as we iterate through the array. This allows us to check the three scenarios in constant time at each step.

### Optimized Python Solution

```python
from collections import defaultdict

def maxEqualFreq(nums):
    # count: maps element -> its frequency (how many times it appeared)
    count = defaultdict(int)
    # freq: maps frequency -> how many distinct elements have that frequency
    freq = defaultdict(int)
    
    max_freq = 0
    ans = 0
    distinct_elements = 0

    for i, x in enumerate(nums):
        # L is the current prefix length
        L = i + 1
        
        # 1. Update the state for element x
        if x in count:
            old_f = count[x]
            freq[old_f] -= 1
        else:
            distinct_elements += 1
            
        count[x] += 1
        new_f = count[x]
        freq[new_f] += 1
        
        # 2. Track the global maximum frequency
        max_freq = max(max_freq, new_f)
        
        # 3. Check the Scenarios using the Formulas
        possible = False
        
        # Scenario 1: All unique or only one distinct number type
        # e.g., [1, 2, 3, 4] or [5, 5, 5, 5]
        if max_freq == 1 or distinct_elements == 1:
            possible = True
            
        # Scenario 2: One outlier has frequency 1, others have max_freq
        # Formula: (max_freq * count_of_max_freq_elements) + 1 == Total Length
        # e.g., [2, 2, 1, 1, 5] -> (2 * 2) + 1 = 5
        elif (max_freq * freq[max_freq]) + 1 == L:
            possible = True
            
        # Scenario 3: One outlier has max_freq, others have max_freq - 1
        # Formula: (max_freq - 1) * (distinct_elements - 1) + max_freq == Total Length
        # Also requires that there is only ONE element with the max frequency.
        # e.g., [1, 1, 2, 2, 3, 3, 3] -> (2 * 2) + 3 = 7
        elif freq[max_freq] == 1 and ((max_freq - 1) * (distinct_elements - 1)) + max_freq == L:
            possible = True
            
        if possible:
            ans = L
            
    return ans

# Testing with an example
nums = [2, 2, 1, 1, 5, 3, 3, 5]
print(f"Longest prefix length: {maxEqualFreq(nums)}") 
# Output: 7 (Prefix [2, 2, 1, 1, 5, 3, 3])
```

### Key Optimizations Explained:
1.  **Single Pass ($O(N)$):** We traverse the list `nums` exactly once.
2.  **Incremental Updates:** Instead of re-calculating `max()` or `sum()` over the whole dictionary at every index, we update `count`, `freq`, and `max_freq` in $O(1)$ time per step.
3.  **Space Complexity ($O(D)$):** We store counts for $D$ distinct elements, which is at most $O(N)$.

### Why it passes:
The constraints are $N = 10^5$. 
*   **Brute Force:** $10^5 \times 10^5 = 10^{10}$ operations (Too slow).
*   **Optimized:** $10^5$ operations (Passes in ~50-100ms).

## Commands

*No content*

## Notes

*No content*


---

*Created: February 16, 2026 at 02:13 PM*
*Updated: February 16, 2026 at 02:15 PM*

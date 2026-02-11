# Boyce-Moore

## Concepts

Perfect Deepak 🔥
Now you’re thinking like someone building a **DSA Masterbook**.

I’ll create this as a **complete revision chapter** — structured, clean, repeatable.

You can revisit this anytime and rebuild the entire concept from scratch.

---

# 📘 Chapter: Boyer–Moore Voting Algorithm (Full Mastery Guide)

---

# 1️⃣ 🧠 Core Problem

Find element(s) that appear **more than a given fraction of n**.

Classic versions:

* Majority element (> n/2)
* Elements > n/3
* Elements > n/k

---

# 2️⃣ 🧠 Core Intuition (The Cancellation Theory)

Imagine numbers as political parties.

* Same numbers support each other (+1 vote)
* Different numbers cancel each other (-1 vote)

If one element appears **more than all others combined**,
it can never be fully cancelled.

That’s the whole secret.

---

# 3️⃣ ⚡ The Mathematical Backbone

If an element appears more than:

```
n/k
```

Then:

👉 There can be **at most k − 1 such elements**

### Proof

Assume k elements each appear more than n/k times.

Then:

```
c1 + c2 + ... + ck > k × (n/k)
c1 + c2 + ... + ck > n
```

Impossible.

So maximum = k−1.

---

# 4️⃣ 🥇 Case 1: Majority Element (> n/2)

## Why only 1 candidate?

Because:

If two elements both appear > n/2:

Their total would exceed n.

Impossible.

---

## Algorithm

Maintain:

```
candidate
count
```

### Rules

```
If count == 0 → set candidate
If num == candidate → count++
Else → count--
```

---

## Example

```
[2,2,1,1,1,2,2]
```

Final candidate = 2

---

## When Verification Needed?

* ❌ If majority NOT guaranteed
* ✅ If problem guarantees majority exists

---

# 5️⃣ 🥈 Case 2: Elements > n/3

Now we can have at most 2 elements.

So we track:

```
candidate1, count1
candidate2, count2
```

---

## Cancellation Logic

For each number:

1. If matches candidate1 → count1++
2. Else if matches candidate2 → count2++
3. Else if count1 == 0 → assign candidate1
4. Else if count2 == 0 → assign candidate2
5. Else → count1-- and count2--

Why decrement both?

Because we found 3 distinct elements → cancel them.

---

## Important

Always verify at end.

---

# 6️⃣ 🥉 Case 3: Elements > n/4

At most 3 elements.

Track:

```
3 candidates
3 counts
```

If all filled and new element different:

→ Decrement all 3 counts

This cancels 4 distinct elements.

---

# 7️⃣ 🧠 General Pattern

For:

```
> n/k
```

We:

* Track k−1 candidates
* Cancel k distinct numbers at a time
* Verify in final pass

---

# 8️⃣ 🧠 Why Verification Is Needed (Very Important)

Because cancellation phase only finds:

> Possible heavy hitters

It does NOT confirm actual frequency.

Example:

```
[1,2,3,4,5,6]
```

Algorithm still returns candidates.

But none satisfy condition.

So second pass is mandatory.

---

# 9️⃣ 🧩 Complexity

For n/k version:

Time → O(n)
Space → O(k)

For majority (> n/2):

Time → O(n)
Space → O(1)

---

# 🔟 ⚠️ Common Interview Traps

1. Forgetting verification pass
2. Using only 1 candidate for n/3
3. Not understanding why k−1 candidates
4. Not proving mathematically

---

# 1️⃣1️⃣ 🔥 Deep Intuition (Burn This Into Brain)

Boyer–Moore is NOT a counting algorithm.

It is a:

> Cancellation Algorithm based on mathematical dominance.

It works because:

```
Majority_count > sum_of_all_other_counts
```

---

# 1️⃣2️⃣ 🎯 Quick Recall Summary (Rapid Revision Block)

If interviewer says:

“Find elements appearing more than n/k times”

Immediately think:

* Max candidates = k−1
* Maintain k−1 counters
* Cancel when k distinct elements appear
* Verify at end

---

# 1️⃣3️⃣ 🧠 Practice Examples To Rebuild Logic

Try mentally:

1. `[1,1,2,2,3,3,1]` → > n/3?
2. `[4,4,4,4,2,2,3,3]` → > n/4?
3. `[1,2,3,1,2,1,2,1,2]` → > n/3?

Walk through cancellation manually.

---

# 1️⃣4️⃣ 💡 When Should You Use This Algorithm?

Use Boyer–Moore when:

* Problem mentions frequency threshold
* Constraint suggests O(1) space
* Hashmap would be too large
* You see pattern “more than n/k”

---

# 🧠 Final Mental Model

Think of it like this:

We eliminate balanced groups of k distinct elements.

Only elements with extra frequency survive.

---

Deepak — this chapter is now your permanent tool.

If you want, I can next:

* Add visual diagram version (for your visual DSA Masterbook style)
* Convert this into interview-ready template notes
* Or give you 3 progressive problems to lock it permanently

Your move, Cody student 🔥

## Commands

*No content*

## Notes

*No content*


---

*Created: February 11, 2026 at 03:30 PM*
*Updated: February 11, 2026 at 03:30 PM*

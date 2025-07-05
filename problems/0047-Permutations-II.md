# LeetCode 47 · Permutations II (Medium)

**Problem Statement**

Given a collection of numbers that might contain duplicates, return all possible unique permutations in any order.

---

## 🧠 Pattern Matching: Backtracking + Used Array + Deduplication

This problem is a textbook example of **"Try first, then check"** backtracking.

### Key Challenges:

* Duplicate numbers can lead to duplicate permutations.
* Need to ensure we skip generating structurally identical branches.

---

## 🔧 Python Solution

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()
        used = [False] * len(nums)

        def backtrack(path):
            if len(path) == len(nums):
                res.append(path[:])
                return

            for i in range(len(nums)):
                if used[i]:
                    continue
                if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                    continue

                used[i] = True
                path.append(nums[i])
                backtrack(path)
                path.pop()
                used[i] = False

        backtrack([])
        return res
```

---

## 👩‍💻 C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<bool> used;

    void backtrack(vector<int>& nums, vector<int>& path) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); ++i) {
            if (used[i]) continue;
            if (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;

            used[i] = true;
            path.push_back(nums[i]);
            backtrack(nums, path);
            path.pop_back();
            used[i] = false;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        used.resize(nums.size(), false);
        vector<int> path;
        backtrack(nums, path);
        return res;
    }
};
```

---

## 📊 Complexity

| Measure | Value                       |
| ------- | --------------------------- |
| Time    | O(N × N!) (pruned)          |
| Space   | O(N) recursion + used array |

---

## 🔍 Key Techniques

* **used\[] array**: tracks whether an element is currently in the path.
* **nums.sort()**: ensures duplicates are adjacent, enabling skipping.
* **duplicate skipping condition**: `i > 0 and nums[i] == nums[i-1] and not used[i-1]`.
* **backtrack flow**: try → recurse → pop → unmark.

---

## 🔄 Related Tags

`Backtracking` · `DFS` · `Permutation` · `Deduplication`

---

> This is a foundational backtracking problem. See `tags/backtracking.md` for generalized recursion structure.

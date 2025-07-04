# LeetCode 126 · Word Ladder II (Hard)

**Problem Statement**

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return all **shortest transformation sequences** from `beginWord` to `endWord`, such that:

- Only one letter can be changed at a time.
- Each transformed word must exist in the dictionary.
- Return all sequences in any order.

---

## 🧠 Core Concept

**Combine BFS and DFS**:

1. **BFS** to determine the minimum number of steps (levels) from `beginWord` to `endWord`, building a multi-parent mapping (`parents`).
2. **DFS** to backtrack from `endWord` to `beginWord` using the `parents` map, enumerating all shortest paths.

---

## 🔧 Solution Outline

### 1. BFS: Build the `parents` Map

- Initialize:

```python
wordSet = set(wordList)
if endWord not in wordSet:
    return []
parents = defaultdict(set)
queue = deque([beginWord])
found = False

```

- Layer-by-layer traversal:

```python
while queue and not found:
    next_level = set()
    for _ in range(len(queue)):
        word = queue.popleft()
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                candidate = word[:i] + c + word[i+1:]
                if candidate in wordSet:
                    if candidate == endWord:
                        found = True
                    next_level.add(candidate)
                    parents[candidate].add(word)
    queue = deque(next_level)
    wordSet -= next_level
```

- **Key Details**:
    - `next_level` is a **set** to avoid duplicate entries.
    - Once `endWord` is found in a level, BFS stops expanding further levels but finishes recording the current level.
    - Removing `next_level` from `wordSet` prevents revisiting nodes and ensures shortest paths.

### 2. DFS: Backtrack to Generate Paths

- Recursive backtracking:

```python
res = []
def dfs(current: str, path: List[str]):
    if current == beginWord:
        res.append(path[::-1])
        return
    for predecessor in parents[current]:
        dfs(predecessor, path + [predecessor])

if found:
    dfs(endWord, [endWord])
return res
```

- **Key Details**:
    - Each recursion step moves one level closer to `beginWord`.
    - Paths are reversed before addition to `res` to restore forward order.

---

## 📊 Complexity Analysis

| **Measure** | **Complexity** |
| --- | --- |
| Time | O(N × L² + P) |
| Space | O(N × L + P) |
- N = number of words in `wordList`
- L = length of each word
- P = total number of edges in the constructed parent graph

---

## 🔍 Key Insights & Best Practices

- **Multi-parent mapping**: A word may be reached from multiple predecessors; store all in `parents`.
- **Level control**: Stop BFS after the level where `endWord` is discovered to guarantee minimal path length.
- **Pruning**: Remove visited words (`next_level`) from `wordSet` immediately to avoid cycles and redundant work.
- **Separation of concerns**: BFS for distance, DFS for path recovery.

---

## 🧩 Related Problems for Practice

| ID | Title | Note |
| --- | --- | --- |
| 127 | Word Ladder I | Compute only the shortest path length |
| 433 | Minimum Genetic Mutation | Similar state-transition graph problem |
| 797 | All Paths From Source to Target | DFS-based path enumeration |
| 752 | Open the Lock | BFS on a state graph |

---

## 🔖 Tags

`Graph` · `BFS` · `DFS` · `Shortest Path` · `Backtracking` · `Path Recovery`

---

> Personal Note:
> 
> 
> This is a key milestone problem in my algorithm journey. I spent nearly two hours dissecting and validating every detail, and finally achieved a complete, deep understanding.
> 
> It taught me how to combine BFS for structure and DFS for solution recovery — a reusable pattern in many graph traversal problems.
>

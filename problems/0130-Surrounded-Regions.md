# LeetCode 130 · Surrounded Regions (Medium)

**Problem Statement**

Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`. A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

---

## 🧠 Pattern Matching

This problem is a classic **Grid + DFS/BFS traversal** problem with a twist:

- The solution requires a **reverse traversal from the border**.
- We mark `'O'`s that are **not** surrounded (i.e., connected to border) and leave them intact.
- All other `'O'`s will be turned into `'X'`.

**Key Idea**: Start DFS/BFS **from the borders** and mark all `'O'`s connected to the border with a placeholder (e.g., `'#'`).

---

## 🔧 Python DFS Solution

```
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        if not board: return
        rows, cols = len(board), len(board[0])

        def dfs(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != 'O':
                return
            board[r][c] = '#'  # Temporarily mark safe region
            for dr, dc in [(-1,0), (1,0), (0,-1), (0,1)]:
                dfs(r + dr, c + dc)

        # Start from border cells
        for r in range(rows):
            dfs(r, 0)
            dfs(r, cols - 1)
        for c in range(cols):
            dfs(0, c)
            dfs(rows - 1, c)

        # Post-process the board
        for r in range(rows):
            for c in range(cols):
                if board[r][c] == 'O':
                    board[r][c] = 'X'
                elif board[r][c] == '#':
                    board[r][c] = 'O'
```

---

## 👩‍💻 C++ DFS Solution

```
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        int rows = board.size();
        if (rows == 0) return;
        int cols = board[0].size();

        function<void(int, int)> dfs = [&](int r, int c) {
            if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] != 'O') return;
            board[r][c] = '#';
            vector<pair<int, int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
            for (auto [dr, dc] : directions) {
                dfs(r + dr, c + dc);
            }
        };

        for (int r = 0; r < rows; ++r) {
            dfs(r, 0);
            dfs(r, cols - 1);
        }
        for (int c = 0; c < cols; ++c) {
            dfs(0, c);
            dfs(rows - 1, c);
        }

        for (int r = 0; r < rows; ++r) {
            for (int c = 0; c < cols; ++c) {
                if (board[r][c] == 'O') board[r][c] = 'X';
                else if (board[r][c] == '#') board[r][c] = 'O';
            }
        }
    }
};
```

---

## 📊 Complexity Analysis

| Measure | Complexity |
| --- | --- |
| Time | O(m × n) |
| Space | O(m × n) (Recursion stack) |

---

## 🔍 Highlights & Takeaways

- **Grid traversal** via DFS is a core skill.
- **Reverse thinking**: Start from border instead of internal cells.
- In-place marking (`'#'`) is a common trick to distinguish regions.
- Can also be solved using **BFS with queue** if stack depth is a concern.

---

## 🌐 Related Grid Problems

| ID | Title | Comment |
| --- | --- | --- |
| 200 | Number of Islands | Traverse each land mass via DFS/BFS |
| 695 | Max Area of Island | Similar structure but with area count |
| 733 | Flood Fill | Coloring via DFS/BFS |
| 994 | Rotting Oranges | BFS + Level simulation |

---

> Example problem in grid traversal pattern. See tags/grid.md for the general strategy.
>

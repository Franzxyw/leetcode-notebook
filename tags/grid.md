## 🧠 Common Pattern: Grid Traversal (BFS/DFS)

This pattern appears in many classic grid-based problems such as:

- Number of Islands
- Surrounded Regions
- Flood Fill
- Pacific Atlantic Water Flow
- Closed Islands
- Shortest Path in Binary Matrix

### 🔁 Typical elements:

| Element | Description |
|--------|-------------|
| `dfs` / `bfs` | Used to traverse all connected reachable cells |
| `visited` matrix / in-place marking | To prevent re-visiting and infinite loops |
| 2D directional array | `directions = [(-1,0), (1,0), (0,-1), (0,1)]` or `[-1,0,1,0,-1]` |
| Boundary check | Always validate: `0 <= r < rows` and `0 <= c < cols` |
| Conditional check | Often: current cell is `'1'`, `'O'`, or is not visited |

### 🛠️ Common techniques:

- **Reverse from boundary**: Start BFS/DFS from borders (e.g., Surrounded Regions)
- **Mark first, recurse later**: Prevent early re-visits
- **Restore after traversal**: (e.g., change `'#'` back to `'O'`)

### Code Template (DFS)
Python 🐍
```python
def dfs(r, c):
    if not (0 <= r < rows and 0 <= c < cols): return
    if grid[r][c] != '1': return
    grid[r][c] = '#'  # mark as visited
    for dr, dc in directions:
        dfs(r + dr, c + dc)
```
C++ 🐱‍👤
```cpp
void dfs(int r, int c, vector<vector<char>>& grid, vector<vector<bool>>& visited) {
    int rows = grid.size(), cols = grid[0].size();
    if (r < 0 || r >= rows || c < 0 || c >= cols) return;
    if (grid[r][c] != '1' || visited[r][c]) return;

    visited[r][c] = true;
    // Or use: grid[r][c] = '#' if allowed to modify original grid

    vector<pair<int, int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    for (auto [dr, dc] : directions) {
        dfs(r + dr, c + dc, grid, visited);
    }
}
```

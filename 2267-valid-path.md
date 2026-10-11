# 2267. Check if There Is a Valid Parentheses String Path

**Approach:** DFS + Memoization (3D DP)

## Intuition

We need to find a path from the top-left cell to the bottom-right cell such that the parentheses along the path form a valid string.

We can move only **right or down**.

Instead of generating every possible path, we track the number of unmatched opening parentheses using `openCount`.

- If the current cell contains `(`, increment `openCount`.
- If it contains `)`, decrement `openCount`.
- If `openCount < 0`, the path is invalid because a closing parenthesis appears before a matching opening parenthesis.
- At the destination, the path is valid only if `openCount == 0`.

## Why This Approach?

**1. DFS:** Explore possible paths by moving down or right.

**2. Memoization:** Different paths can reach the same cell with the same `openCount`. We store the result to avoid solving the same state repeatedly.

**3. 3D DP:** The answer depends on three values:

- `i` — current row.
- `j` — current column.
- `openCount` — current unmatched opening parentheses.

Therefore, we use `t[i][j][openCount]`.

## Important Observations

- The total number of cells on a path is `m + n - 1`. It must be even because a valid parentheses string contains equal numbers of `(` and `)`.
- If the starting cell is `)` or the destination cell is `(`, a valid path is impossible.
- `openCount` must never become negative.
- The destination is valid only when `openCount == 0`.

## C++ Solution

```cpp
class Solution {
public:
    int m, n;
    int t[101][101][201];

    bool solve(int i, int j, int openCount,
               vector<vector<char>>& grid) {

        openCount += (grid[i][j] == '(') ? 1 : -1;

        if (openCount < 0) return false;

        if (t[i][j][openCount] != -1)
            return t[i][j][openCount];

        if (i == m - 1 && j == n - 1)
            return t[i][j][openCount] = (openCount == 0);

        // Move down
        if (i + 1 < m) {
            if (solve(i + 1, j, openCount, grid))
                return t[i][j][openCount] = true;
        }

        // Move right
        if (j + 1 < n) {
            if (solve(i, j + 1, openCount, grid))
                return t[i][j][openCount] = true;
        }

        return t[i][j][openCount] = false;
    }

    bool hasValidPath(vector<vector<char>>& grid) {
        m = grid.size();
        n = grid[0].size();

        if ((m + n - 1) % 2 == 1) return false;

        if (grid[0][0] == ')' || grid[m - 1][n - 1] == '(')
            return false;

        memset(t, -1, sizeof(t));

        return solve(0, 0, 0, grid);
    }
};
```

## Complexity Analysis

Let `m` be the number of rows and `n` be the number of columns.

- **Time Complexity:** `O(m × n × (m+n))` — each cell can be evaluated for multiple possible `openCount` values.
- **Space Complexity:** `O(m × n × (m+n))` for the memoization table, plus `O(m+n)` maximum recursion stack depth.

## Important Points to Remember

- DFS explores paths; memoization avoids repeated states.
- Always update `openCount` using the current cell before checking the state.
- A negative `openCount` means the path can never become valid.
- The state is not just `(i, j)`; it also depends on `openCount`.
- The grid has at most `m+n-1` cells along a path, so the balance dimension is bounded by the path length.

## Pattern to Remember

**Grid path + constraints + repeated states → DFS + Memoization**

When a grid problem asks whether a path exists and the answer depends on an accumulated value, include that value in the DP state.

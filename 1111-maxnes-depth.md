# 1111. Maximum Nesting Depth of Two Valid Parentheses Strings

**Approach:** Greedy + Depth Parity (Odd/Even)

## Intuition

We need to divide a valid parentheses string into two groups, `A` and `B`, so that the maximum nesting depth of both groups is minimized.

We can achieve this by assigning parentheses at **odd and even nesting depths to different groups**.

- Group `0` represents one group.
- Group `1` represents the other group.
- Alternate groups based on the current nesting depth.

This distributes nested parentheses between the two groups.

## Why This Approach?

The key observation is that nested parentheses have alternating depths.

For example, in `(()())`:

- The outermost pair has depth `1`.
- The inner pairs have depth `2`.

If we assign odd depths to group `1` and even depths to group `0`, deeply nested parentheses are distributed between the groups.

This greedy approach minimizes the maximum depth of the two resulting groups.

## C++ Solution

```cpp
class Solution {
public:
    vector<int> maxDepthAfterSplit(string seq) {
        vector<int> result(seq.size());
        int d = 0;

        for (int i = 0; i < seq.size(); i++) {
            if (seq[i] == '(') {
                d++;
                result[i] = d % 2 == 0 ? 0 : 1;
            } 
            else {
                result[i] = d % 2 == 0 ? 0 : 1;
                d--;
            }
        }

        return result;
    }
};
```

## Dry Run

Input: `seq = "(()())"`

| Index | Character | Depth `d` | Group |
|---:|:---:|---:|---:|
| 0 | `(` | 1 | 1 |
| 1 | `(` | 2 | 0 |
| 2 | `)` | 2 | 0 |
| 3 | `(` | 2 | 0 |
| 4 | `)` | 2 | 0 |
| 5 | `)` | 1 | 1 |

Output: `[1, 0, 0, 0, 0, 1]`

**Note:** The closing parenthesis must be assigned using the current depth before decrementing `d`. This ensures that each opening and closing parenthesis of a pair belongs to the same group.

## Complexity Analysis

- **Time Complexity:** `O(n)` — traverse the string once.
- **Space Complexity:** `O(n)` — store one group assignment per character in `result`.

## Important Points

- `d` tracks the current nesting depth.
- Increment `d` before assigning an opening parenthesis.
- Assign a closing parenthesis before decrementing `d`.
- Odd depth → group `1`; even depth → group `0`.
- Both groups remain valid parentheses sequences.

## Pattern to Remember

**Minimize maximum nesting depth across two groups → Greedy + Depth Parity**

When a problem asks you to distribute nested parentheses between two groups, alternating assignments by depth is an efficient strategy.


# 1614. Maximum Nesting Depth of the Parentheses

**Approach:** Counter / Greedy Scan

## Intuition

We need to find the maximum number of parentheses nested inside each other.

Instead of using a stack, we can maintain a counter `openBrackets` to track the current nesting depth.

- When we encounter `(`, increase the depth by `1`.
- When we encounter `)`, decrease the depth by `1`.
- After every character, update `result` with the maximum depth seen so far.

**Key observation:** The current depth tells us how many opening parentheses are currently unmatched. The maximum value of this depth is the answer.

## Why This Approach?

- We only need the maximum nesting depth, not to match different types of brackets.
- A counter is sufficient because the input contains only parentheses `(` and `)`.
- A stack would use extra memory unnecessarily.
- We can solve the problem in one traversal with constant extra space.

## C++ Solution

```cpp
class Solution {
public:
    int maxDepth(string s) {
        int openBrackets = 0;
        int result = 0;

        for (char &ch : s) {
            if (ch == '(') {
                openBrackets++;
            } else if (ch == ')') {
                openBrackets--;
            }

            result = max(result, openBrackets);
        }

        return result;
    }
};
```

## Dry Run

Input: `"(1+(2*3)+((8)/4))+1"`

| Character | `openBrackets` | `result` |
|---|---:|---:|
| `(` | 1 | 1 |
| `(` | 2 | 2 |
| `)` | 1 | 2 |
| `(` | 2 | 2 |
| `(` | 3 | 3 |
| `)` | 2 | 3 |
| `)` | 1 | 3 |
| `)` | 0 | 3 |

Maximum depth = **3**

*Note: The table shows the important nesting changes, not every character in the input.*

## Complexity Analysis

- **Time Complexity:** `O(n)` — We traverse the string once.
- **Space Complexity:** `O(1)` — We use only two integer variables.

## Important Points

- `openBrackets` tracks the current nesting depth.
- `result` stores the maximum depth encountered so far.
- Update `result` after processing each character.
- Ignore all characters other than `(` and `)`.
- No stack or recursion is needed.

## Pattern to Remember

**Parentheses depth problem → Counter**

Whenever a problem asks for maximum nesting depth and only the current balance matters, try a counter before using a stack.

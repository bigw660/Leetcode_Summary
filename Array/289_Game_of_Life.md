<https://leetcode.com/problems/game-of-life/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
O(MN) memory solution is very easy. How to solve this problem with O(1) memory? We can use extra values to simultanously indicate the current and next states of the cell, that is, use the
magnitude to maintain current state and use symbol to record next state. For example, if a cell turns into 0 from 1, set the new value as 2; if a cell turns into 1 from 0, set the new values 
as -1. So when we count the conditions of neighbors, we only accumulate abs(cell_val)%2, where -1 still contributes 1 but dead and 2 still contributes 0 but alive. Finally, update each cell with symbol.

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int total = 0;
                for (int di = -1; di <= 1; ++di) {
                    for (int dj = -1; dj <= 1; ++dj) {
                        if (di == 0 && dj == 0)
                            continue;
                        if (i + di < 0 || i + di >= m || j + dj < 0 || j + dj >= n) {
                            continue;
                        }
                        
                        total += (Math.abs(board[i+di][j+dj]) % 2);
                    }
                }
                
                if (board[i][j] == 1 && (total < 2 || total > 3)) {
                    board[i][j] = -1;
                }
                
                if (board[i][j] == 0 && total == 3) {
                    board[i][j] = 2;
                }
            }
        }
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] <= 0)
                    board[i][j] = 0;
                else
                    board[i][j] = 1;
            }
        }
    }
}
// Time complexity: O(MN)
// Memory complexity: O(1)
```

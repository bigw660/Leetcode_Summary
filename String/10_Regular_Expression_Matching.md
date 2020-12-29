<https://leetcode.com/problems/regular-expression-matching/>

## Python:
```python
        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
<image src="https://github.com/bigw660/Leetcode_Summary/blob/main/Images/10_Analysis.png"/>

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];
        dp[0][0] = true;
        for (int i = 1; i <= m; ++i) {
            dp[i][0] = false;
        }
        
        for (int i = 1; i <= n; ++i) {
            if (p.charAt(i - 1) == '*') {
                dp[0][i] = dp[0][i-2];
            }
            else {
                dp[0][i] = false;
            }
        }
        
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s.charAt(i-1) == p.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1];
                }
                else if (p.charAt(j-1) == '.') {
                    dp[i][j] = dp[i-1][j-1];
                }
                else if (p.charAt(j-1) == '*') {
                    if (s.charAt(i-1) == p.charAt(j-2) || p.charAt(j-2) == '.') {
                        dp[i][j] = dp[i-1][j-2] || dp[i-1][j] || dp[i][j-2];      
                    }
                    else {
                        dp[i][j] = dp[i][j-2];
                    }
                }
            }
        }
        
        return dp[m][n];
    }
}

// Time complexity: O(MN)
// Memory complexity: O(MN)
```

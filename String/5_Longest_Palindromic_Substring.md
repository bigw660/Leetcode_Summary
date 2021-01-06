<https://leetcode.com/problems/longest-palindromic-substring/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Solution 1: Dynamic Programming
Let dp[i][j] indicate if the substring s[i, j] is a valid palindromic string. After updating all the values, just return longest substring that is valid.

Some cases for initialization and update:
- dp[i][i] = true, a single letter is palindromic;
- dp[i][i+1] = true if s[i] == s[i+1] else false;
- Given a varible len >= 2, dp[i][i+len] = dp[i+1][i+len-1] && s[i] == s[i+len]

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0)
            return new String();
        boolean[][] dp = new boolean[n][n];
        for (boolean[] row : dp)
            Arrays.fill(row, false);
        int st = 0, ed = 0, max = 1;
        for (int i = 0; i < n; ++i)
            dp[i][i] = true;
        for (int i = 0; i < n - 1; ++i) {
            if (s.charAt(i) == s.charAt(i+1)) {
                dp[i][i+1] = true;
                if (max < 2) {
                    max = 2;
                    st = i;
                    ed = i + 1;
                }
            }
        }
        
        for (int len = 2; len < n; ++len) {
            for (int i = 0; i + len < n; ++i) {
                dp[i][i+len] = (dp[i+1][i+len-1]) && (s.charAt(i) == s.charAt(i+len));
                if (dp[i][i+len] && max < len + 1) {
                    max = len + 1;
                    st = i;
                    ed = i + len;
                }
            }
        }
        
        return s.substring(st, ed + 1);
        
    }
}

// Time complexity: O(n^2)
// Memory complexity: O(n^2)
```

### Solution 2: Expand Around Center
A palindromic string mirrors around its center. So a palindromic string can be expanded its center, and there are only 2n-1 such centers (including the vitual centers located 
between two letters). So just find the longest substring expanded from all centers.

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0)
            return s;
        
        int st = 0, ed = 0;
        for (int i = 0; i < n; ++i) {
            int len1 = expandFromCenter(s, i, i);
            int len2 = expandFromCenter(s, i, i+1);
            int maxLen = Math.max(len1, len2);
            
            if (maxLen > ed - st + 1) {
                st = i - (maxLen - 1) / 2;
                ed = i + maxLen / 2;
            }
        }
        
        return s.substring(st, ed + 1);
    }
    
    public int expandFromCenter(String s, int l, int r) {
        int n = s.length();
        while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
            l --;
            r ++;
        }
        
        return r - l - 1;
    }
}

// Time complexity: O(n^2)
// Memory complexity: O(1)
```

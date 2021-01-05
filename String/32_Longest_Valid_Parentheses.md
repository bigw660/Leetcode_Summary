<https://leetcode.com/problems/longest-valid-parentheses/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Solution 1: Stack
Instead of finding every possible string and checking its validity, we can make use of stack while scanning the given string to check if the string scanned so far is valid, 
and also the length of the longest valid string. In order to do so, we start by pushing -1 onto the stack.

For every '(' encountered, push its index into the stack;

For every ')' encountered, pop one element from the stack. If the stack is not empty, it indicates that a '(' is paired with current ')' and the string between this pair is valid.
So subtract the current index from the top element of the stack to get the length of the currently encountered valid string. If the stack becames empty after popping the element,
push current index to the stack.

In this, keep calculating the lengths of valid substrings and return the largest value.

```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        Stack<Integer> st = new Stack<>();
        st.push(-1);
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '(') {
                st.push(i);
            }
            else {
                st.pop();
                if (st.isEmpty()) {
                    st.push(i);
                }
                else {
                    max = Math.max(max, i - st.peek());
                }
            }
        }
        
        return max;
    }
}

// Time complexity: O(n)
// Memory complexity: O(n)
```

### Solution 2: Dynamic Programming
Keep dp[i] as the longest length of the valid substring ending at i-th index. So there are some cases:

- s[i] == '(', dp[i] = 0;
- s[i] == ')' and s[i-1] == '(', dp[i] = dp[i-2] + 2;
- s[i] == ')' and s[i-1] == ')', it needs to check if s[i-dp[i-1]-1] == '('. If yes, we find a valid '(' for current ')', so dp[i] = dp[i-1] + dp[i-dp[i-1]-2] + 2; otherwise,
dp[i] = 0;

```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        int n = s.length();
        int[] dp = new int[n];
        
        for (int i = 1; i < n; ++i) {
            if (s.charAt(i) == ')') {
                if (s.charAt(i-1) == '(') {
                    dp[i] = (i >= 2? dp[i-2] : 0) + 2;
                }
                else {
                    if (i - dp[i-1] - 1 >= 0 && s.charAt(i-dp[i-1]-1) == '(') {
                        dp[i] = dp[i-1] + 2 + (i - dp[i-1] - 2 >= 0? dp[i-dp[i-1]-2] : 0);
                    }
                }
                max = Math.max(max, dp[i]);
            }
        }
        
        return max;
    }
}

// Time complexity: O(n)
// Memory complexity: O(n)
```

### Solution 3: Scan Twice
This is a very tricky solution. Just scan the string twice, one time from left to right and one time from right to left.

Initialize two variable l and r as 0. When scanning the string from left to right, accumulate l by 1 if encountering '(' else accumulate r by 1. If l == r, it means a valid string
is formed, update the solution if l + r is bigger. If r > l, it means the currently scanned substring is invalid, reset l = r = 0.

Do the similar way from right to left, but change the conditon as l > r to reset varibles. Return the longest length.

```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        int n = s.length();
        int l = 0, r = 0;
        
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '(') {
                l ++;
            }    
            else {
                r ++;
            }
            
            if (l == r) {
                max = Math.max(l + r, max);
            }
            
            if (r > l) {
                l = r = 0;
            }
        }
        
        l = r = 0; // reset the varibles
        for (int i = n - 1; i >= 0; --i) {
            if (s.charAt(i) == '(') {
                l ++;
            }
            else {
                r ++;
            }
            
            if (l == r) {
                max = Math.max(max, l + r);
            }
            
            if (l > r) {
                l = r = 0;
            }
        }
        
        return max;
    }
}

// Time complexity: O(n)
// Memory complexity: O(1)
```

<https://leetcode.com/problems/counting-bits/>
## Problem
![Problem](https://github.com/bigw660/Leetcode_Summary/blob/main/Images/338.png)

## Analysis
![Analysis](https://github.com/bigw660/Leetcode_Summary/blob/main/Images/338_Analysis.png)

## Python 
```python
class Solution:
    def countBits(self, num: int) -> List[int]:
        dp = [0 for _ in range(num+1)]
        for i in range(num + 1):
            dp[i] = dp[i >> 1] + (i & 1)
        return dp
```

## Java

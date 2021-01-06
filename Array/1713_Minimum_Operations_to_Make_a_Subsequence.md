<https://leetcode.com/problems/minimum-operations-to-make-a-subsequence/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Solution 1: Longest Common Subsequence
Intuitively, this problem is a variation of longest common subsequence problem. Just find the length of LCS between two arrays, and subtract such length from the length of **_target_**
array to get the solution. LCS problem is a classic problem that can be solved by dynamic programming.

```java
class Solution {
    public int minOperations(int[] target, int[] arr) {
        int m = target.length, n = arr.length;
        int[][] dp = new int[m+1][n+1];
        
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (target[i-1] == arr[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        
        return m - dp[m][n];
    }
}

// Time complexity: O(mn)
// Memory complexity: O(mn)
// Since the constraint for this problem is 1 <= target.length, arr.length <= 10^5, it would be TLE and MLE for very long array input.
```

### Solution 2: Longest Increasing Subsequence
Since this problem states that the **_target_** array contains no duplicates, it can be transformed into the longest increasing subsequence problem. In detail, we can map the **_target_** array
into a strictly increasing array. Then update the elements in **_arr_** array based on the mapping table. Thus, this problem turns into finding the longest increasing subsequence in the
new **_arr_** array. Here we directly use the code from [problem 300](https://github.com/bigw660/Leetcode_Summary/blob/main/Array/300_Longest_Increasing_Subsequence.md).

```java
class Solution {
    public int minOperations(int[] target, int[] arr) {
        int m = target.length, n = arr.length;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < m; ++i) {
            map.put(target[i], i + 1);
        }
        
        for (int i = 0; i < n; ++i) {
            if (map.containsKey(arr[i])) {
                arr[i] = map.get(arr[i]);
            }
            else {
                arr[i] = -1;
            }
        }
        
        return m - lengthOfLIS(arr);
    }
    
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        int ed = 0, res = 0;
        for (int i : nums) {
            if (i == -1)
                continue;
            int idx = binarySearch(dp, i, 0, ed);
            if (idx == -1) {
                dp[ed++] = i;
                res = Math.max(res, ed);
            }
            else {
                dp[idx] = i;
                res = Math.max(res, idx);
            }
        }
        
        return res;
    }
    
    // find the left-most element e where e >= target,
    // otherwise return -1 if no such element existed
    public int binarySearch(int[] nums, int target, int l, int r) {
        int left = l, right = r;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        
        if (left > r) {
            return -1;
        }
        return left;
    }
}

// Time complexity: O(m + n + n log n) = O(m + n log n)
// Memory complexity: O(m + n)
// The time and memory complexity is allowed for this problem.
```

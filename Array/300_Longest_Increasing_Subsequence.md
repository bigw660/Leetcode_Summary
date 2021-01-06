<https://leetcode.com/problems/longest-increasing-subsequence/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Solution 1: Dynamic Programming
Keep dp[i] as the longest length of the strictly increasing subsequence ending at i-th index. So dp[0] = 1; dp[i] = max(dp[j]) + 1 where 0 <= j < i and nums[j] < nums[i].

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        
        int max = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            max = Math.max(max, dp[i]);
        }
        
        return max;
    }
}

// Time complexity: O(n^2)
// Memory complexity: O(n)
```

### Solution 2: Dynamic Programming + Binary Search
Take the dp array to store the increasing subsequence formed by including the elements currently encountered. Maintain a varible ed to record the current tail of the subsequence.
Initially, ed = 0. For each encountered element, use binary search to quickly find the smallest element that is no less than the current element in the dp array. If such element does
not exist, it means current element is bigger than all previous elements, just put such element at the tail position of dp array and move forward tail pointer ed by 1; Otherwise,
just put the current element at the found index of dp array. Once updated the dp array, update the maximum length of currently found increasing subsequence.

Here is a demo for the process. For example, the nums array is [10, 9, 2, 5, 3, 7, 101, 18]. Initialize ed = 0, max = 1, dp = [-10001] * n.
- search >=10 in dp, return -1. dp[ed] = dp[0] = 10, ed = 1. dp: [10], max = 1.
- search >=9 in dp, return 0. dp[0] = 9, ed = 1. dp: [9], max = 1.
- search >=2 in dp, return 0. dp[0] = 2, ed = 1. dp: [2], max = 1.
- search >=5 in dp, return -1. dp[ed] = dp[1] = 5, ed = 2. dp: [2, 5], max = 2.
- search >=3 in dp, return 1. dp[1] = 3, ed = 2. dp: [2, 3], max = 2.
- search >=7 in dp, return -1. dp[ed] = dp[2] = 7, ed = 3. dp: [2, 3, 7], max = 3.
- search >=101 in dp, return -1. dp[ed] = dp[3] = 101, ed = 4. dp: [2, 3, 7, 101], max = 4.
- search >=18 in dp, return 3. dp[3] = 18, ed = 4. dp: [2, 3, 7, 18], max = 4.
- Done! Return 4.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, -10001);
        int ed = 0, res = 0;
        for (int i : nums) {
            int idx = binarySearch(dp, i, 0, ed);
            System.out.println(idx);
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

// Time complexity: O(n log n)
// Memory complexity: O(n)
```

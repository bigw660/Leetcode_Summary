<https://leetcode-cn.com/problems/sliding-window-maximum/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
Maintain a monotonic decreasing doubly-end queue to record the left-most maximum value for each window. If we find an index j where i < j && nums[i] <= nums[j], as long as index i is inside the
window, then j must be inside the window. Thus, we can remove i from the queue and keep j. With this queue, the left-end of the queue should be the largest element in the window. In
additon, it also needs to poll the left-end element if such element is deleted from the window.
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> dq = new LinkedList<>();
        for (int i = 0; i < k; ++i) {
            while (!dq.isEmpty() && nums[dq.peekLast()] <= nums[i]) {
                dq.pollLast();
            }
            dq.offerLast(i);
        }
        int n = nums.length;
        int[] res = new int[n - k + 1];
        res[0] = nums[dq.peekFirst()];
        for (int i = 0; i < n - k; ++i) {
            int del = i, add = k + i;
            if (del == dq.peekFirst()) {
                dq.pollFirst();
            }
            while (!dq.isEmpty() && nums[dq.peekLast()] <= nums[add]) {
                dq.pollLast();
            }
            dq.offerLast(add);
            res[i + 1] = nums[dq.peekFirst()];
        }

        return res;
    }
}

// Time complexity: O(n)
// Memory complexity: O(k)
```

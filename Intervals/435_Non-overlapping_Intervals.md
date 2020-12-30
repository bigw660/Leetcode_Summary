<https://leetcode-cn.com/problems/non-overlapping-intervals/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
Greedy thought: always keep the smaller right-bound when the new interval is overlapped with previous intervals. So two steps:
  - Sort the array with increasing order of left-bound.
  - Initialize the end_val as the right-bound of the first interval. Traverse the array, if the current interval is overlapped with the end_val, update the end_val with the smaller
  value between end_val and right-bound, and add 1 to the solution. Otherwise, update the end_val with the current right-bound.
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0)
            return 0;
        Arrays.sort(intervals, (a, b) -> a[0] == b[0]? a[1] - b[1] : a[0] - b[0]);
        int ed = intervals[0][1];
        int res = 0;
        for (int i = 1; i < intervals.length; ++i) {
            if (intervals[i][0] < ed) {
                ed = Math.min(ed, intervals[i][1]);
                res ++;
            }
            else {
                ed = intervals[i][1];
            }
        }

        return res;
    }
}

// Time complexity: O(nlogn)
// Memory complexity: O(logn)
```
